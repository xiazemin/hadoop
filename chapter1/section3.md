# 单节点实践

[https://archive.apache.org/dist/hadoop/common/hadoop-2.6.4/](https://archive.apache.org/dist/hadoop/common/hadoop-2.6.4/)

$ vi etc/hadoop/core-site.xml

配置core-site.xml

&lt;configuration&gt;

&lt;property&gt;

```
&lt;name&gt;hadoop.tmp.dir&lt;/name&gt;



&lt;value&gt;/users/billy/hadoop/tmp/hadoop-${user.name}&lt;/value&gt;



&lt;description&gt;A base for other temporary directories.&lt;/description&gt;
```

&lt;/property&gt;

&lt;property&gt;

```
&lt;name&gt;fs.default.name&lt;/name&gt;



&lt;value&gt;hdfs://localhost:8020&lt;/value&gt;
```

&lt;/property&gt;

&lt;/configuration&gt;

配置hdfs-site.xml

&lt;configuration&gt;

```
&lt;property&gt;

    &lt;name&gt;dfs.replication&lt;/name&gt;

    &lt;value&gt;1&lt;/value&gt;

&lt;/property&gt;
```

&lt;/configuration&gt;

配置mapred-site.xml

&lt;configuration&gt;

```
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
```

&lt;/configuration&gt;

$ ./bin/hadoop namenode -format

DEPRECATED: Use of this script to execute hdfs command is deprecated.

Instead use the hdfs command for it.



17/07/26 16:48:15 INFO namenode.NameNode: STARTUP\_MSG:

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

STARTUP\_MSG: Starting NameNode

STARTUP\_MSG:   host = localhost/127.0.0.1

STARTUP\_MSG:   args = \[-format\]

STARTUP\_MSG:   version = 2.6.4

STARTUP\_MSG:   classpath = /Users/didi/hadoop/hadoop/etc/hadoop:/Users/didi/hadoop/hadoop/share/hadoop/common/lib/activation-1.1.jar:/Users/didi/hadoop/hadoop/share/hadoop/common/lib/apacheds-i18n-2.0.0-M15.jar:/Users/didi/hadoop/hadoop/share/hadoop/common/lib/apacheds-kerberos-codec-2.0.0-M15.jar:/Users/didi/hadoop/hadoop/share/hadoop/common/lib/api-asn1-api-1.0.0-M20.jar:/Users/didi/hadoop/hadoop/sha

$ ./bin/hadoop version

Hadoop 2.6.4

Subversion https://git-wip-us.apache.org/repos/asf/hadoop.git -r 5082c73637530b0b7e115f9625ed7fac69f937e6

Compiled by jenkins on 2016-02-12T09:45Z

Compiled with protoc 2.5.0

From source with checksum 8dee2286ecdbbbc930a6c87b65cbc010

This command was run using /Users/didi/hadoop/hadoop/share/hadoop/common/hadoop-common-2.6.4.jar

$ ssh localhost

Password:

Last login: Wed Jul 26 15:04:55 2017

$ sbin/start-all.sh

This script is Deprecated. Instead use start-dfs.sh and start-yarn.sh

17/07/26 17:01:22 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

Starting namenodes on \[localhost\]

Password:

localhost: starting namenode, logging to /Users/didi/hadoop/hadoop/logs/hadoop-didi-namenode-localhost.out

Password:

localhost: datanode running as process 8846. Stop it first.

Starting secondary namenodes \[0.0.0.0\]

Password:

0.0.0.0: secondarynamenode running as process 8928. Stop it first.

17/07/26 17:01:36 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable

starting yarn daemons

resourcemanager running as process 8230. Stop it first.

Password:

localhost: nodemanager running as process 9078. Stop it first.

