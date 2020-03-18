本文主要是图文详细介绍如何使用IntelliJ IDEA 创建基于Maven构建的Web项目的过程。

环境配置：

- IntelliJ IDEA 12.1.4
- JDK 1.6.0_51
- Maven 3.0.4

详细步骤：

如果是第一次打开软件直接点击 Create New Project ，如果之前已经打开过项目了，需要点击菜单中 File → New Project … 如下图：

[![idea-maven-web-01](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-01.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-01.png)

选择 Maven module ，输入项目名称，点击 Next 如下：

[![idea-maven-web-02](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-02.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-02.png)

编辑 GroupId、ArtifactId、选择对应的 archetype ，点击Next 如下：

[![idea-maven-web-03](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-03.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-03.png)

确认信息正确后点击 Finish 按钮即可生成项目，如下图：

[![idea-maven-web-04](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-04.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-04.png)

到此项目已经创建完成，下面开始配置 Tomcat服务，点击工具栏中的 向下箭头的图标（如下图 红框标注）或者点击菜单栏中的 Run → Edit Configurations…  如下图：

[![idea-maven-web-06](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-06.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-06.png)

选中后如下：

[![idea-maven-web-07](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-07.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-07.png)

点击红色标注的 + 按钮，如下图：

[![idea-maven-web-08](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-08.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-08.png)

选择 Tomca Server → Local  如下图:

[![idea-maven-web-09](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-09.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-09.png)

输入名称（随便取比如 tomcat6）、配置tomcat 本地环境以及相应端口后，点击 页签 Deployment 如下图：

[![idea-maven-web-10](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-10.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-10.png)

点击 + 选择 Artifact …

[![idea-maven-web-11](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-11.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-11.png)

选择需要发布的应用，点击 OK 按钮。

[![idea-maven-web-12](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-12.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-12.png)

编辑 Application Context 的名称，点击OK按钮即完成Tomcat 配置。



点击下图中标注的绿色启动按钮，即可发布相关应用：

[![idea-maven-web-13](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-13.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-13.png)

启动正常应该看到如下信息：

[![idea-maven-web-14](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-14.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-14.png)

启动后会自动跳转到浏览器,看到如下内容：

[![idea-maven-web-15](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-15.png)](http://www.micmiu.com/wp-content/uploads/2013/10/idea-maven-web-15.png)

到此已经完成了项目创建、Tomcat配置、项目发布等过程。