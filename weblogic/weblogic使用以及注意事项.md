# weblogic单机+负载均衡安装部署，服务发布以及配置

## 基础操作

### 安装部署文档

weblogic安装部署文档请参考

[04呼叫中心-weblogic安装部署](./04呼叫中心-weblogic安装部署.docx)

### 服务部署和发布

1: 登录weblogic后，初始页面内容中点击左侧的部署，如图：

![](./weblogicimages/1.png)

2: 点击安装，根据操作点击下一步 上传自己服务的war包<font color=red>注意：weblogic和Tomcat部署服务有区别，Tomcat会解压war包，weblogic不会解压war，所以上传war时候需要把对应环境的配置都调整好</font> 选择服务所在的server,其他都默认下一步直接到完成，如图：

![](./weblogicimages/2.png)

![](./weblogicimages/3.png)

![](./weblogicimages/4.png)

### 服务负载均衡（Nginx）

Nginx安装部署以及配置文档请参考

[06nginx安装部署+Nginx配置.docx](./06nginx安装部署+Nginx配置.docx)

### 服务测试

#### weblogic直连测试

![](./weblogicimages/5.png)

#### weblogic+Nginx测试

![](./weblogicimages/6.png)

总结：以上2种测试效果其实是一样，只是从部署结构上一个是单机（单节点）直连一个是负载均衡（多节点）

## 基础配置

### 发布服务服务配置

1：回到初始页面可以看到发布好的服务（如果不知道可以点击左侧的部署菜单）

2：选中发布的服务进入到如图界面

![](./weblogicimages/7.png)

3：点击配置进入配置内容，这里主要是调整2个参数 最大内存中会话数（默认是1） JSP页面检查（默认是1）都调整成-1 可以查看后面框定的注释说明-1是不进行任何检查不占用资源

![](./weblogicimages/8.png)

### weblogic系统配置

1：端架左侧菜单环境-》服务器，进入页面后点击我们发布的服务AdminServer(管理)

![](./weblogicimages/9.png)

2：进入到AdminServer的配置下，选择头部的超载菜单，下面有一个最大线程阻塞时间建议1200（默认是600）可以根据实际调整

![](./weblogicimages/10.png)

3：选择头部并发菜单，最大并发新建线程数建议300（默认100）   最大并发长时间请求数100（默认100），可以根据实际调整

![](./weblogicimages/11.png)

## 通用命令



