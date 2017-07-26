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

![](/assets/import1.png)[http://localhost:8042/node](http://localhost:8042/node)

![](/assets/import2.png)[http://localhost:8088/cluster/nodes](http://localhost:8088/cluster/nodes)

![](/assets/import3.png)$ hadoop fs -du

17/07/26 19:00:56 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

du: \`.': No such file or directory

这个问题基本上是由于在apache hadoop官网上下载的hadoopXXX.bin.tar.gz实在32位的机器上编译的

 cd lib/native/

$ file libhadoop.so.1.0.0

libhadoop.so.1.0.0: ELF 64-bit LSB shared object, x86-64, version 1 \(SYSV\), dynamically linked, not stripped

