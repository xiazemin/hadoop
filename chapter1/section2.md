# 单节点



对我这个之前从未接触过\*nix的用户来说，使用命令行来做一系列的事情还是废了一番功夫。特写这个记录，以做备份。



 



获取Java



我的Mac运行的操作系统是OS X 10.7 Lion，之前已经安装过Java了，可以在实用工具-&gt;终端中使用java -version命令来确认java的版本。如果没有安装java，也可以进入下面网址下载：http://support.apple.com/kb/dl1421。



 



获取Hadoop



具体的地址自己百度吧。我下载的是1.0.4的stable版本。



下载完之后解压缩，我这里放置的目录是/users/Billy/Hadoop。



 



设置环境变量



在实际启动Hadoop之前，有三个文件需要进行配置。



但在这之前，我们需要设置一下几个类似Windows的环境变量，方便以后在命令行敲命令。



export HADOOP\_HOME=/users/billy/hadoop



export PATH=$PATH:$HADOOP\_HOME/bin



 



配置hadoop-env.sh



在Hadoop-&gt;conf目录下，找到hadoop-env.sh，打开编辑进行如下设置：



export JAVA\_HOME=/library/Java/Home（去掉注释）



export HADOOP\_HEAPSIZE=2000（去掉注释）



export HADOOP\_OPTS="-Djava.security.krb5.realm=OX.AC.UK -Djava.security.krb5.kdc=kdc0.ox.ac.uk:kdc1.ox.ac.uk"（去掉注释） 



注意第三个配置在OS X上最好进行配置，否则会报“Unable to load realm info from SCDynamicStore”。



 



配置core-site.xml



复制代码



&lt;configuration&gt;



  &lt;property&gt;



    &lt;name&gt;hadoop.tmp.dir&lt;/name&gt;



    &lt;value&gt;/users/billy/hadoop/tmp/hadoop-${user.name}&lt;/value&gt;



    &lt;description&gt;A base for other temporary directories.&lt;/description&gt;



  &lt;/property&gt;



  &lt;property&gt;



    &lt;name&gt;fs.default.name&lt;/name&gt;



    &lt;value&gt;hdfs://localhost:8020&lt;/value&gt;



  &lt;/property&gt;



&lt;/configuration&gt; 



复制代码



配置hdfs-site.xml



复制代码

&lt;configuration&gt;

    &lt;property&gt;

        &lt;name&gt;dfs.replication&lt;/name&gt;

        &lt;value&gt;1&lt;/value&gt;

    &lt;/property&gt;

&lt;/configuration&gt; 



复制代码

配置mapred-site.xml



复制代码

 &lt;configuration&gt;



    &lt;property&gt;

        &lt;name&gt;mapred.job.tracker&lt;/name&gt;

        &lt;value&gt;localhost:8021&lt;/value&gt;

    &lt;/property&gt;

    

    &lt;property&gt;

        &lt;name&gt;mapred.tasktracker.map.tasks.maximum&lt;/name&gt;

        &lt;value&gt;2&lt;/value&gt;

    &lt;/property&gt;

    

    &lt;property&gt;

        &lt;name&gt;mapred.tasktracker.reduce.tasks.maximum&lt;/name&gt;

        &lt;value&gt;2&lt;/value&gt;

    &lt;/property&gt;

&lt;/configuration&gt;

复制代码

 



安装HDFS



经过以上的配置，就可以进行HDFS的安装了。



$HADOOP\_HOME/bin/hadoop name node -format



如果顺利的话，会得到类似如下的输出：



 BillymatoMacBook-Air:hadoop Billy$ $HADOOP\_HOME/bin/hadoop namenode -format



Warning: $HADOOP\_HOME is deprecated.





12/12/02 17:11:12 INFO namenode.NameNode: STARTUP\_MSG: 



/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*



STARTUP\_MSG: Starting NameNode



STARTUP\_MSG:   host = BillymatoMacBook-Air.local/192.168.1.102



STARTUP\_MSG:   args = \[-format\]



STARTUP\_MSG:   version = 1.0.4



STARTUP\_MSG:   build = https://svn.apache.org/repos/asf/hadoop/common/branches/branch-1.0 -r 1393290; compiled by 'hortonfo' on Wed Oct  3 05:13:58 UTC 2012



\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/



12/12/02 17:11:12 INFO util.GSet: VM type       = 64-bit



12/12/02 17:11:12 INFO util.GSet: 2% max memory = 39.9175 MB



12/12/02 17:11:12 INFO util.GSet: capacity      = 2^22 = 4194304 entries



12/12/02 17:11:12 INFO util.GSet: recommended=4194304, actual=4194304



12/12/02 17:11:12 INFO namenode.FSNamesystem: fsOwner=Billy



12/12/02 17:11:12 INFO namenode.FSNamesystem: supergroup=supergroup



12/12/02 17:11:12 INFO namenode.FSNamesystem: isPermissionEnabled=true



12/12/02 17:11:12 INFO namenode.FSNamesystem: dfs.block.invalidate.limit=100



12/12/02 17:11:12 INFO namenode.FSNamesystem: isAccessTokenEnabled=false accessKeyUpdateInterval=0 min\(s\), accessTokenLifetime=0 min\(s\)



12/12/02 17:11:13 INFO namenode.NameNode: Caching file names occuring more than 10 times 



12/12/02 17:11:13 INFO common.Storage: Image file of size 111 saved in 0 seconds.



12/12/02 17:11:13 INFO common.Storage: Storage directory /users/Billy/hadoop/tmp/hadoop-Billy/dfs/name has been successfully formatted.



12/12/02 17:11:13 INFO namenode.NameNode: SHUTDOWN\_MSG: 



/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*



SHUTDOWN\_MSG: Shutting down NameNode at BillymatoMacBook-Air.local/192.168.1.102



\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/



 



启动Hadoop



很简单，一条命令搞定。



$HADOOP\_HOME/bin/start-all.sh



顺利的话，一般会让你输入三次账号的密码。



 



简单调试



如果想试试看是否已经成功启动，可以用自带的例子试验一下：



 $hadoop jar $HADOOP\_HOME/hadoop-example-1.0.4.jar pi 10 100



成功的话，会有类似结果：



BillymatoMacBook-Air:hadoop Billy$ hadoop jar $HADOOP\_HOME/hadoop-examples-1.0.4.jar pi 10 100



Warning: $HADOOP\_HOME is deprecated.





Number of Maps  = 10



Samples per Map = 100



Wrote input for Map \#0

