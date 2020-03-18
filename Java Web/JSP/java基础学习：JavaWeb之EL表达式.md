# java基础学习：JavaWeb之EL表达式

其他更多java基础文章： [java基础学习(目录)](https://juejin.im/post/5c160421e51d45403917529f)

------

# 一、EL表达式

EL 全名为Expression Language。JSP中可以使用EL表达式，EL表达式是用"${}"括起来的脚本，用来更方便地读取对象，EL表达式写在JSP的HTML代码中，而不能写在"<%%>"引起的JSP脚本中

**EL表达式的功能：**

1. 获取数据：EL表达式主要用于替换JSP页面中的脚本表达式，以从各种类型的web域 中检索java对象、获取数据。(某个web域 中的对象，访问javabean的属性、访问list集合、访问map集合、访问数组)
2. 执行运算：利用EL表达式可以在JSP页面中执行一些基本的关系运算、逻辑运算和算术运算，以在JSP页面中完成一些简单的逻辑运算。${user==null}
3. 获取web开发常用对象：EL 表达式定义了一些隐式对象，利用这些隐式对象，web开发人员可以很轻松获得对web常用对象的引用，从而获得这些对象中的数据。
4. 调用Java方法：EL表达式允许用户开发自定义EL函数，以在JSP页面中通过EL表达式调用Java类的方法。

### 1.1、获取数据

使用EL表达式获取数据语法："${标识符}"

1. EL表达式语句在执行时，会调用pageContext.findAttribute方法，用标识符为关键字，分别从page、request、session、application四个域中查找相应的对象，找到则返回相应对象，找不到则返回”” （注意，不是null，而是空字符串）。 ${abc}
2. EL表达式可以很轻松获取JavaBean的属性，或获取数组、Collection、Map类型集合的数据。${requestScope.emp.address.street}

### 1.2、执行运算

语法：${运算表达式}，EL表达式支持如下运算符：
1）关系运算发

![img](https://user-gold-cdn.xitu.io/2018/12/16/167b64a80d06add1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



2）逻辑运算符

![img](https://user-gold-cdn.xitu.io/2018/12/16/167b64a80d1e05d5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



3）empty运算符：检查对象是否为null(空)，${!empty(list)}

4）二元表达式：${user!=null?[user.name](http://user.name/) :""}

5）[ ] 和 . 号运算符

### 1.3、获得web开发常用对象

EL表达式语言中定义了11个隐含对象，使用这些隐含对象可以很方便地获取web开发中的一些常见对象，并读取这些对象的数据。



![img](https://user-gold-cdn.xitu.io/2018/12/16/167b64a80d2d2ba6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



分析：
pageScope、requestScope、sessionScope、applicationScope代表四个作用域对象（用于保存属性的Map对象）
pageContext：表示的是JSP中内置对象pageContext,能获取request等其他JSP八大内置对象
param：表示一个请求参数， ${param.username}等效 request.getParameter("username");（表示一个保存了所有请求参数的Map对象）
paramValues：表示一组请求参数，${paramValues.abc}等效request.getParameterValues("abc");这种多选框（表示一个保存了所有请求参数的Map对象，它对于某个请求参数，返回的是一个string[]）
header：表示一个请求头，${header.referer}等效request.getHeader("referer");
headerValues：表示一组请求头，${header.cookie}等效 request.getHeaders("cookie");获取的请求头参数中的内容是一组内容，比如cookie就有可以是多个cookie一起传过来
cookie：获得cookie对象（表示一个保存了所有cookie的Map对象）
initPatam：web项目初始化参数，servletContext.getInitParameter("xxx");（表示一个保存了所有web应用初始化参数的map对象）

**注意1**：还有一种特殊的用法，直接获取对象变量${user.username}，user为User的一个实例对象，并且存放在page作用域中，上面这句代码的意思是，依次从page、request、session、application作用域查找user对象，直到找到为止，底层使用的是pageContext.findAttribute(); 是一样的效果。

**注意2**：测试header和headerValues或者有些参数时，如果头里面有“-” ，例Accept-Encoding，则要header["Accept-Encoding"]、headerValues["Accept-Encoding"] 测试cookie时，例${cookie.key}取的是cookie对象，如访问cookie的名称和值，须${[cookie.key.name](http://cookie.key.name/)}或${cookie.key.value}

# 二、总结

  EL表达式比较简单，使用它的目的是为了减少JSP脚本，尽量不要在HTML中嵌入Java代码，显的很混乱，而在HTML中使用EL表达式，就比较好来获取JSP中各种对象，获取四大作用域中的值，

1. 如果要获取四大作用域中的数据，则可以使用![{}、](https://juejin.im/equation?tex=%7B%7D%E3%80%81){[pageScope.xxx](http://pagescope.xxx/)}
2. 如果想要获取请求参数，则使用param或paramValues
3. 如果想要获取请求头中的一些信息，获取想要获取Servlet的一些对象，比如request、session等，可以使用pageContext来获取request对象，然后在获取所需要的信息，或者直接使用header对象来获取头信息
4. 如果想获取web初始化参数，则使用initPatam。

最主要的是要记得EL中有哪11个内置对象，知道了他们就知道了EL可以获取哪些信息。