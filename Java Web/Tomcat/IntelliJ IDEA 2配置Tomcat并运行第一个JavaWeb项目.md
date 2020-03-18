# 在IntelliJ IDEA 2018上配置Tomcat并运行第一个JavaWeb项目

## 1 下载和启动Tomcat

进入官网 [tomcat.apache.org/](http://tomcat.apache.org/) ，下载最新版本的Tomcat 9

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382ba32372e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

根据自己的电脑版本下载，我这里是windows 64位

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382bb1729b5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

下载完之后解压即可。 找到自己解压目录，打开文件夹下面的/bin目录，其中startup.bat是启动tomcat，shutdown.bat 是关闭tomcat

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382bb792c0d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

双击startup.bat启动tomcat后，打开

 

http://localhost:8080

 

，若进入下面的界面则表示启动成功了。

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382bca848f8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 2 在win10中为Tomcat 9配置环境变量

右击“我的电脑”，点击“属性”，选择“高级系统变量”

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382bbd98c6d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

选择“高级”选项卡->“环境变量”

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382c155aa09?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

在“系统变量”中，添加系统变量 添加内容如下： 新建变量名：CATALINA_BASE 变量值：E:\SoftWares\tomcat9\apache-tomcat-9.0.8 //复制你自己电脑上的tomcat解压目录



新建变量名：CATALINA_HOME 变量值：E:\SoftWares\tomcat9\apache-tomcat-9.0.8 //复制你自己电脑上的tomcat解压目录

点击确定。 然后编辑修改CLASSPATH和Path的变量值。 在ClassPath的变量值中加入：%CATALINA_HOME%\lib\servlet-api.jar;（注意要在原变量值后加英文状态下的“;”） 在Path的变量值中加入：%CATALINA_HOME%\bin;%CATALINA_HOME%\lib（注意要在原变量值后加英文状态下的“;”） 点击确定即可。

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382e10ec8f5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

最后验证一下是否配置成功了。 使用键盘win+R 输入cmd，输入startup命令，如果出现下图则表示配置成功了。

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382e19db476?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 3 在IDEA上新建第一个JavaWeb项目

点击File -> New ->Project...

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382e2cfb320?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

选择 Java -> Web Application，然后next

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382e68205db?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

起一个项目名字，这里以JavaWebTest为例，然后点击Finsh即可

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382e8a50ed9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

项目目录结构如下：

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf6382ec291957?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

修改一下index.jsp里面的代码，便于等下测试，代码如下：



```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  Hello world!
  </body>
</html>
复制代码
```

## 4 在IntelliJ IDEA 2018上配置Tomcat并运行项目

打开IDEA，点击Run-Edit Configurations...

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf63830c341886?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

点击“+”号，然后找到Tomcat Server，选择Local

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf63830900ab0d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

在Tomcat Server -> Unnamed -> Server -> Application server项目下，点击 Configuration ，找到本地 Tomcat 服务器（即前面的解压路径），再点击 OK按钮。可以把Unnamed修改成其他名字，如Tomcat 9

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf63830db07ef0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

然后转到旁边的Deployment选项卡，点击“+”号，选择Artifact，选择项目名称

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf63830dc33836?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

如下图所示，也可以修改一下Application context 路径的名字，改得简单一点，然后点击OK即可

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf63830dddb2b1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

配置完之后就可以在项目界面看到下图，点击运行即可

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf638315817b00?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

运行结果

![在这里插入图片描述](https://user-gold-cdn.xitu.io/2019/9/3/16cf63832ea02f5f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





# 备注: Tomcat指定(JDK路径)JAVA_HOME而不用环境变量

​     Tomcat默认情况下会用系统的环境变量中找到JAVA_HOME和JRE_HOME。但是有的时候我们需要不同版本的JDK共存。

可以在${TOMCAT_HOME}/bin/catalina.bat最前面设置JAVA_HOME和JRE_HOME。  

 例如：         

```bash
set JAVA_HOME=E:\Program Files\Java\jdk1.7.0_65
set JRE_HOME=E:\Program Files\Java\jre7
```



或修改${TOMCAT_HOME}/bin/setclasspath.bat文件中添加

```bash
set "JAVA_HOME=C:\Program Files (x86)\Java\jdk_1.6_31_64"
```





在linux下Tomcat配置连接jdk路径${TOMCAT_HOME}/bin/catalina.sh或setenv.sh文件：

```bash
export JAVA_HOME=/usr/java/jdk1.7.0_55

或用的全一点如下设置（用上面只设置一个JAVA_HOME足够了）：
export JAVA_HOME=/usr/local/java/jdk1.8.0_162
export JRE_HOME=/usr/local/java/jdk1.8.0_162/jre
export PATH=$PATH:/usr/local/java/jdk1.8.0_162/bin
export CLASSPATH=/usr/local/java/jdk1.8.0_162/lib:/usr/local/java/jdk1.8.0_162/jre/lib
```





注：export JAVA_HOME=/usr/lib/java 的 “=”左右两边不能有空格。不然会报export: `/usr/lib/java': 不是有效的标识符



另外，有时需要设置Tomcat的最大内存，方法如下：

在catalina.bat的中加入

```bash
windows下在catalina.bat中添加：
set JAVA_OPTS=%JAVA_OPTS% -server -XX:PermSize=128M -XX:MaxPermSize=512m

linux下在catalina.sh中添加：

JAVA_OPTS="-Xms256m -Xmx512m -Xss1024K -XX:PermSize=128m -XX:MaxPermSize=256m"
```

