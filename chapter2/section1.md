# 问题及解决方案

The import org.apache.hadoop.mapreduce cannot be resolved

引入hadoop插件jar文件只是可以开发hadoop工程，但是hadoop的库文件必须引入

1.zkpk--&gt;hadoop-2.5.2--&gt;share--&gt;comom三个包

2 zkpk--&gt;hadoop-2.5.2--&gt;share--&gt;hdfs下三个包

3 zkpk--&gt;hadoop-2.5.2--&gt;share--&gt;mapreduce三个包

4 zkpk--&gt;Hadoop-2.5.2--&gt;share--&gt;Yarn三个包

建立Jar包软连接

第一次:

Libraries--&gt;Add Library--&gt;User Library --&gt;

next--&gt; UserLibrary\(创建名称\)--&gt;

New\(填写名字\)--&gt;OK--&gt;Add External JARs..--&gt;

导入所需Jar包

使用:

右击项目 Library--&gt;导入之前所建立的包名即可

hadoop类库文件路径

/zkpk/resource/software/hadoop/apache/hadoop-2.5.2-src

简化：

hadoop-2.7.2/share/hadoop/mapreduce下的所有jar包（子文件夹下的jar包不用）

hadoop-2.7.2/share/hadoop/common下的hadoop-common-2.7.2.jar

hadoop-2.7.2/share/hadoop/common/lib下的commons-cli-1.2.jar



