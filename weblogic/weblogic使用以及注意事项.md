# weblogic介绍

[toc]

## 1. 简介

WebLogic是美国Oracle公司出品的一个application server，确切的说是一个基于JAVAEE架构的中间件，WebLogic是用于开发、集成、部署和管理大型分布式Web应用、网络应用和数据库应用的Java应用服务器。将Java的动态功能和Java Enterprise标准的安全性引入大型网络应用的开发、集成、部署和管理之中。

通俗的讲weblogci是一种web容器，如果你知道apache，tomcat，iis你就应该知道，就是把我们开发的java应用程序运行起来，并提供服务的一种程序。

### 1.1 Domain 

域是作为单元进行管理的一组相关的 WebLogic Server 资源。一个域包含一个或多个 WebLogic Server 实例，这些实例可以是群集实例、非群集实例，或者是群集与非群集实例的组合。一个域可以包含多个群集。域还包含部署在域中的应用程序组件、此域中的这些应用程序组件和服务器实例所需的资源和服务。应用程序和服务器实例使用的资源和服务示例包括计算机定义、可选网络通道、连接器和启动类。

Domain 中包含一个特殊的 WebLogic 服务器实例，叫做 Administration Server，这是我们配置、管理Domain中所有资源的核心。通常，我们称加入Domain中的其他实例为 Managed Server，所有的Web应用、EJB、Web Services和其他资源都部署在这些服务器上。

我们可以按照以下的一些标准来组织Domain：程序的逻辑划分，例如一个电商网站，可以将前台作为一个Domain，订单处理作为一个Domain，其他资源作为一个Domain；以物理地域为划分，例如一个跨国公司的网站，每个国家可以建立一个Domain；以尺寸来划分。

### 1.2 Managed Server 

被管理服务器是一个WebLogic Server的实例，他从Admin Server那里获取配置信息。通常在Managed Server上部署自己的服务、组件或应用。基于性能考虑，Managed Server维护着一份只读的域配置文件，当服务启动的时候，会从Admin Server上同步配置文件信息，如果配置文件发生了改变，Admin Server也会将改变推送到Managed Server上。

### 1.3 Machine 

Machine 是指运行WebLogic Server实例的物理机器，通常与部属在其上的Managed Server相关连。Machine用于Node Manager（一个运行在物理服务器上的进程，可以远程重启Admin 和 Managed Server）重启失败的Managed Server，以及为集群的Managed Servers选择合适的Session存放位置（Session避免存放在同一台Machine上）。

![](./weblogicimages/17.png)

## 2. 基础操作

### 2.1 weblogic安装

weblogic安装部署文档请参考

[04呼叫中心-weblogic安装部署](./04呼叫中心-weblogic安装部署.docx)



### 2.2 Nginx安装（负载均衡）

Nginx安装部署以及配置文档请参考

[06nginx安装部署+Nginx配置.docx](./06nginx安装部署+Nginx配置.docx)



### 2.3 weblogic发布服务

1: 登录weblogic后，初始页面内容中点击左侧的部署，如图：

![](./weblogicimages/1.png)



2: 点击安装，根据操作点击下一步 上传自己服务的war包<font color=red>注意：weblogic和Tomcat部署服务有区别，Tomcat会解压war包，weblogic不会解压war，所以上传war时候需要把对应环境的配置都调整好</font> 选择服务所在的server,其他都默认下一步直接到完成，如图：

![](./weblogicimages/2.png)

![](./weblogicimages/3.png)

![](./weblogicimages/4.png)



### 2.4 服务基于weblogic集群

暂无



### 2.5 发布服务测试(单机+负载均衡)

#### 2.5.1 weblogic直连测试--单机

![](./weblogicimages/5.png)



#### 2.5.2 weblogic+Nginx测试--负载均衡

![](./weblogicimages/6.png)

总结：以上2种测试效果其实是一样，只是从部署结构上一个是单机（单节点）直连一个是负载均衡（多节点）



### 2.6 weblogic集群测试

暂无



## 3. 基础配置

### 3.1 weblogic系统配置

<font color=red>系统配置只需要在对应的Server上配置，不需要重新配置，如果创建新的Server就需要重新配置</font>

1：端架左侧菜单环境-》服务器，进入页面后点击我们发布的服务AdminServer(管理)

![](./weblogicimages/9.png)



2：进入到AdminServer的配置下，选择头部的“超载”菜单，下面有一个最大线程阻塞时间建议1200（默认是600）可以根据实际调整

![](./weblogicimages/10.png)



3：选择头部“并发”菜单，最大并发新建线程数建议300（默认100）   最大并发长时间请求数100（默认100），可以根据实际调整

![](./weblogicimages/11.png)



4：点击“服务器启动”菜单 ，在下面的参数设置如下内容

配置内容：

~~~
-Xms2048m  -Xmx2048m  -XX:PermSize=512m -XX:MaxPermSize=1024m
~~~

配置说明：

~~~
-Xmx    Java Heap最大值，默认值为物理内存的1/4，最佳设值视物理内存大小及计算机内其他内存开销而定

-Xms    Java Heap初始值，Server端JVM最好将-Xms和-Xmx设为相同值，开发测试机JVM可以保留默认值；
-XX:MaxPermSize     Perm（俗称方法区）占整个堆内存的最大值
~~~

![](./weblogicimages/12.png)

5: 点击proddomain-》配置-》并发 

参数默认值都是50

![](./weblogicimages/16.png)



### 3.2 发布服务配置

<font color=red>发布服务的配置是每次发布服务后都需要重新配置一次</font>

1：回到初始页面可以看到发布好的服务（如果不知道可以点击左侧的部署菜单）

2：选中发布的服务进入到如图界面

![](./weblogicimages/7.png)

3：点击配置进入配置内容，这里主要是调整2个参数 最大内存中会话数（默认是1） JSP页面检查（默认是1）都调整成-1 可以查看后面框定的注释说明-1是不进行任何检查不占用资源

![](./weblogicimages/8.png)



### 3.3 查看配置所在文件位置

![](./weblogicimages/15.png)



## 4. 通用命令

1：启动weblogic命令

cd /usr/local/weblogic/weblogic/user_projects/proddomain/bin

nohup ./startWebLogic.sh



2：查看weblogic进程

ps -ef|grep weblogic



3：查看服务器weblogic资源使用

top  查看服务器进程情况



4：停止weblogic命令

cd /usr/local/weblogic/weblogic/user_projects/proddomain/bin

./stopWebLogic.sh



5：日志查看

cd /usr/local/weblogic/weblogic/user_projects/proddomain/logs



## 5. 已知问题和遇到的坑

### 5.1 服务部署失败

部署服务时候回出现服务发布不成功，重启weblogic服务后恢复



### 5.2 weblogic初步访问很慢

目前这个是已知问题Java JVM和Linux环境匹配时候（ 实际是JVM在Linux下的bug：他想调用一个随机函数，但是取不到。）

~~~
1)较好的解决办法：
        在Weblogic启动参数里添加 “-D java.security.egd=file:/dev/./urandom” (/dev/urandom 无法启动)
2)最差的解决办法： 
        执行命令 mv /dev/random /dev/random.ORIG ; ln /dev/urandom /dev/random
　　将/dev/random 指向/dev/urandom
3)最好的解决办法：
        修改Linux上Weblogic使用的jdk $JAVA_HOME/jre/lib/security/java.security 文件
　　将securerandom.source=file:/dev/urandom 修改为 securerandom.source=file:/dev/./urandom

这样可以解决任何一个域Weblogic启动慢的问题的问题
~~~

总结：演示环境做了测试目前发现没有多大的效果



### 5.3 weblogic发布服务中集成OData

1：OData中使用的协议

![](./weblogicimages/13.png)

2：现有系统组件包使用的协议情况

![](./weblogicimages/14.png)



