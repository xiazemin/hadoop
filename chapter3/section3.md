# 问题

$ hdfs dfs -ls

17/08/07 21:44:56 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

ls: Call From bogon/172.22.29.98 to localhost:8020 failed on connection exception: java.net.ConnectException: Connection refused; For more details see:  [http://wiki.apache.org/hadoop/ConnectionRefused](http://wiki.apache.org/hadoop/ConnectionRefused)

$ hadoop namenode -format 仍然无法解决

配置文件目录有问题$ vi etc/hadoop/hdfs-site.xml

&lt;property&gt;

&lt;name&gt;dfs.namenode.name.dir &lt;/name&gt;

&lt;value&gt;/Users/didi/hadoop/tmp/name&lt;/value&gt;

&lt;/property&gt;

&lt;property&gt;

&lt;name&gt;dfs.datanode.data.dir&lt;/name&gt;

&lt;value&gt;/Users/didi/hadoop/tmp/data&lt;/value&gt;

&lt;/property&gt;

$ sbin/stop-all.sh

$ jps

19525 SecondaryNameNode

25205 NodeManager

24983 DataNode

2362

25243 Jps

19628 ResourceManager

637 Main

namenode没有启动

$ hadoop namenode -format

$ sbin/start-all.sh

$ jps

26016 ResourceManager

26096 NodeManager

25816 DataNode

25912 SecondaryNameNode

2362

26154 Jps

637 Main

25742 NameNode

bogon:hadoop didi$ vi etc/hadoop/hdfs-site.xml

成功

$  hdfs dfs -mkdir /input

$ hdfs dfs -lsr /

drwxr-xr-x   - didi supergroup          0 2017-08-08 10:14 /input

$ hdfs dfs -put /Users/didi/java/WordCount/input/\*.txt  /input

$ hdfs dfs -lsr /

lsr: DEPRECATED: Please use 'ls -R' instead.

17/08/08 10:16:15 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

drwxr-xr-x   - didi supergroup          0 2017-08-08 10:16 /input

-rw-r--r--   1 didi supergroup         12 2017-08-08 10:16 /input/input1.txt

-rw-r--r--   1 didi supergroup         13 2017-08-08 10:16 /input/input2.txt

