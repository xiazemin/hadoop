# 基本功能介绍



A more thorough overview of the architectural decisions involved in the design and implementation of HDFS is given in the official Hadoop HDFS documentation. Before continuing in this tutorial, it is advisable that you read and understand the information presented there.

Configuring HDFS

The HDFS for your cluster can be configured in a very short amount of time. First we will fill out the relevant sections of the Hadoop configuration file, then format the NameNode.

CLUSTER CONFIGURATION



These instructions for cluster configuration assume that you have already downloaded and unzipped a copy of Hadoop. Module 3 discusses getting started with Hadoop for this tutorial. Module 7 discusses how to set up a larger cluster and provides preliminary setup instructions for Hadoop, including downloading prerequisite software.

The HDFS configuration is located in a set of XML files in the Hadoop configuration directory; conf/ under the main Hadoop install directory \(where you unzipped Hadoop to\). The conf/hadoop-defaults.xml file contains default values for every parameter in Hadoop. This file is considered read-only. You override this configuration by setting new values in conf/hadoop-site.xml. This file should be replicated consistently across all machines in the cluster. \(It is also possible, though not advisable, to host it on NFS.\)

Configuration settings are a set of key-value pairs of the format:

  &lt;property&gt;

    &lt;name&gt;property-name&lt;/name&gt;

    &lt;value&gt;property-value&lt;/value&gt;



  &lt;/property&gt;

Adding the line &lt;final&gt;true&lt;/final&gt; inside the property body will prevent properties from being overridden by user applications. This is useful for most system-wide configuration options.

The following settings are necessary to configure HDFS:

key	value	example

fs.default.name	protocol://servername:port	hdfs://alpha.milkman.org:9000

dfs.data.dir	pathname	/home/username/hdfs/data

dfs.name.dir	pathname	/home/username/hdfs/name

These settings are described individually below:

fs.default.name - This is the URI \(protocol specifier, hostname, and port\) that describes the NameNode for the cluster. Each node in the system on which Hadoop is expected to operate needs to know the address of the NameNode. The DataNode instances will register with this NameNode, and make their data available through it. Individual client programs will connect to this address to retrieve the locations of actual file blocks.

dfs.data.dir - This is the path on the local file system in which the DataNode instance should store its data. It is not necessary that all DataNode instances store their data under the same local path prefix, as they will all be on separate machines; it is acceptable that these machines are heterogeneous. However, it will simplify configuration if this directory is standardized throughout the system. By default, Hadoop will place this under /tmp. This is fine for testing purposes, but is an easy way to lose actual data in a production system, and thus must be overridden.

dfs.name.dir - This is the path on the local file system of the NameNode instance where the NameNode metadata is stored. It is only used by the NameNode instance to find its information, and does not exist on the DataNodes. The caveat above about /tmp applies to this as well; this setting must be overridden in a production system.

Another configuration parameter, not listed above, is dfs.replication. This is the default replication factor for each block of data in the file system. For a production cluster, this should usually be left at its default value of 3. \(You are free to increase your replication factor, though this may be unnecessary and use more space than is required. Fewer than three replicas impact the high availability of information, and possibly the reliability of its storage.\)

The following information can be pasted into the hadoop-site.xml file for a single-node configuration:

&lt;configuration&gt;

  &lt;property&gt;

    &lt;name&gt;fs.default.name&lt;/name&gt;



    &lt;value&gt;hdfs://your.server.name.com:9000&lt;/value&gt;

  &lt;/property&gt;

  &lt;property&gt;

    &lt;name&gt;dfs.data.dir&lt;/name&gt;



    &lt;value&gt;/home/username/hdfs/data&lt;/value&gt;

  &lt;/property&gt;

  &lt;property&gt;

    &lt;name&gt;dfs.name.dir&lt;/name&gt;



    &lt;value&gt;/home/username/hdfs/name&lt;/value&gt;

  &lt;/property&gt;

&lt;/configuration&gt;

Of course, your.server.name.com needs to be changed, as does username. Using port 9000 for the NameNode is arbitrary.

After copying this information into your conf/hadoop-site.xml file, copy this to the conf/ directories on all machines in the cluster.

The master node needs to know the addresses of all the machines to use as DataNodes; the startup scripts depend on this. Also in the conf/ directory, edit the file slaves so that it contains a list of fully-qualified hostnames for the slave instances, one host per line. On a multi-node setup, the master node \(e.g., localhost\) is not usually present in this file.

Then make the directories necessary:

  user@EachMachine$ mkdir -p $HOME/hdfs/data



  user@namenode$ mkdir -p $HOME/hdfs/name

The user who owns the Hadoop instances will need to have read and write access to each of these directories. It is not necessary for all users to have access to these directories. Set permissions with chmod as appropriate. In a large-scale environment, it is recommended that you create a user named "hadoop" on each node for the express purpose of owning and running Hadoop tasks. For a single individual's machine, it is perfectly acceptable to run Hadoop under your own username. It is not recommended that you run Hadoop as root.

STARTING HDFS



Now we must format the file system that we just configured:

  user@namenode:hadoop$ bin/hadoop namenode -format

This process should only be performed once. When it is complete, we are free to start the distributed file system:

  user@namenode:hadoop$ bin/start-dfs.sh

This command will start the NameNode server on the master machine \(which is where the start-dfs.sh script was invoked\). It will also start the DataNode instances on each of the slave machines. In a single-machine "cluster," this is the same machine as the NameNode instance. On a real cluster of two or more machines, this script will ssh into each slave machine and start a DataNode instance.

Interacting With HDFS

This section will familiarize you with the commands necessary to interact with HDFS, loading and retrieving data, as well as manipulating files. This section makes extensive use of the command-line.

The bulk of commands that communicate with the cluster are performed by a monolithic script named bin/hadoop. This will load the Hadoop system with the Java virtual machine and execute a user command. The commands are specified in the following form:

  user@machine:hadoop$ bin/hadoop moduleName -cmd args...

The moduleName tells the program which subset of Hadoop functionality to use. -cmd is the name of a specific command within this module to execute. Its arguments follow the command name.

Two such modules are relevant to HDFS: dfs and dfsadmin. Their use is described in the sections below.

COMMON EXAMPLE OPERATIONS



The dfs module, also known as "FsShell," provides basic file manipulation operations. Their usage is introduced here.

A cluster is only useful if it contains data of interest. Therefore, the first operation to perform is loading information into the cluster. For purposes of this example, we will assume an example user named "someone" -- but substitute your own username where it makes sense. Also note that any operation on files in HDFS can be performed from any node with access to the cluster, whose conf/hadoop-site.xml is configured to set fs.default.name to your cluster's NameNode. We will call the fictional machine on which we are operating anynode. Commands are being run from the "hadoop" directory where you installed Hadoop. This may be /home/someone/src/hadoop on your machine, or /home/foo/hadoop on someone else's. These initial commands are centered around loading information into HDFS, checking that it's there, and getting information back out of HDFS.

Listing files

If we attempt to inspect HDFS, we will not find anything interesting there:

  someone@anynode:hadoop$ bin/hadoop dfs -ls

  someone@anynode:hadoop$

The "-ls" command returns silently. Without any arguments, -ls will attempt to show the contents of your "home" directory inside HDFS. Don't forget, this is not the same as /home/$USER \(e.g., /home/someone\) on the host machine \(HDFS keeps a separate namespace from the local files\). There is no concept of a "current working directory" or cd command in HDFS.

If you provide -ls with an argument, you may see some initial directory contents:

  someone@anynode:hadoop$ bin/hadoop dfs -ls /

  Found 2 items

  drwxr-xr-x   - hadoop supergroup          0 2008-09-20 19:40 /hadoop

  drwxr-xr-x   - hadoop supergroup          0 2008-09-20 20:08 /tmp

These entries are created by the system. This example output assumes that "hadoop" is the username under which the Hadoop daemons \(NameNode, DataNode, etc\) were started. "supergroup" is a special group whose membership includes the username under which the HDFS instances were started \(e.g., "hadoop"\). These directories exist to allow the Hadoop MapReduce system to move necessary data to the different job nodes; this is explained in more detail in Module 4.

So we need to create our home directory, and then populate it with some files.

Inserting data into the cluster

Whereas a typical UNIX or Linux system stores individual users' files in /home/$USER, the Hadoop DFS stores these in /user/$USER. For some commands like ls, if a directory name is required and is left blank, this is the default directory name assumed. \(Other commands require explicit source and destination paths.\) Any relative paths used as arguments to HDFS, Hadoop MapReduce, or other components of the system are assumed to be relative to this base directory.

Step 1: Create your home directory if it does not already exist.

  someone@anynode:hadoop$ bin/hadoop dfs -mkdir /user

If there is no /user directory, create that first. It will be automatically created later if necessary, but for instructive purposes, it makes sense to create it manually ourselves this time.

Then we are free to add our own home directory:



  someone@anynode:hadoop$ bin/hadoop dfs -mkdir /user/someone

Of course, replace /user/someone with /user/yourUserName.

Step 2: Upload a file. To insert a single file into HDFS, we can use the put command like so:

  someone@anynode:hadoop$ bin/hadoop dfs -put /home/someone/interestingFile.txt /user/yourUserName/

This copies /home/someone/interestingFile.txt from the local file system into /user/yourUserName/interestingFile.txt on HDFS.

Step 3: Verify the file is in HDFS. We can verify that the operation worked with either of the two following \(equivalent\) commands:

  someone@anynode:hadoop$ bin/hadoop dfs -ls /user/yourUserName

  someone@anynode:hadoop$ bin/hadoop dfs -ls

You should see a listing that starts with Found 1 items and then includes information about the file you inserted.

The following table demonstrates example uses of the put command, and their effects:

Command:	Assuming:	Outcome:

bin/hadoop dfs -put foo bar	No file/directory named /user/$USER/bar exists in HDFS	Uploads local file foo to a file named /user/$USER/bar

bin/hadoop dfs -put foo bar	/user/$USER/bar is a directory	Uploads local file foo to a file named /user/$USER/bar/foo

bin/hadoop dfs -put foo somedir/somefile	/user/$USER/somedir does not exist in HDFS	Uploads local file foo to a file named /user/$USER/somedir/somefile, creating the missing directory

bin/hadoop dfs -put foo bar	/user/$USER/bar is already a file in HDFS	No change in HDFS, and an error is returned to the user.

When the put command operates on a file, it is all-or-nothing. Uploading a file into HDFS first copies the data onto the DataNodes. When they all acknowledge that they have received all the data and the file handle is closed, it is then made visible to the rest of the system. Thus based on the return value of the put command, you can be confident that a file has either been successfully uploaded, or has "fully failed;" you will never get into a state where a file is partially uploaded and the partial contents are visible externally, but the upload disconnected and did not complete the entire file contents. In a case like this, it will be as though no upload took place.

Step 4: Uploading multiple files at once. The put command is more powerful than moving a single file at a time. It can also be used to upload entire directory trees into HDFS.

Create a local directory and put some files into it using the cp command. Our example user may have a situation like the following:

  someone@anynode:hadoop$ ls -R myfiles

  myfiles:

  file1.txt  file2.txt  subdir/



  myfiles/subdir:

  anotherFile.txt

  someone@anynode:hadoop$

This entire myfiles/ directory can be copied into HDFS like so:

  someone@anynode:hadoop$ bin/hadoop -put myfiles /user/myUsername

  someone@anynode:hadoop$ bin/hadoop -ls

  Found 1 items

  /user/someone/myfiles   &lt;dir&gt;    2008-06-12 20:59    rwxr-xr-x    someone    supergroup

  user@anynode:hadoop bin/hadoop -ls myfiles

  Found 3 items

  /user/someone/myfiles/file1.txt   &lt;r 1&gt;   186731  2008-06-12 20:59        rw-r--r--       someone   supergroup

  /user/someone/myfiles/file2.txt   &lt;r 1&gt;   168     2008-06-12 20:59        rw-r--r--       someone   supergroup

  /user/someone/myfiles/subdir      &lt;dir&gt;           2008-06-12 20:59        rwxr-xr-x       someone   supergroup

Thus demonstrating that the tree was correctly uploaded recursively. You'll note that in addition to the file path, ls also reports the number of replicas of each file that exist \(the "1" in &lt;r 1&gt;\), the file size, upload time, permissions, and owner information.

Another synonym for -put is -copyFromLocal. The syntax and functionality are identical.

Retrieving data from HDFS

There are multiple ways to retrieve files from the distributed file system. One of the easiest is to use cat to display the contents of a file on stdout. \(It can, of course, also be used to pipe the data into other applications or destinations.\)

Step 1: Display data with cat.

If you have not already done so, upload some files into HDFS. In this example, we assume that a file named "foo" has been loaded into your home directory on HDFS.

  someone@anynode:hadoop$ bin/hadoop dfs -cat foo

  \(contents of foo are displayed here\)

  someone@anynode:hadoop$

Step 2: Copy a file from HDFS to the local file system.

The get command is the inverse operation of put; it will copy a file or directory \(recursively\) from HDFS into the target of your choosing on the local file system. A synonymous operation is called -copyToLocal.

  someone@anynode:hadoop$ bin/hadoop dfs -get foo localFoo

  someone@anynode:hadoop$ ls

  localFoo

  someone@anynode:hadoop$ cat localFoo

  \(contents of foo are displayed here\)

Like the put command, get will operate on directories in addition to individual files.

Shutting Down HDFS

If you want to shut down the HDFS functionality of your cluster \(either because you do not want Hadoop occupying memory resources when it is not in use, or because you want to restart the cluster for upgrading, configuration changes, etc.\), then this can be accomplished by logging in to the NameNode machine and running:

  someone@namenode:hadoop$ bin/stop-dfs.sh

This command must be performed by the same user who started HDFS with bin/start-dfs.sh.

HDFS COMMAND REFERENCE



There are many more commands in bin/hadoop dfs than were demonstrated here, although these basic operations will get you started. Running bin/hadoop dfs with no additional arguments will list all commands which can be run with the FsShell system. Furthermore, bin/hadoop dfs -help commandName will display a short usage summary for the operation in question, if you are stuck.

A table of all operations is reproduced below. The following conventions are used for parameters:

italics denote variables to be filled out by the user.

"path" means any file or directory name.

"path..." means one or more file or directory names.

"file" means any filename.

"src" and "dest" are path names in a directed operation.

"localSrc" and "localDest" are paths as above, but on the local file system. All other file and path names refer to objects inside HDFS.

Parameters in \[brackets\] are optional.

Command	Operation

-ls path	Lists the contents of the directory specified by path, showing the names, permissions, owner, size and modification date for each entry.

-lsr path	Behaves like -ls, but recursively displays entries in all subdirectories of path.

-du path	Shows disk usage, in bytes, for all files which match path; filenames are reported with the full HDFS protocol prefix.

-dus path	Like -du, but prints a summary of disk usage of all files/directories in the path.

-mv src dest	Moves the file or directory indicated by src to dest, within HDFS.

-cp src dest	Copies the file or directory identified by src to dest, within HDFS.

-rm path	Removes the file or empty directory identified by path.

-rmr path	Removes the file or directory identified by path. Recursively deletes any child entries \(i.e., files or subdirectories of path\).

-put localSrc dest	Copies the file or directory from the local file system identified by localSrc to dest within the DFS.

-copyFromLocal localSrc dest	Identical to -put

-moveFromLocal localSrc dest	Copies the file or directory from the local file system identified by localSrc to dest within HDFS, then deletes the local copy on success.

-get \[-crc\] src localDest	Copies the file or directory in HDFS identified by src to the local file system path identified by localDest.

-getmerge src localDest \[addnl\]	Retrieves all files that match the path src in HDFS, and copies them to a single, merged file in the local file system identified by localDest.

-cat filename	Displays the contents of filename on stdout.

-copyToLocal \[-crc\] src localDest	Identical to -get

-moveToLocal \[-crc\] src localDest	Works like -get, but deletes the HDFS copy on success.

-mkdir path	Creates a directory named path in HDFS. Creates any parent directories in path that are missing \(e.g., like mkdir -p in Linux\).

-setrep \[-R\] \[-w\] rep path	Sets the target replication factor for files identified by path to rep. \(The actual replication factor will move toward the target over time\)

-touchz path	Creates a file at path containing the current time as a timestamp. Fails if a file already exists at path, unless the file is already size 0.

-test -\[ezd\] path	Returns 1 if path exists; has zero length; or is a directory, or 0 otherwise.

-stat \[format\] path	Prints information about path. format is a string which accepts file size in blocks \(%b\), filename \(%n\), block size \(%o\), replication \(%r\), and modification date \(%y, %Y\).

-tail \[-f\] file	Shows the lats 1KB of file on stdout.

-chmod \[-R\] mode,mode,... path...	Changes the file permissions associated with one or more objects identified by path.... Performs changes recursively with -R. mode is a 3-digit octal mode, or {augo}+/-{rwxX}. Assumes a if no scope is specified and does not apply a umask.

-chown \[-R\] \[owner\]\[:\[group\]\] path...	Sets the owning user and/or group for files or directories identified by path.... Sets owner recursively if -R is specified.

-chgrp \[-R\] group path...	Sets the owning group for files or directories identified by path.... Sets group recursively if -R is specified.

-help cmd	Returns usage information for one of the commands listed above. You must omit the leading '-' character in cmd

DFSADMIN COMMAND REFERENCE



While the dfs module for bin/hadoop provides common file and directory manipulation commands, they all work with objects within the file system. The dfsadmin module manipulates or queries the file system as a whole. The operation of the commands in this module is described in this section.

Getting overall status: A brief status report for HDFS can be retrieved with bin/hadoop dfsadmin -report. This returns basic information about the overall health of the HDFS cluster, as well as some per-server metrics.

More involved status: If you need to know more details about what the state of the NameNode's metadata is, the command bin/hadoop dfsadmin -metasave filename will record this information in filename. The metasave command will enumerate lists of blocks which are under-replicated, in the process of being replicated, and scheduled for deletion. NB: The help for this command states that it "saves NameNode's primary data structures," but this is a misnomer; the NameNode's state cannot be restored from this information. However, it will provide good information about how the NameNode is managing HDFS's blocks.

Safemode: Safemode is an HDFS state in which the file system is mounted read-only; no replication is performed, nor can files be created or deleted. This is automatically entered as the NameNode starts, to allow all DataNodes time to check in with the NameNode and announce which blocks they hold, before the NameNode determines which blocks are under-replicated, etc. The NameNode waits until a specific percentage of the blocks are present and accounted-for; this is controlled in the configuration by the dfs.safemode.threshold.pct parameter. After this threshold is met, safemode is automatically exited, and HDFS allows normal operations. The bin/hadoop dfsadmin -safemode what command allows the user to manipulate safemode based on the value of what, described below:

enter - Enters safemode

leave - Forces the NameNode to exit safemode

get - Returns a string indicating whether safemode is ON or OFF

wait - Waits until safemode has exited and returns

Changing HDFS membership - When decommissioning nodes, it is important to disconnect nodes from HDFS gradually to ensure that data is not lost. See the section on decommissioning later in this document for an explanation of the use of the -refreshNodes dfsadmin command.

Upgrading HDFS versions - When upgrading from one version of Hadoop to the next, the file formats used by the NameNode and DataNodes may change. When you first start the new version of Hadoop on the cluster, you need to tell Hadoop to change the HDFS version \(or else it will not mount\), using the command: bin/start-dfs.sh -upgrade. It will then begin upgrading the HDFS version. The status of an ongoing upgrade operation can be queried with the bin/hadoop dfsadmin -upgradeProgress status command. More verbose information can be retrieved with bin/hadoop dfsadmin -upgradeProgress details. If the upgrade is blocked and you would like to force it to continue, use the command: bin/hadoop dfsadmin -upgradeProgress force. \(Note: be sure you know what you are doing if you use this last command.\)

When HDFS is upgraded, Hadoop retains backup information allowing you to downgrade to the original HDFS version in case you need to revert Hadoop versions. To back out the changes, stop the cluster, re-install the older version of Hadoop, and then use the command: bin/start-dfs.sh -rollback. It will restore the previous HDFS state.

Only one such archival copy can be kept at a time. Thus, after a few days of operation with the new version \(when it is deemed stable\), the archival copy can be removed with the command bin/hadoop dfsadmin -finalizeUpgrade. The rollback command cannot be issued after this point. This must be performed before a second Hadoop upgrade is allowed.

Getting help - As with the dfs module, typing bin/hadoop dfsadmin -help cmd will provide more usage information about the particular command.

Using HDFS in MapReduce

The HDFS is a powerful companion to Hadoop MapReduce. By setting the fs.default.name configuration option to point to the NameNode \(as was done above\), Hadoop MapReduce jobs will automatically draw their input files from HDFS. Using the regular FileInputFormat subclasses, Hadoop will automatically draw its input data sources from file paths within HDFS, and will distribute the work over the cluster in an intelligent fashion to exploit block locality where possible. The mechanics of Hadoop MapReduce are discussed in much greater detail in Module 4.

Using HDFS Programmatically

While HDFS can be manipulated explicitly through user commands, or implicitly as the input to or output from a Hadoop MapReduce job, you can also work with HDFS inside your own Java applications. \(A JNI-based wrapper, libhdfs also provides this functionality in C/C++ programs.\)

This section provides a short tutorial on using the Java-based HDFS API. It will be based on the following code listing:



1:  import java.io.File;

2:  import java.io.IOException;

3:

4:  import org.apache.hadoop.conf.Configuration;

5:  import org.apache.hadoop.fs.FileSystem;

6:  import org.apache.hadoop.fs.FSDataInputStream;

7:  import org.apache.hadoop.fs.FSDataOutputStream;

8:  import org.apache.hadoop.fs.Path;

9:

10: public class HDFSHelloWorld {

11:

12:   public static final String theFilename = "hello.txt";

13:   public static final String message = "Hello, world!\n";

14:

15:   public static void main \(String \[\] args\) throws IOException {

16:

17:     Configuration conf = new Configuration\(\);

18:     FileSystem fs = FileSystem.get\(conf\);

19:

20:     Path filenamePath = new Path\(theFilename\);

21:

22:     try {

23:       if \(fs.exists\(filenamePath\)\) {

24:         // remove the file first

25:         fs.delete\(filenamePath\);

26:       }

27:

28:       FSDataOutputStream out = fs.create\(filenamePath\);

29:       out.writeUTF\(message;

30:       out.close\(\);

31:

32:       FSDataInputStream in = fs.open\(filenamePath\);

33:       String messageIn = in.readUTF\(\);

34:       System.out.print\(messageIn\);

35:       in.close\(\);

46:     } catch \(IOException ioe\) {

47:       System.err.println\("IOException during operation: " + ioe.toString\(\)\);

48:       System.exit\(1\);

49:     }

40:   }

41: }

This program creates a file named hello.txt, writes a short message into it, then reads it back and prints it to the screen. If the file already existed, it is deleted first.

First we get a handle to an abstract FileSystem object, as specified by the application configuration. The Configuration object created uses the default parameters.

17:     Configuration conf = new Configuration\(\);

18:     FileSystem fs = FileSystem.get\(conf\);

The FileSystem interface actually provides a generic abstraction suitable for use in several file systems. Depending on the Hadoop configuration, this may use HDFS or the local file system or a different one altogether. If this test program is launched via the ordinary 'java classname' command line, it may not find conf/hadoop-site.xml and will use the local file system. To ensure that it uses the proper Hadoop configuration, launch this program through Hadoop by putting it in a jar and running:

$HADOOP\_HOME/bin/hadoop jar yourjar HDFSHelloWorld

Regardless of how you launch the program and which file system it connects to, writing to a file is done in the same way:

28:       FSDataOutputStream out = fs.create\(filenamePath\);

29:       out.writeUTF\(message\);

30:       out.close\(\);

First we create the file with the fs.create\(\) call, which returns an FSDataOutputStream used to write data into the file. We then write the information using ordinary stream writing functions; FSDataOutputStream extends the java.io.DataOutputStream class. When we are done with the file, we close the stream with out.close\(\).

This call to fs.create\(\) will overwrite the file if it already exists, but for sake of example, this program explicitly removes the file first anyway \(note that depending on this explicit prior removal is technically a race condition\). Testing for whether a file exists and removing an existing file are performed by lines 23-26:

23:       if \(fs.exists\(filenamePath\)\) {

24:         // remove the file first

25:         fs.delete\(filenamePath\);

26:       }

Other operations such as copying, moving, and renaming are equally straightforward operations on Path objects performed by the FileSystem.

Finally, we re-open the file for read, and pull the bytes from the file, converting them to a UTF-8 encoded string in the process, and print to the screen:

32:       FSDataInputStream in = fs.open\(filenamePath\);

33:       String messageIn = in.readUTF\(\);

34:       System.out.print\(messageIn\);

35:       in.close\(\);

The fs.open\(\) method returns an FSDataInputStream, which subclasses java.io.DataInputStream. Data can be read from the stream using the readUTF\(\) operation, as on line 33. When we are done with the stream, we call close\(\) to free the handle associated with the file.

More information:

Complete JavaDoc for the HDFS API is provided at http://hadoop.apache.org/common/docs/r0.20.2/api/index.html.

A direct link to the FileSystem interface is: http://hadoop.apache.org/common/docs/r0.20.2/api/org/apache/hadoop/fs/FileSystem.html.

Another example HDFS application is available on the Hadoop wiki. This implements a file copy operation.

HDFS 

