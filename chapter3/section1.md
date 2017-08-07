# java环境配置

$ jps

Unable to locate an executable at "/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home//bin/jps" \(-1\)

$ vi .bashrc

\#export JAVA\_HOME=/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home/

export JAVA\_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0\_144.jdk/Contents/Home/

export PATH=$PATH:/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home/bin

$ jps

17792 Jps

16840 NameNode

17002 SecondaryNameNode

2362

17180 NodeManager

637 Main

17103 ResourceManager

$ java

用法: java \[-options\] class \[args...\]

           \(执行类\)

   或  java \[-options\] -jar jarfile \[args...\]

           \(执行 jar 文件\)

