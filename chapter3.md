# streaming

[http://hadoop.apache.org/docs/r1.0.4/cn/streaming.html](http://hadoop.apache.org/docs/r1.0.4/cn/streaming.html)

$ ${HADOOP\_HOME}/bin/hadoop jar ${HADOOP\_HOME}/share/hadoop/tools/lib/hadoop-streaming-2.6.0.jar \

```
-input &lt;输入目录&gt; \ \# 可以指定多个输入路径，例如：-input '/user/foo/dir1' -input '/user/foo/dir2'

-inputformat &lt;输入格式 JavaClassName&gt; \

-output &lt;输出目录&gt; \

-outputformat &lt;输出格式 JavaClassName&gt; \

-mapper &lt;mapper executable or JavaClassName&gt; \

-reducer &lt;reducer executable or JavaClassName&gt; \

-combiner &lt;combiner executable or JavaClassName&gt; \

-partitioner &lt;JavaClassName&gt; \

-cmdenv &lt;name=value&gt; \ \# 可以传递环境变量，可以当作参数传入到任务中，可以配置多个

-file &lt;依赖的文件&gt; \ \# 配置文件，字典等依赖

-D &lt;name=value&gt; \ \# 作业的属性配置
```

$ hadoop jar share/hadoop/tools/lib/hadoop-streaming-2.6.4.jar -input input -output outputStream1 -mapper /bin/cat -reducer /bin/wc

$HADOOP\_HOME/bin/hadoop  jar $HADOOP\_HOME/hadoop-streaming.jar \

```
-input myInputDirs \

-output myOutputDir \

-mapper myPythonScript.py \

-reducer /bin/wc \

-file myPythonScript.py \

-file myDictionary.txt
```



