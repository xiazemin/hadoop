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





