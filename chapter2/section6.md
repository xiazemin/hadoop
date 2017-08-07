# 运行eclipse实例

2017-08-07 11:09:10,570 INFO  \[main\] mapreduce.JobSubmitter \(JobSubmitter.java:submitJobInternal\(250\)\) - Cleaning up the staging area file:/tmp/hadoop-didi/mapred/staging/didi619393345/.staging/job\_local619393345\_0001

Exception in thread "main" org.apache.hadoop.mapreduce.lib.input.InvalidInputException: Input path does not exist: file:/Users/didi/java/WordCount/input

$     hdfs dfs -mkdir java/WordCount/input  x

$mkdir /Users/didi/java/WordCount/input √

Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/hadoop/yarn/util/Apps

```
at java.lang.ClassLoader.defineClass1\(Native Method\)
解决：没有把yarn下的包以及yarn 下的lib目录下的包导入
```

build path/configbuildpath  把share/hadoop/yarn 目录下的jar包引入即可

2017-08-07 11:26:16,564 INFO  \[main\] mapreduce.Job \(Job.java:monitorAndPrintJob\(1374\)\) -  map 0% reduce 100%

2017-08-07 11:26:16,565 INFO  \[main\] mapreduce.Job \(Job.java:monitorAndPrintJob\(1385\)\) - Job job\_local1158256095\_0001 completed successfully

2017-08-07 11:26:16,573 INFO  \[main\] mapreduce.Job \(Job.java:monitorAndPrintJob\(1392\)\) - Counters: 24

	File System Counters

		FILE: Number of bytes read=22

		FILE: Number of bytes written=251569

		FILE: Number of read operations=0

		FILE: Number of large read operations=0

		FILE: Number of write operations=0

	Map-Reduce Framework

		Combine input records=0

		Combine output records=0

		Reduce input groups=0

		Reduce shuffle bytes=0

		Reduce input records=0

		Reduce output records=0

		Spilled Records=0

		Shuffled Maps =0

		Failed Shuffles=0

		Merged Map outputs=0

		GC time elapsed \(ms\)=0

		Total committed heap usage \(bytes\)=163053568

	Shuffle Errors

		BAD\_ID=0

		CONNECTION=0

		IO\_ERROR=0

		WRONG\_LENGTH=0

		WRONG\_MAP=0

		WRONG\_REDUCE=0

	File Output Format Counters 

		Bytes Written=8

