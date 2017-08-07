# 运行实例

$ hadoop version

Hadoop 2.6.4

Subversion https://git-wip-us.apache.org/repos/asf/hadoop.git -r 5082c73637530b0b7e115f9625ed7fac69f937e6

Compiled by jenkins on 2016-02-12T09:45Z

$ hadoop jar /opt/hadoop-2.6.4/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.4.jar grep input output 'dfs\[a-z.\]+'

org.apache.hadoop.mapreduce.lib.input.InvalidInputException: Input path does not exist: hdfs://localhost:8020/user/didi/input

$  hdfs dfs -ls input

ls: \`input': No such file or directory

$  hdfs dfs -mkdir input

$  hdfs dfs -ls

Found 1 items

drwxr-xr-x   - didi supergroup          0 2017-08-07 10:28 input

$ hdfs dfs -put etc/hadoop/\*.xml input

$ hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.4.jar grep input output 'dfs\[a-z.\]+'

$  hdfs dfs -ls

17/08/07 10:31:49 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

Found 2 items

drwxr-xr-x   - didi supergroup          0 2017-08-07 10:29 input

drwxr-xr-x   - didi supergroup          0 2017-08-07 10:29 output

$  hdfs dfs -ls output

17/08/07 10:32:04 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

Found 2 items

-rw-r--r--   1 didi supergroup          0 2017-08-07 10:29 output/\_SUCCESS

-rw-r--r--   1 didi supergroup          0 2017-08-07 10:29 output/part-r-00000

