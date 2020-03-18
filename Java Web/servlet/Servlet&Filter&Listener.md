### Servlet

Servlet 是服务端的 Java 应用程序，用于处理HTTP请求，做出相应的响应。

![img](http://static.codebelief.com/2017/09/20/servlet_mechanism.jpg)

当客户端向服务器发出HTTP请求时，首先会由服务器中的 Web 容器（如Tomcat）对请求进行路由，交给该URL对应的 Servlet 进行处理，Servlet 所要做的事情就是返回适当的内容给用户。

在这里，我顺便谈谈自己对 JSP 和 Servlet 两者关系的理解。JSP 页面实际是被转换成了 Servlet 的形式运行，也就是说，我们可以将不同的 JSP 页面视为不同的 Servlet，理论上它们中的每一个都能够对客户端请求做出响应，只不过这些 Servlet 可能会将请求转发给其它 Servlet 进行处理，进而由其它 Servlet 做出响应。

要创建自己的 Servlet，可以通过以下三种方式来实现

- 实现 Servlet 接口
- 继承 GenericServlet 类
- 继承 HttpServlet 类

它们的 package 分别是：[javax.servlet.Servlet](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/Servlet.html)、[javax.servlet.GenericServlet](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/GenericServlet.html)、[javax.servlet.http.HttpServlet](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/http/HttpServlet.html)。

### Filter

Filter 是介于 Web 容器和 Servlet 之间的过滤器，用于过滤未到达 Servlet 的请求或者由 Servlet 生成但还未返回响应。

![img](http://static.codebelief.com/2017/09/20/filter_mechanism.jpg)

客户端请求从 Web 容器到达 Servlet 之前，会先经过 Filter，由 Filter 对 request 的某些信息进行处理之后交给 Servlet。

同样，响应从 Servlet 传回 Web 容器之前，也会被 Filter 拦截，由 Filter 对 response 进行处理之后再交给 Web 容器。

若要创建自定义 Filter，需要实现 [javax.servlet.Filter](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/Filter.html) 接口。

### Listener

Listener 是用于监听某些特定动作的监听器。当特定动作发生时，监听该动作的监听器就会自动调用对应的方法。

以 [HttpSessionListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/http/HttpSessionListener.html) 为例：

![img](http://static.codebelief.com/2017/09/20/listener_mechanism.jpg)

该 Listener 监听 session 的两种状态，即创建和销毁。当 session 被创建时，会自动调用 [HttpSessionListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/http/HttpSessionListener.html) 的 sessionCreated() 方法，我们可以在该方法中添加一些处理逻辑。当 session 被销毁时，则会调用 sessionDestroyed() 方法。

下面是 Servlet 中的 8 个 Listener 接口，可分为三类：

第一类，ServletContext 相关接口

1. [ServletContextListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/ServletContextListener.html)：用于监听 ServletContext 的启动和销毁。
2. [ServletContextAttributeListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/ServletContextAttributeListener.html)：用于监听 application 范围的属性变化。

第二类，HttpSession 相关接口

1. [HttpSessionListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/http/HttpSessionListener.html)：用于监听 session 的创建和销毁。
2. [HttpSessionIdListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/http/HttpSessionIdListener.html)：用于监听 session 的 id 是否被更改。
3. [HttpSessionAttributeListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/http/HttpSessionAttributeListener.html)：用于监听 session 范围的属性变化。
4. [HttpSessionActivationListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/http/HttpSessionActivationListener.html)：用于监听绑定在 HttpSession 对象中的 JavaBean 状态。
5. [HttpSessionBindingListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/http/HttpSessionBindingListener.html)：用于监听对象与 session 的绑定和解绑。

第三类，ServletRequest 相关接口

1. [ServletRequestListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/ServletRequestListener.html)：用于坚挺 ServletRequest 对象的初始化和销毁。
2. [ServletRequestAttributeListener](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/ServletRequestAttributeListener.html)：用于监听 ServletRequest 对象的属性变化。