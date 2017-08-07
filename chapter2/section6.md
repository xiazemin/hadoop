# 运行eclipse实例

2017-08-07 11:09:10,570 INFO  \[main\] mapreduce.JobSubmitter \(JobSubmitter.java:submitJobInternal\(250\)\) - Cleaning up the staging area file:/tmp/hadoop-didi/mapred/staging/didi619393345/.staging/job\_local619393345\_0001

Exception in thread "main" org.apache.hadoop.mapreduce.lib.input.InvalidInputException: Input path does not exist: file:/Users/didi/java/WordCount/input

$     hdfs dfs -mkdir java/WordCount/input

