# 问题及解决方案

$ bin/hadoop dfs -ls

DEPRECATED: Use of this script to execute hdfs command is deprecated.

Instead use the hdfs command for it.

$ bin/hadoop hdfs -ls

错误: 找不到或无法加载主类 hdfs

管理界面：[http://localhost:8088](http://localhost:8088)

NameNode界面：[http://localhost:50070](http://localhost:50070)

HDFS NameNode界面：[http://localhost:8042](http://localhost:8042)

[http://localhost:8020/](http://localhost:8020/)

It looks like you are making an HTTP request to a Hadoop IPC port. This is not the correct port for the web interface on this daemon.

[http://localhost:50070/dfshealth.html\#tab-overview](http://localhost:50070/dfshealth.html#tab-overview)

![](/assets/import1.png)http://localhost:8042/node

![](/assets/import2.png)http://localhost:8088/cluster/nodes

![](/assets/import3.png)

