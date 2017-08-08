# 运行streaming

[http://hadoop.apache.org/docs/r1.0.4/cn/streaming.html](http://hadoop.apache.org/docs/r1.0.4/cn/streaming.html)

$   hadoop jar share/hadoop/tools/lib/hadoop-streaming-2.6.4.jar -input /input/input1.txt -output /output1 -mapper /bin/cat -reducer /bin/wc

Cannot run program "/bin/wc": error=2, No such file or directory

$     which wc

/usr/bin/wc

$   hadoop jar share/hadoop/tools/lib/hadoop-streaming-2.6.4.jar -input /input/input1.txt -output /output1 -mapper /bin/cat -reducer /usr/bin/wc

17/08/08 10:25:50 INFO mapred.LocalJobRunner: 1 / 1 copied.

17/08/08 10:25:50 INFO mapred.Task: Task attempt\_local1549656460\_0001\_r\_000000\_0 is allowed to commit now

17/08/08 10:25:50 INFO output.FileOutputCommitter: Saved output of task 'attempt\_local1549656460\_0001\_r\_000000\_0' to hdfs://localhost:8020/output2/\_temporary/0/task\_local1549656460\_0001\_r\_000000

17/08/08 10:25:50 INFO mapred.LocalJobRunner: Records R/W=1/1 &gt; reduce

17/08/08 10:25:50 INFO mapred.Task: Task 'attempt\_local1549656460\_0001\_r\_000000\_0' done.

17/08/08 10:25:50 INFO mapred.LocalJobRunner: Finishing task: attempt\_local1549656460\_0001\_r\_000000\_0

17/08/08 10:25:50 INFO mapred.LocalJobRunner: reduce task executor complete.

17/08/08 10:25:50 INFO mapreduce.Job: Job job\_local1549656460\_0001 running in uber mode : false

17/08/08 10:25:50 INFO mapreduce.Job:  map 100% reduce 100%

17/08/08 10:25:50 INFO mapreduce.Job: Job job\_local1549656460\_0001 completed successfully

17/08/08 10:25:50 INFO mapreduce.Job: Counters: 35

$ hdfs dfs -lsr /output2/

lsr: DEPRECATED: Please use 'ls -R' instead.

17/08/08 10:27:24 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

-rw-r--r--   1 didi supergroup          0 2017-08-08 10:25 /output2/\_SUCCESS

-rw-r--r--   1 didi supergroup         26 2017-08-08 10:25 /output2/part-00000

$ hdfs dfs -cat  /output2/part-00000

17/08/08 10:27:40 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

```
   1       2      13
```



