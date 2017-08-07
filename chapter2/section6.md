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



