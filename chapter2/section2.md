# 第一个MapReduce程序

将上面的程序打包成jar包导出，这里为了方便我直接放到了跟目录下

设置输入目录，这时候我们看源程序的主函数，其中有两行:

FileInputFormat.addInputPath\(job, new Path\(args\[0\]\)\);

FileOutputFormat.setOutputPath\(job, new Path\(args\[1\]\)\);

1

2

1

2

说明之行这个WordCount类需要两个参数，一个输入目录一个输出目录，输出目录实现不能被创建。这里我还是在跟目录中创建了一个输入目录,并随便扔了一个README.md进去。



    \#这条命令用于在本地文件系统创建目录

    hadoop fs -mkdir input

启动命令行运行程序

    \#这个命令是 jar &lt;jar&gt; \[class\] \[args\]的形式

    \#注意:

    \#1.这里之所以是wordcount.jar是因为我把它放到跟目录在，如果不再跟目录，则需要给出路径。

    \#2.\[class\]中是全局命名，即写出了包名

    hadoop jar wordcount.jar wordcount.WordCount input output

最后可以在output中看到\_SUCCESS和part-r-00000两个文件，其中打开part-r-00000可以看到如下结果:

output



说明这个程序完成了”字数统计”的工作。说明环境搭建完成并且你也运行了第一个mapreduce程序。

