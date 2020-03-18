### 1、使用Maven模板创建项目

- 打开IDEA，选择Create New Project
  或者通过菜单File->New->Project创建

![IDEA + Maven Java开发](https://img.ken.io/blog/idea/project/project-01-01.png-kbrbs.png)

- 选择Maven，然后选择Maven项目模板maven-archetype-quickstart

![IDEA + Maven Java开发](https://img.ken.io/blog/idea/project/project-01-02.png-kbrbs.png)

### 2、项目命名与Maven配置

- 项目Maven坐标信息

![IDEA + Maven Java开发](https://img.ken.io/blog/idea/project/project-01-03.png-kbrbs.png)

在 POM 中，groupId, artifactId, packaging, version 叫作 maven 坐标，它能唯一的确定一个项目。有了 maven 坐标，我们就可以用它来指定我们的项目所依赖的其他项目，插件，或者父项目。

| 参数       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| groupId    | 代表组织和整个项目的唯一标志。比如说所有的Maven组件的groupId都是org.apache.maven。 |
| artifactId | 具体项目的名称，它于groupId共同确定一个项目在maven repo中的位置，例如，groupId=org.codehaus.mojo, artifactId=my-project的项目，在maven repo中的位置为：$M2_REPO/org/codehaus/mojo/my-project |
| version    | 用于说明目前项目的版本，在引用依赖的时候确定具体依赖的版本号。 |
| packaging  | 规定项目的输出格式，包括jar、war、pom、apk等，根据实际需要确定。例如，开发一般的java库，可以使用jar packaging；开发android则是apk packaging。 |

一般 maven 坐标写成如下的格式：

```
groupId:artifactId:packaging:version
```

虽然在不将项目提交到Maven官方仓库的情况下，这不是强制约束，但还是建议不论大小项目一律遵守Maven的命名标准。

- 选择Maven配置、仓库等。

这个在上一篇中已经配置过。保留默认即可。

![IDEA + Maven Java开发](https://img.ken.io/blog/idea/project/project-01-04.png-kbrbs.png)

- 指定项目名称&项目目录

这里的项目名称只做显示使用。跟Maven坐标无关。

![IDEA + Maven Java开发](https://img.ken.io/blog/idea/project/project-01-05.png-kbrbs.png)

如果项目文件夹未创建，会提示帮你创建，选择OK之后。IDEA就会进行项目创建&初始化工作

### 3、项目预览与启动

- 阅读IDEA使用tips

![IDEA + Maven Java开发](https://img.ken.io/blog/idea/project/project-01-06.png-kbrbs.png)

- 开启Maven Project自动导入

![IDEA + Maven Java开发](https://img.ken.io/blog/idea/project/project-01-07.png)

项目创建完成后，IDEA识别到这是一个Maven项目，是否导入到项目的IDEA的配置中。选择开启自动导入即可

- 启动应用程序

![IDEA + Maven Java开发](https://img.ken.io/blog/idea/project/project-01-08.png-kbrbs.png)

双击打开App.java文件。然后对文件编辑区域唤出鼠标右键菜单，选择Run App.main();
或者直接使用快捷键Ctrl+Shift+F10，IDEA会自动帮你创建调试配置并启动应用程序。 运行结果在IDEA底部的console面板。
（当然也可以通过运行AppTest来运行单元测试）
到这里，就完成了使用IntelliJ IDEA+Maven 创建，运行的第一个项目

## 三、项目结构说明

### 1、根目录说明

![IDEA + Maven项目目录结构](https://img.ken.io/blog/idea/project/project-02-00.png)

| 目录/文件          | 说明（[ken.io](https://ken.io/home/about)）                  |
| ------------------ | ------------------------------------------------------------ |
| .idea              | IDEA配置文件目录                                             |
| src                | 源文件文件目录（源代码、静态资源等等）                       |
| target             | 编译输出目录，用于存放编译后的文件（类文件，war包jar包等）。 |
| helloworld.iml     | IDEA用于记录Module配置的文件                                 |
| pom.xml            | Maven Project配置文件                                        |
| External Libraries | 用于查看Project的依赖                                        |

> 在 IntelliJ IDEA 中 Project 是最顶级的级别，次级别是 Module。一个 Project 可以有多个 Module。目前主流的大型项目结构都是类似这种多 Module 结构，比如我们的项目可以划分为helloworld-app，helloworld-utils，helloworld-model等等，Module之前可以互相依赖。而Project是一个抽象个概念，Project由一个或者多个Module组成。Project跟Module之间的关系由pom.xml来配置，Module之间的依赖由Module文件夹中的pom.xml来配置。

### 2、源文件目录说明

![IDEA + Maven项目目录结构](https://img.ken.io/blog/idea/project/project-02-01.png)

| 目录/文件                       | 说明（[ken.io](https://ken.io/home/about)）        |
| ------------------------------- | -------------------------------------------------- |
| src/main                        | 项目主目录                                         |
| src/main/java                   | 项目的源代码所在的目录（Sources Root）             |
| src/main/resources              | 项目的资源文件所在的目录（Resources Root）         |
| src/main/filters                | 项目的资源过滤文件所在的目录                       |
| src/main/webapp                 | web项目的配置、视图等目录                          |
| src/main/java/io.ken.helloworld | io.ken.helloworld是module的默认package,Maven的规范 |
| src/test                        | 项目测试目录（Sources Root）                       |
| src/test/java                   | 测试代码所在的目录（Resources Root）               |
| src/test/resources              | 测试相关的资源文件所在的目录                       |
| src/test/filters                | 测试相关的资源过滤文件所在的目录                   |

大多数情况下，一个项目都只有一个Module构成，需要进行分层都会通过package来完成。
例如：

![IDEA + Maven项目目录结构](https://img.ken.io/blog/idea/project/project-03-01.png)

### 3、pom.xml 文件说明

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>io.ken.helloworld</groupId>
  <artifactId>helloworld</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>helloworld</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <junit.version>3.8.1</junit.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>${junit.version}</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| modelVersion | Maven配置版本                                                |
| groupId      | 代表组织和整个项目的唯一标志。比如说所有的Maven组件的groupId都是org.apache.maven。 |
| artifactId   | 具体项目的名称，它于groupId共同确定一个项目在maven repo中的位置，例如，groupId=org.codehaus.mojo, artifactId=my-project的项目，在maven repo中的位置为：$M2_REPO/org/codehaus/mojo/my-project |
| version      | 用于说明目前项目的版本，在引用依赖的时候确定具体依赖的版本号。 |
| packaging    | 规定项目的输出格式，包括jar、war、pom、apk等，根据实际需要确定。例如，开发一般的java库，可以使用jar packaging；开发android则是apk packaging。 |
| name         | 项目显示名称                                                 |
| url          | 项目地址                                                     |
| properties   | 用于定义变量，可以在当前配置文件pom.xml，以及子Module的pom.xml中引用，引用方式：${propertyname}，例如：${junit.version} |
| dependencies | 用户配置Module的依赖                                         |





# 快速导入Maven依赖的方法

打开网络上的远程Maven仓库，网址：http://mvnrepository.com/，这个网站是这个样子的

![这里写图片描述](https://img-blog.csdn.net/20180106213715794?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZWFnbGV1bml2ZXJzaXR5ZXll/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

红线中是搜索框，用来搜索目标jar包，比如，我想在Maven项目中导入common-io包，我就搜索common-io

![这里写图片描述](https://img-blog.csdn.net/20180106213935681?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZWFnbGV1bml2ZXJzaXR5ZXll/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

搜索结果中的第一个就是我想导入的jar包，点进去，选择版本

![这里写图片描述](https://img-blog.csdn.net/20180106214434829?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZWFnbGV1bml2ZXJzaXR5ZXll/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

选一个最新版的，点进就有pom.xml文件的配置，复制粘贴即可

![img](https://img-blog.csdn.net/20180710164338588?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NTI1ODk5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)