# 问题

$ hdfs dfs -ls

17/08/07 21:44:56 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

ls: Call From bogon/172.22.29.98 to localhost:8020 failed on connection exception: java.net.ConnectException: Connection refused; For more details see:  http://wiki.apache.org/hadoop/ConnectionRefused

$ hadoop namenode -format 仍然无法解决



