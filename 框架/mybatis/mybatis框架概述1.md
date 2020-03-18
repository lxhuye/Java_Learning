# mybatis框架概述

## 一、mybatis入门

```
mybatis的概述
mybatis的环境搭建
mybatis入门案例
自定义mybatis框架（主要的目的是为了让大家了解mybatis中执行细节）
复制代码
```

## 二、mybatis基本使用

```
mybatis的单表crud操作
mybatis的参数和返回值
mybatis的dao编写
mybatis配置的细节
	几个标签的使用
复制代码
```

------

**1、什么是框架？**

```
它是我们软件开发中的一套解决方案，不同的框架解决的是不同的问题。
使用框架的好处：
	框架封装了很多的细节，使开发者可以使用极简的方式实现功能。大大提高开发效率。
复制代码
```

**2、三层架构**

```
表现层：
	是用于展示数据的
业务层：
	是处理业务需求
持久层：
	是和数据库交互的
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/11/5/16e3ab5a23825e9d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 3、持久层技术解决方案

```
JDBC技术：
	Connection
	PreparedStatement
	ResultSet
Spring的JdbcTemplate：
	Spring中对jdbc的简单封装
Apache的DBUtils：
	它和Spring的JdbcTemplate很像，也是对Jdbc的简单封装

以上这些都不是框架
	JDBC是规范
	Spring的JdbcTemplate和Apache的DBUtils都只是工具类
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/11/5/16e3ab7e03048fad?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 4、mybatis的简介

```
mybatis是一个持久层框架，用java编写的。
它封装了jdbc操作的很多细节，使开发者只需要关注sql语句本身，而无需关注注册驱动，创建连接等繁杂过程
它使用了ORM思想实现了结果集的封装。

ORM：
	Object Relational Mappging 对象关系映射
	简单的说：
		就是把数据库表和实体类及实体类的属性对应起来
		让我们可以操作实体类就实现操作数据库表。

		user			User
		id			userId
		user_name		userName
复制代码
```

## 5、mybatis的入门

```
mybatis的环境搭建
	第一步：创建maven工程并导入坐标
	第二步：创建实体类和dao的接口
	第三步：创建Mybatis的主配置文件
			SqlMapConifg.xml
	第四步：创建映射配置文件
			IUserDao.xml
环境搭建的注意事项：
	第一个：创建IUserDao.xml 和 IUserDao.java时名称是为了和我们之前的知识保持一致。
		在Mybatis中它把持久层的操作接口名称和映射文件也叫做：Mapper
		所以：IUserDao 和 IUserMapper是一样的
	第二个：在idea中创建目录的时候，它和包是不一样的
		包在创建时：com.itheima.dao它是三级结构
		目录在创建时：com.itheima.dao是一级目录
	第三个：mybatis的映射配置文件位置必须和dao接口的包结构相同
	第四个：映射配置文件的mapper标签namespace属性的取值必须是dao接口的全限定类名
	第五个：映射配置文件的操作配置（select），id属性的取值必须是dao接口的方法名

	当我们遵从了第三，四，五点之后，我们在开发中就无须再写dao的实现类。
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/11/5/16e3ab907e543070?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

mybatis的创建使用了三种设计模式



```
1、建造者模式
2、工厂模式
3、代理模式
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/11/5/16e3abac2dc72f3c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

mybatis实现增删改查的底层原理



实际上底层通过代理对象（sqlSession）调用selectList方法实现的，对JDBC底层一些细节做了封装。



![img](https://user-gold-cdn.xitu.io/2019/11/5/16e3abc2a7e0aa95?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

一张图让你清楚mybatis的底层执行流程





![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>)

分析代理dao的执行过程（开发中经常使用代理对象执行方法，不需要写dao的实现类）





![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="410" height="1280"></svg>)

分析编写dao实现类mybatis的执行过程（比较麻烦，了解即可）





![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1280" height="382"></svg>)

 mybatis的入门案例 第一步：读取配置文件 第二步：创建SqlSessionFactory工厂 第三步：创建SqlSession 第四步：创建Dao接口的代理对象 第五步：执行dao中的方法 第六步：释放资源



```
	注意事项：
		不要忘记在映射配置中告知mybatis要封装到哪个实体类中
		配置的方式：指定实体类的全限定类名
	
	mybatis基于注解的入门案例：
		把IUserDao.xml移除，在dao接口的方法上使用@Select注解，并且指定SQL语句
		同时需要在SqlMapConfig.xml中的mapper配置时，使用class属性指定dao接口的全限定类名。
明确：
	我们在实际开发中，都是越简便越好，所以都是采用不写dao实现类的方式。
	不管使用XML还是注解配置。
	但是Mybatis它是支持写dao实现类的。
复制代码
```



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1280" height="494"></svg>)



## 6、自定义Mybatis的分析：

```
mybatis在使用代理dao的方式实现增删改查时做什么事呢？
	只有两件事：
		第一：创建代理对象
		第二：在代理对象中调用selectList
	
自定义mybatis能通过入门案例看到类
	class Resources
	class SqlSessionFactoryBuilder
	interface SqlSessionFactory
	interface SqlSession
复制代码
```



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1280" height="606"></svg>)





![img](https://user-gold-cdn.xitu.io/2019/11/5/16e3acc9ffeb2c58?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 写在最后

作者：Lmobject

链接：https://juejin.im/post/5dc13a02e51d45446159389a

来源：掘金

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。