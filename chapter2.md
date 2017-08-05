# 开发环境搭建

下载hadoop2x-eclipse-plugin：

[https://github.com/xiazemin/hadoop2x-eclipse-plugin](https://github.com/xiazemin/hadoop2x-eclipse-plugin)

在hadoop2x-eclipse-plugin/release/下找选一个\*.jar拷贝到eclipse安装目录的plugin目录下。这里说两点:

1，mac下如果eclipse被拖到了application中，那么辅助点按finder/application下的eclipse图标，选择”显示包内容”即可进入安装目录。

2，我的hadoop版本是2.7.2，兼容版本号是2.6.0的插件。

进入eclipse,首先要配置就是library pth，在Preferences\(可以通过quick access找到\) 中可以看到有一个Hadoop Map/Reduce栏，将自己的hadoop安装路径写下,比如我的是/usr/local/Cellar/hadoop/2.7.2/libexec。

现在新建工程，可以看到有一个”Map/Reduce Project”可供选择，没什么好说的，按照创建一般工程的方法创建下去，我在这里创建了一个wordcount工程，里面有一个类WordCount,然后把MapReduce Tutorial中的实例代码复制过去

以上，各种import包没有报错，那么ok，至少说明可以编译通过了。

