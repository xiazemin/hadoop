# 运行streaming

[http://hadoop.apache.org/docs/r1.0.4/cn/streaming.html](http://hadoop.apache.org/docs/r1.0.4/cn/streaming.html)

$   hadoop jar share/hadoop/tools/lib/hadoop-streaming-2.6.4.jar -input /input/input1.txt -output /output1 -mapper /bin/cat -reducer /bin/wc

