JDK，开发java程序用的开发包，JDK里面有java的运行环境(JRE)，包括client和server端的。需要配置环境变量。。。。

 

JRE，运行java程序的环境，JVM，JRE里面只有client运行环境，安装过程中，会自动添加PATH。

jdk 8 就是 jdk 1.8

jdk9 就是 jdk 1.9

**其他同理**

**官方虽然更新的快，但是大多数公司，为因为习惯问题和调整的麻烦，在加上 jdk 8 (jdk 1.8 ）能稳定使用就没有必要再用新的技术，但是学习还是建议学习最新版本的**

 

下载jdk 11 dev

安装，配置环境变量

Path 填绝对路径

C:\Program Files\Java\jdk-11.0.7\bin

 

https://tomcat.apache.org/download-90.cgi

下载windows 64位版本

 

解压，配置环境变量

 

 

Tomcat 乱码

**2.修改logging.properties配置**

a.打开tomcat/conf/logging.properties

b.添加语句：

java.util.logging.ConsoleHandler.encoding = GBK

c.重启tomcat，查看日志数据即可

 

来自 <https://blog.csdn.net/qq_23994787/article/details/104269538> 