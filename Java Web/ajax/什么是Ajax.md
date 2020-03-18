# 什么是Ajax

**Ajax(Asynchronous JavaScript and XML) 异步JavaScript和XML**

Ajax实际上是下面这几种技术的融合：

- (1)XHTML和CSS的基于标准的表示技术
- (2)DOM进行动态显示和交互
- (3)XML和XSLT进行数据交换和处理
- (4)XMLHttpRequest进行异步数据检索
- (5)Javascript将以上技术融合在一起

**客户端与服务器，可以在【不必刷新整个浏览器】的情况下，与服务器进行异步通讯的技术**

# 为什么我们需要Ajax？

在我们之前的开发，每当用户向服务器发送请求，**哪怕只是需要更新一点点的局部内容，服务器都会将整个页面进行刷新。**

- **性能会有所降低(一点内容，刷新整个页面！)**
- **用户的操作页面会中断(整个页面被刷新了)**

**Ajax就是能够做到局部刷新**！



![这里写图片描述](https://user-gold-cdn.xitu.io/2018/2/13/1618f9496059c917?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



------

# XMLHttpRequest

**XMLHttpRequest对象是Ajax中最重要的一个对象**。**使用Ajax更多的是编写客户端代码**，而不是服务端的代码。

\##XMLHttpRequest 工作原理##

传统的web前端与后端的交互中，浏览器直接访问Tomcat的Servlet来获取数据。Servlet通过转发把数据发送给浏览器。

当我们使用AJAX之后，**浏览器是先把请求发送到XMLHttpRequest异步对象之中，异步对象对请求进行封装，然后再与发送给服务器。服务器并不是以转发的方式响应，而是以流的方式把数据返回给浏览器**

XMLHttpRequest异步对象会**不停监听服务器状态的变化，得到服务器返回的数据，就写到浏览器上【因为不是转发的方式，所以是无刷新就能够获取服务器端的数据】**



![这里写图片描述](https://user-gold-cdn.xitu.io/2018/2/13/1618f9496dfa3abb?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



------

## 创建XMLHttpRequest对象

要创建XMLHttpRequest对象是要分两种情况考虑的：

- **在IE6以下的版本**
- **在IE6以上的版本以及其他内核的浏览器(Mozilla)等**

```
    <script type="text/javascript">

        var httpRequest;

        if(window.XMLHttpRequest) {

            //在IE6以上的版本以及其他内核的浏览器(Mozilla)等
            httpRequest = new XMLHttpRequest();
        }else if(window.ActiveXObject) {

            //在IE6以下的版本
            httpRequest = new ActiveXObject();
        }


    </script>
复制代码
```

## 了解XMLHttpRequest对象的属性和方法

### **方法**

- **open()**(**String method,String url,boolean asynch**,String username,String password)
- **send(content)**
- **setRequestHeader(String header,String value)**
- getAllResponseHeaders()
- getResponseHeader(String header)
- abort()

**常用的方法就是黑色粗体的前三个**

- open()：该方法创建http请求
  - **第一个参数是指定提交方式(post、get)**
  - **第二个参数是指定要提交的地址是哪**
  - **第三个参数是指定是异步还是同步(true表示异步，false表示同步)**
  - 第四和第五参数在http认证的时候会用到。是可选的
- setRequestHeader(String header,String value)：设置消息头（使用post方式才会使用到，get方法并不需要调用该方法）
  - **xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");**
- send(content)：发送请求给服务器
  - **如果是get方式，并不需要填写参数，或填写null**
  - **如果是post方式，把要提交的参数写上去**

### **属性**

- **onreadystatechange：请求状态改变的事件触发器（readyState变化时会调用此方法），一般用于指定回调函数**
- readyState：请求状态readyState一改变，回调函数被调用，它有5个状态
  - 0：未初始化
  - 1：open方法成功调用以后
  - 2：服务器已经应答客户端的请求
  - 3：交互中。Http头信息已经接收，响应数据尚未接收。
  - **4：完成。数据接收完成**



![这里写图片描述](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1280" height="706"></svg>)



- **responseText：服务器返回的文本内容**
- **responseXML：服务器返回的兼容DOM的XML内容**
- **status：服务器返回的状态码**
- statusText：服务器返回状态码的文本信息

上面有两个地方都提及了回调函数，回调函数是什么？？

回调函数就是**接收服务器返回的内容！**



![这里写图片描述](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1280" height="706"></svg>)



------

# 编写第一个Ajax程序

**检测用户输入的用户名是否为"zhongfucheng"，只要不是zhongfucheng，就可以使用！**

## html代码

- **创建的div只要用于显示服务器返回的数据**
- **当用户点击按钮的时候，就触发事件。**

```
	<input type="text" id="username">
	<input type="button" onclick="checkUsername()" value="检测用户名是否合法">
	<div id="result">
	
	</div>

复制代码
```

## JavaScript代码

- 创建XMLHttpRequest对象
- 创建http请求
- 把文本框的数据发送给http请求的目标
- 指定回调函数
- 编写回调函数
- 发送http请求
- 回调函数得到http返回的内容，把内容写在div上

```
    <script type="text/javascript">

        var httpRequest;
        function checkUsername() {

            if(window.XMLHttpRequest) {

                //在IE6以上的版本以及其他内核的浏览器(Mozilla)等
                httpRequest = new XMLHttpRequest();
            }else if(window.ActiveXObject) {

                //在IE6以下的版本
                httpRequest = new ActiveXObject();
            }


            //创建http请求
            httpRequest.open("POST", "Servlet1", true);

            //因为我使用的是post方式，所以需要设置消息头
            httpRequest.setRequestHeader("Content-type", "application/x-www-form-urlencoded");

            //指定回调函数
            httpRequest.onreadystatechange = response22;

            //得到文本框的数据
            var name = document.getElementById("username").value;

            //发送http请求，把要检测的用户名传递进去
            httpRequest.send("username=" + name);

        }

        function response22() {

            //判断请求状态码是否是4【数据接收完成】
            if(httpRequest.readyState==4) {

                //再判断状态码是否为200【200是成功的】
                if(httpRequest.status==200) {

                    //得到服务端返回的文本数据
                    var text = httpRequest.responseText;

                    //把服务端返回的数据写在div上
                    var div = document.getElementById("result");
                    div.innerText = text;
                }

            }
        }
    </script>

复制代码
```

## 效果

**实现了局部更新，不需要刷新整一个页面**



![这里写图片描述](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="988" height="538"></svg>)



------

# XMLHttpRequest解决中文乱码

在传统的Web中我们已经解决过中文乱码问题了。

- **服务器传送给浏览器数据发生乱码：response设置编码的时候和浏览器页面的编码一致便可以解决**
- **浏览器传送给服务器数据发生乱码：如果是post方式，request设置编码便可以解决。如果是get方式，Tomcat下，使用ISO8859-1编码得到原本的二进制数组，再使用UTF-8编码便可以解决**

接下来，要介绍的是：**我们可以屏蔽任何浏览器和任何服务器的编码格式，浏览器发送给服务器的数据不造成乱码问题！**

具体我们是这样做的：

- **发送数据给服务器的时候，JavaScript使用两次EncodeURI()**
- **服务器得到数据，使用URLEncode.decode(数据,"utf-8")进行解码**

为啥我能说这种方式屏蔽任何浏览器和服务器的编码格式，都不会乱码呢？？



![这里写图片描述](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1012" height="542"></svg>)



------

# XMLHttpRequest解决缓存问题

在传统的Web中我们也解决过缓存的问题，**通过设置response的头信息，返回给浏览器就可以实现不缓存页面了。**

但是呢，现在我们使用XMLHttpRequest，**拿到的不是全新的页面，仅仅是服务器端发送过来的数据！！**

那我们要怎么解决缓存的问题呢？？产生缓存的原因就是：**我们请求了同一个地址，做了相同的操作。服务端认为我的操作并没有什么变化，就直接把缓存的信息给我了**。这样的话，我就不能更换验证码图片了(等等应用)。

我们可以这样做：

- **在每次请求url中加入一个时间戳参数【每次url就不一样了】**
- 加入时间戳参数到url时，也分两种情况
  - **url本身就带有参数了，也就是说有"?"号了，那么添加时间戳的时候就需要用"&"号**
  - [x] **url没有参数，直接使用"?"号来添加时间戳**

```
	if(url.indexOf("?") >= 0){
	url = url + "&t=" + (new Date()).valueOf();
	} else{
	url = url + "?t=" + (new Date()).valueOf();
	}

复制代码
```

------

# XMLHttpRequest跨域访问

**使用XMLHttpRequest去跨域访问是会出现错误的**。



![这里写图片描述](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="503" height="251"></svg>)



我们要怎么解决呢？？这时候就要**用代理思想了**

- **XMLHttpRequest先把请求提交给同域的Servlet处理**
- **同域Servlet再将XMLHttpRequest的请求提交给跨域的服务器**
- **同域Servlet得到跨域服务器的返回值，再返回给XMLHttpRequest**



![这里写图片描述](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="552" height="186"></svg>)



这个时候，**XMLHttpRequest跨域访问就分两种(GET和POST)情况了，因为这两种提交数据的方式是不一样的！**

## 浏览器代码

- 我们需要在调用open方法之前**判断一下要连接的地址是不是以http开头的，如果是则认为要访问的是跨域的资源**
- **首先将当前url中的”?”变成”&”，这是因为将要连接的地址改为”Proxy?url=” + url以后，如果原来url地址中有参数的话，新的url地址中就会有两个“?”这会导致服务器端解析参数错误，”url=”之后的内容表示本来要访问的跨域资源的地址。**

## GET方式

**GET方式是直接把参数的信息都放在url地址上，所以处理起来会相对简单。**

步骤：

- **使用StringBuilder装载着getParameter("url")【获取得到地址，呆会要做拼接，所以用StringBuilder】**
- **得到其他参数的时候，做URLEncode.encode()，因为我们进入Servlet的时候已经被decode了一次【我们要尽可能保留原始请求】(参照解决中文乱码) **
- **遍历所有的请求参数，只要名字不是"url"，就添加到StringBuilder中【第一个参数为"?",其他的参数为"&"】(http://localhost:8080/url?aa=bb&cc=dd)**
- **创建URL对象，把拼接成的StringBuilder传递进去**
- **使用BufferReader读取远程服务器返回的数据，要指定输入流编码格式，否则会乱码**

```
	BufferedReader reader = new BufferedReader(new InputStreamReader(URL对象.openSteam(),"UTF-8"));

复制代码
```

- 最后，把远程服务器读取到的数据再返回给XMLHttpRequest

------

## POST方式

**POST方式把参数的信息都封装到HTTP请求中，在URL进行连接的时候，需要把数据写给远程服务器**

步骤：

- **得到url参数，创建StringBuilder**
- **得到其他参数的时候，做URLEncode.encode()，因为我们进入Servlet的时候已经被decode了一次【我们要尽可能保留原始请求】(参照解决中文乱码) **
- **遍历所有的请求参数，只要名字不是"url"，就添加到StringBuilder中【第一个参数直接给出,其他的参数为"&"】(aa=bb&cc=dd&ee=ff)**
- **创建URL对象，创建URL连接器，允许写数据到远程服务器上**

```
	URL url = new URL(url);
	URLConnection connection = url.openConnection;
	connection.setDoOutPut(true);


复制代码
```

- **得到写数据流**

```
	OutputSteamWriter writer = new OutputSteamWriter(conncetion.getOutputSteam)

复制代码
```

- **把StringBuilder的数据写到远程服务器上，并flush**
- **使用BufferReader读取远程服务器返回的数据**

```
	BufferedReader reader =  new BufferedReader(new InputSteamReader(conncetion.inputSteamReader,"UTF-8"));

复制代码
```

------

# AJAX二级下拉联动案例【XML版】

我们在购物的时候，常常需要我们来选择自己的收货地址，**先选择省份，再选择城市**...

有没有发现：**当我们选择完省份的时候，出现的城市全部都是根据省份来给我们选择的**。这是怎么做到的呢？？？其实就是通过AJAX来完成的。使用AJAX技术让我们看起来网页非常“智能”，会根据省份来给出对应的城市信息。



![这里写图片描述](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1280" height="588"></svg>)



我们这里就不读取数据库了，直接**在Servlet写死数据来进行模拟测试**。

------

## 分析

我们知道AJAX与服务器之间的交互常用的传输载体格式有三种：

- HTML
- XML
- JSON

由于**省份与城市是有层级关系的**，因此我们**只能用XML或者JSON**。

我们这里**首先就用XML来进行，后面会使用JSON，来看看他俩有什么不同的地方**。。

### 前台分析

**当用户选择了某个省份之后，就使用AJAX与服务器进行交互，那么在选择城市的时候就出现对应的城市信息。**

- **监听下拉框值变化事件**
- **只要下拉框值变化了，就与服务器进行交互**
- **得到服务器返回的值，解析XML**
- **使用DOM把数据写到城市下拉框列表中**

### 后台分析

- **得到前台带过来的数据**
- **判断该数据是什么，返回对应的的XML文件**

------

## 写JSP页面

```
<%--
  Created by IntelliJ IDEA.
  User: ozc
  Date: 2017/5/17
  Time: 19:38
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>多级联动</title>
    <script type="text/javascript" src="js/ajax.js"></script>
</head>
<body>

<%--############前台页面###################--%>
<select name="province" id="provinceId">
    <option value="-1">请选择省份</option>
    <option>广东</option>
    <option>湖南</option>
</select>
<select name="city" id="cityId">
    <option>请选择城市</option>
</select>

<%--############AJAX###################--%>

<script type="text/javascript">

    document.getElementById("provinceId").onchange = function () {


        /**********定位到下拉框，获取下拉框的值***************/
        // 获取选中的下拉框索引值
        var index = this.selectedIndex;
        // 得到下拉框的值
        var province = this.options[index].innerHTML;

        //下拉框的值要是“请选择”，那么我们是不会访问服务器的
        if ("请选择省份" != province) {

            /********由于每次都会自动添加，因此每次在调用的时候清除***********/
            var citySelect = document.getElementById("cityId");

            //每次都将option变成长度只有1的
            citySelect.options.length = 1;

            /*************ajax代码*********************/
            //创建AJAX对象
            var ajax = createAJAX();
            //准备发送请求
            var method = "post";
            var url = "${pageContext.request.contextPath}/ProvinceServlet?time=" + new Date().getTime();
            ajax.open(method, url);
            //由于是POST方式，因此要设置头
            ajax.setRequestHeader("content-type", "application/x-www-form-urlencoded");

            ajax.send("province=" + province);

            /************ajax回调函数*********************/
            ajax.onreadystatechange = function () {

                if (ajax.readyState == 4) {
                    if (ajax.status == 200) {

                        //得到服务器端带过来的XML
                        var XMLDocument = ajax.responseXML;

                        /**********解析XML文档，使用DOM写到下拉框中****************/
                        var cities = XMLDocument.getElementsByTagName("city");

                        //得到每一个cities节点的值，动态生成下拉框，添加到下拉框中
                        for (var i = 0; i < cities.length; i++) {
                            var value = cities[i].firstChild.nodeValue;
                            //动态生成下拉框
                            var optionElement = document.createElement("option");
                            optionElement.innerHTML = value;

                            //添加到下拉框中
                            citySelect.appendChild(optionElement);

                        }
                    }
                }
            };

        }

    };

</script>


</body>
</html>

复制代码
```

## Servlet

```
import java.io.IOException;
import java.io.PrintWriter;

/**
 * Created by ozc on 2017/5/17.
 */
@javax.servlet.annotation.WebServlet(name = "ProvinceServlet",urlPatterns = "/ProvinceServlet")
public class ProvinceServlet extends javax.servlet.http.HttpServlet {
    protected void doPost(javax.servlet.http.HttpServletRequest request, javax.servlet.http.HttpServletResponse response) throws javax.servlet.ServletException, IOException {

        //设置中文编码
        request.setCharacterEncoding("UTF-8");
        String province = request.getParameter("province");

        //这里是返回的是XML，因此指定XML数据！
        response.setContentType("text/xml;charset=UTF-8");

        PrintWriter printWriter = response.getWriter();

        /****************返回XML文件给前台**************/
        printWriter.write("<?xml version='1.0' encoding='UTF-8'?>");
        printWriter.write("<root>");
        if("广东".equals(province)){
            printWriter.write("<city>广州</city>");
            printWriter.write("<city>深圳</city>");
            printWriter.write("<city>中山</city>");
        }else if("湖南".equals(province)){
            printWriter.write("<city>长沙</city>");
            printWriter.write("<city>株洲</city>");
            printWriter.write("<city>湘潭</city>");
            printWriter.write("<city>岳阳</city>");
        }
        printWriter.write("</root>");

        System.out.println("1111");


        /*******事后操作*******/
        printWriter.flush();
        printWriter.close();


    }

    protected void doGet(javax.servlet.http.HttpServletRequest request, javax.servlet.http.HttpServletResponse response) throws javax.servlet.ServletException, IOException {

        this.doPost(request, response);
    }
}


复制代码
```

## 效果：



![这里写图片描述](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1280" height="742"></svg>)



## XML方式总结

- **监听下拉框的变化，如果变化了，那么就使用异步操作去访问服务器，得到对应的数据返回给异步对象**
- 异步对象解析服务器带过来的数据，使用DOM编程把数据动态添加到页面上
  - **在Servlet上记得要指定返回的是XML的数据！**
  - **在前台解析XML文档的时候，不能直接使用innerHtml来得到节点的值，只能通过firstChild.nodeValue的方式获取。**
  - **由于每次append到下拉框都会连续append，因此在响应事件的时候，把下拉框清零**
  - **把下拉框options的长度赋值为1，那么就是清零的操作了**。

------

# AJAX二级下拉联动案例【JSON版】

前面我们已经使用过了XML作为数据载体在AJAX中与服务器进行交互。当时候我们的案例是二级联动，使用Servlet进行控制

这次我们使用JSON作为数据载体在AJAX与服务器交互，使用三级联动，使用Action进行控制....

- **省份-城市-区域三级联动【Struts2 + JSON版】**

------

## 分析

与上次是一样的，只不过这次换了用JSON，使用Action控制罢了...

**监听下拉框的变动，使用异步对象与服务器进行交互。**

### 前台分析

- **监听下拉框的变动**
- **得到服务器返回的JSON数据**
- **使用eval()进行解析，得到具体的对象**
- **使用DOM编程把数据填充到对应的下拉框上**

### 后台分析

- **得到前台发送过来的数据**
- **判断具体的数据是什么，给出对应的数据**
- **使用Struts2提供的组件把数据封装成JSON**
- **返回给浏览器**

------

## 监听省份JSP页面

```
<%--
  Created by IntelliJ IDEA.
  User: ozc
  Date: 2017/5/18
  Time: 13:36
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>使用JSON数据载体与服务器进行交互</title>

      <script type="text/javascript" src="js/ajax.js"></script>
  </head>
  <body>


  <%--############前台页面##############################--%>
  <select name="province" id="provinceId">
    <option>请选择省份</option>
    <option>广东</option>
    <option>北京</option>
  </select>

  <select name="city" id="cityId">
    <option>请选择城市</option>
  </select>


  <select name="area" id="areaId">
    <option>请选择区域</option>
  </select>

  <%--############监听省份##############################--%>
  <script type="text/javascript">

      document.getElementById("provinceId").onchange= function () {

          // 得到选中的下拉框的值
          var provinceValue = this.options[this.selectedIndex].innerHTML;


          /***************ajax代码*************************/
          if("请选择省份" != provinceValue) {

              //每次访问的时候，都要清空select的值
              var citySelect = document.getElementById("cityId");
              citySelect.options.length = 1;

              var ajax = createAJAX();
              var method = "post";
              var url = "${pageContext.request.contextPath}/province_findCityByProvince?time=" + new Date().getTime();
              ajax.open(method, url);
              ajax.setRequestHeader("content-type", "application/x-www-form-urlencoded");

              //顾及到发送的key、value值有很多，于是我们使用对象吧。
              ajax.send("bean.name=" + provinceValue);

              /***************等待服务器的响应，得到服务器返回的数据************************/
              ajax.onreadystatechange = function () {

                  if(ajax.readyState==4) {
                      if(ajax.status==200) {
                        var jsonJava = ajax.responseText;

                        //解析成是JS类型的JSON
                        var json = eval("(" + jsonJava + ")");

                        //得到每个城市的值
                        for(var i=0;i<json.city.length;i++) {
                          var city = json.city[i];

                          //动态创建option控件
                          var option = document.createElement("option");
                          option.innerHTML = city;

                          citySelect.appendChild(option);
                        }
                      }
                  }
              };

          }

      };
    
  </script>

  </body>
</html>

复制代码
```

## 监听省份Action

**要想Struts2能够把Action的数据封装成JSON，就需要导入Struts2的开发包**

- **struts2-json-plugin-2.3.4.1.jar**



![这里写图片描述](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1048" height="477"></svg>)



**在Action中对应的成员属性需要给getter方法**

```
import com.opensymphony.xwork2.ActionSupport;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by ozc on 2017/5/18.
 */
public class ProvinceAction  extends ActionSupport{

    //自动封装数据
    private Bean bean;
    public Bean getBean() {
        return bean;
    }
    public void setBean(Bean bean) {
        this.bean = bean;
    }

    //封装城市的集合
    private List<String> city = new ArrayList<>();
    public List<String> getCity() {
        return city;
    }

    public String findCityByProvince() throws Exception {

        if ("广东".equals(bean.getName())) {
            city.add("广州");
            city.add("珠海");
            city.add("从化");
        } else if ("北京".equals(bean.getName())) {
            city.add("一环");
            city.add("二环");
            city.add("三环");
            city.add("四环");

        } else {
            System.out.println("没有你选择的地区");

        }
        return "ok";
    }
}

复制代码
```

**返回给前端的时候，数据是这样子的：**



![这里写图片描述](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="937" height="573"></svg>)



------

## 效果



![这里写图片描述](https://user-gold-cdn.xitu.io/2018/2/13/1618f949decd7ffc?imageslim)



------

## 监听城市JSP

```
  <%--############监听城市##############################--%>

  <script type="text/javascript">
      document.getElementById("cityId").onchange = function () {

          //清空值
          var areaSelect = document.getElementById("areaId");
          areaSelect.options.length = 1;

          //得到选择选中的下拉框的值
          var cityValue = this.options[this.selectedIndex].innerHTML;
          if(cityValue!="请选择城市"){

              var ajax = createAJAX();
              var method = "post";
              var url = "${pageContext.request.contextPath}/province_findAreaByCity?time=" + new Date().getTime();
              ajax.open(method, url);
              ajax.setRequestHeader("content-type", "application/x-www-form-urlencoded");

              //顾及到发送的key、value值有很多，于是我们使用对象吧。
              ajax.send("bean.name=" + cityValue);

              /***************等待服务器的响应，得到服务器返回的数据************************/
              ajax.onreadystatechange = function () {

                  if(ajax.readyState==4) {
                      if(ajax.status==200) {

                          var jsonJava = ajax.responseText;

                          var json = eval("(" + jsonJava + ")");

                          //得到每个地区的值
                          for(var i=0;i<json.area.length;i++) {
                              var area = json.area[i];

                              //动态创建option控件
                              var option = document.createElement("option");
                              option.innerHTML = area;

                              areaSelect.appendChild(option);
                          }

                      }
                  }
              }

          };
      };

  </script>


复制代码
```

## Action页面

```
    public String findAreaByCity() throws Exception {

        if ("广州".equals(bean.getName())) {
            area.add("白云区");
            area.add("黄浦区");
            area.add("萝岗区");
        } else if ("珠海".equals(bean.getName())) {
            area.add("香江");
            area.add("拱北");
            area.add("EE");
            area.add("xx");
        } else {
            System.out.println("没有你选择的地区");

        }
        return "ok";
    }
复制代码
```

## 最终效果：



![这里写图片描述](https://user-gold-cdn.xitu.io/2018/2/13/1618f949badf7a75?imageslim)



## 总结

这次使用的是JSON作为数据载体与服务器进行交互，和XML本质上是没有区别的。

**只不过JSON是更加轻量级文本数据，在JavaScript能够方便地获取返回的数据**

- 在Struts2中把Action数据封装成JSON格式，返回给异步对象
  - **需要导入jar包**
  - **在配置文件中配置继承json包**
  - **返回的类型是json**
- **如果使用POST时，发送的key、vaulue太多的话，我们可以使用bean进行封装**
- **当选中省份时，把城市和区域的下拉框清空，当选择城市时，把区域的下拉框清空**

------

\#总结图#



![这里写图片描述](https://user-gold-cdn.xitu.io/2018/2/13/1618f949df250050?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)


作者：Java3y
链接：https://juejin.im/post/5a82f6e86fb9a0633f0e1f2a
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。