# hdfs写入文件失败

$ hdfs dfs -put  /Users/didi/java/WordCount/input/input1.txt /user/didi/input

```
at org.apache.hadoop.hdfs.DFSOutputStream$DataStreamer.run\(DFSOutputStream.java:588\)
```

put: File /user/didi/input/input1.txt.\_COPYING\_ could only be replicated to 0 nodes instead of minReplication \(=1\).  There are 0 datanode\(s\) running and no node\(s\) are excluded in this operation.

$ jps

16840 NameNode

17002 SecondaryNameNode

2362

17180 NodeManager

17805 Jps

637 Main

17103 ResourceManager

$ vi etc/hadoop/hdfs-site.xml

增加



&lt;property&gt;

&lt;name&gt;dfs.namenode.name.dir &lt;/name&gt;

&lt;value&gt;file:///data/hdfs/name&lt;/value&gt;

&lt;/property&gt;

&lt;property&gt;

&lt;name&gt;dfs.datanode.data.dir&lt;/name&gt;

&lt;value&gt;file:///data/hdfs/data&lt;/value&gt;

&lt;/property&gt;

