## 前言

首先网上类似这种博客已经很多了，但是我觉得写得不够具体和贴切。我的需求是安装好之后可以在 控制面板\所有控制面板项\管理工具\服务 看到它的名字和描述，更重要的是可以控制它的启动和停止。网上很多例子都是采用双击bin目录下的startup.bat来启动，这样的方式存在很大的弊端，一旦不小心关掉窗口或者电脑重启，那么tomcat就会关闭，接下来我们就得花时间去管理，很麻烦。

## 安装流程

**1、首先安装JDK并且配置环境变量JAVA_HOME到path中，这个网上教程很多，就不在这里描述了。安装好之后，在cmd窗口中输入java -version可以显示下图JDK版本信息即可**


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190117153044722.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNTc0NDM1,size_16,color_FFFFFF,t_70)


**2、到[官网](https://tomcat.apache.org/download-80.cgi)下载tomcat版本。**

**3、将TOMCAT的压缩包解压到当前文件夹，并且修改文件夹的名字。**

个人建议是TOMCAT版本号-端口号。比如说：TOMCAT8-7000。这样以后在这个路径下安装多个TOMCAT的时候可以方便管理，一目了然。

**4、修改相关端口**

打开conf目录下的server.xml文件，将以下的端口进行修改。个人建议直接修改前面那个数字，比如说8443改成7443，简单粗暴，每个tomcat使用的端口第一个数字都不相同，避免了相互干扰。把下图中的8080,8443，8009改成你希望的端口号即可。


      <Connector port="8080" protocol="HTTP/1.1"
                   connectionTimeout="20000"
                   redirectPort="8443" />
    
     <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
     

**5、修改TOMCAT在服务列表中显示的名字和描述**

编辑bin目录下的service.bat文件，搜索“service_name”找到以下地方。service_name建议改成tomcat + 版本号 + -端口号，比如说Tomcat8-7000。方便以后管理。

    rem Set default Service name
    set SERVICE_NAME=Tomcat8
    set DISPLAYNAME=Apache Tomcat 8.5 %SERVICE_NAME%
    
再搜索“des”去找到服务的描述并进行修改，例如这是我的：

    "%EXECUTABLE%" //IS//%SERVICE_NAME% ^
        --Description "Apache Tomcat8-70端口 本地测试使用 Server - http://tomcat.apache.org/" ^
        
SERVICE_NAME，DISPLAYNAME和Description 分别对应下图的服务名称，显示名称和描述。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190117155709339.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNTc0NDM1,size_16,color_FFFFFF,t_70)


**5、使用命令安装服务**

在cmd中进入tomcat的bin目录，然后执行service install。安装好之后如下图所示，要注意catalina_home 和 catalina_base是否跟你解压出来的Tomcat目录一致。因为我遇到过有人在path中配置了这两个变量和路径，导致我出现无缘无故的问题。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190117160141802.png)

**6、接下来到服务列表中进行管理，在 控制面板\所有控制面板项\管理工具\服务  中可以看到你刚才安装的服务，如下图所示。**

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019011716054087.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNTc0NDM1,size_16,color_FFFFFF,t_70)


**7、这个时候需要你手动设置启动类型为自动并且点击启动服务。**

**8、在浏览器输入ip和端口如果可以看到tomcat的主页即可表示安装成功。**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190117160804860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNTc0NDM1,size_16,color_FFFFFF,t_70)


**9、如果你本机可以访问但是别人不能访问你的tomcat主页，那么估计是防火墙的拦截，这里的解决方案有两种：
（1）控制面板\所有控制面板项\Windows 防火墙\自定义设置 将防火墙关闭（不推荐）
（2） 在 控制面板\所有控制面板项\Windows 防火墙 左侧的高级设置中进入，然后点击入站规则和右侧的新建规则，将你的端口设置开放即可。出站规则也是如此设置。**

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019011716160717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNTc0NDM1,size_16,color_FFFFFF,t_70)

具体的设置在这里就不描述了，不懂得同学可以去查一下，设置好之后别人就成功访问了。

## 结语

**在本地将tomcat安装成windons服务主要是为了跟eclipse脱离开来，我们在eclipse自我测试通过后通过发布一个内网版本又可以愉快的码代码了，而不需要别人占用你eclipse下的tomcat从而影响你的工作。还有就是模拟线上的运行环境，把代码部署到内网中跑一段时间，通过观察日志看看是否产生异常也可以给我们很大的帮助。**
