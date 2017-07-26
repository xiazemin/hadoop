# 单节点实践

$ vi etc/hadoop/core-site.xml

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



 



