# php   hadoop streaming

Hadoop流

虽然Hadoop是用Java写的，但是hadoop提供了Hadoop流，Hadoop流提供一个API, 允许用户使用任何语言编写map函数和reduce函数.

Hadoop流动关键是，它使用UNIX标准流作为程序与Hadoop之间的接口。因此，任何程序只要可以从标准输入流中读取数据，并且可以把数据写入标准输出流中，那么就可以通过Hadoop流使用任何语言编写MapReduce程序的map函数和reduce函数。

例如：bin/hadoop jar contrib/streaming/hadoop-streaming-0.20.203.0.jar -mapper /usr/local/hadoop/mapper.PHP -reducer /usr/local/hadoop/reducer.php -input test/\* -output out4

Hadoop流引入的包：hadoop-streaming-0.20.203.0.jar,Hadoop根目录下是没有hadoop-streaming.jar的，因为streaming是一个contrib，所以要去contrib下面找，以hadoop-0.20.2为例，它在这里：

-input：指明输入hdfs文件的路径

-output：指明输出hdfs文件的路径

-mapper：指明map函数

-reducer：指明reduce函数

mapper函数

mapper.php文件，写入如下代码：

\#!/usr/local/php/bin/php  

&lt;?php  

$word2count = array\(\);  

// input comes from STDIN \(standard input\)  

// You can this code :$stdin = fopen\(“php://stdin”, “r”\);  

while \(\($line = fgets\(STDIN\)\) !== false\) {  

    // remove leading and trailing whitespace and lowercase  

    $line = strtolower\(trim\($line\)\);  

    // split the line into words while removing any empty string  

    $words = preg\_split\('/\W/', $line, 0, PREG\_SPLIT\_NO\_EMPTY\);  

    // increase counters  

    foreach \($words as $word\) {  

        $word2count\[$word\] += 1;  

    }  

}  

// write the results to STDOUT \(standard output\)  

// what we output here will be the input for the  

// Reduce step, i.e. the input for reducer.py  

foreach \($word2count as $word =&gt; $count\) {  

    // tab-delimited  

    echo $word, chr\(9\), $count, PHP\_EOL;  

}  

?&gt;  

这段代码的大致意思是：把输入的每行文本中的单词找出来，并以”

             hello    1

             world  1″

这样的形式输出出来。

和之前写的PHP基本没有什么不同，对吧，可能稍微让你感到陌生有两个地方：

PHP作为可执行程序

第一行的

\#!/usr/local/php/bin/php  

告诉linux，要用\#!/usr/local/php/bin/php这个程序作为以下代码的解释器。写过linux shell的人应该很熟悉这种写法了，每个shell脚本的第一行都是这样: \#!/bin/bash, \#!/usr/bin/python

有了这一行，保存好这个文件以后，就可以像这样直接把mapper.php当作cat, grep一样的命令执行了：./mapper.php

使用stdin接收输入

PHP支持多种参数传入的方法，大家最熟悉的应该是从$\_GET, $\_POST超全局变量里面取通过Web传递的参数，次之是从$\_SERVER\['argv'\]里取通过命令行传入的参数，这里，采用的是标准输入stdin

它的使用效果是：

在Linux控制台输入 ./mapper.php

mapper.php运行，控制台进入等候用户键盘输入状态

用户通过键盘输入文本

用户按下Ctrl + D终止输入，mapper.php开始执行真正的业务逻辑，并将执行结果输出

那么stdout在哪呢？print本身已经就是stdout啦，跟我们以前写web程序和CLI脚本没有任何不同。

reducer函数

创建reducer.php文件，写入如下代码：

\#!/usr/local/php/bin/php  

&lt;?php  

$word2count = array\(\);  

// input comes from STDIN  

while \(\($line = fgets\(STDIN\)\) !== false\) {  

    // remove leading and trailing whitespace  

    $line = trim\($line\);  

    // parse the input we got from mapper.php  

    list\($word, $count\) = explode\(chr\(9\), $line\);  

    // convert count \(currently a string\) to int  

    $count = intval\($count\);  

    // sum counts  

    if \($count &gt; 0\) $word2count\[$word\] += $count;  

}  

// sort the words lexigraphically  

//  

// this set is NOT required, we just do it so that our  

// final output will look more like the official Hadoop  

// word count examples  

ksort\($word2count\);  

// write the results to STDOUT \(standard output\)  

foreach \($word2count as $word =&gt; $count\) {  

    echo $word, chr\(9\), $count, PHP\_EOL;  

}  

?&gt;  

这段代码的大意是统计每个单词出现了多少次数，并以”

hello   2

world  1″

这样的形式输出

用Hadoop来运行

把文件放入 Hadoop 的 DFS 中：

bin/hadoop dfs -put test.log test

执行 php 程序处理这些文本\(以Streaming方式执行PHP mapreduce程序:\):

bin/hadoop jar contrib/streaming/hadoop-streaming-0.20.203.0.jar -mapper /usr/local/hadoop/mapper.php -reducer /usr/local/hadoop/reducer.php -input test/\* -output out

注意：

1\) input和output目录是在hdfs上的路径

2\) mapper和reducer是在本地机器的路径，一定要写绝对路径，不要写相对路径，以免到时候hadoop报错说找不到mapreduce程序

3 \) mapper.php 和 reducer.php 必须复制到所有 DataNode 服务器上的相同路径下, 所有的服务器都已经安装php.且安装路径一样.

查看结果

bin/hadoop d fs -cat /tmp/out/part-00000

