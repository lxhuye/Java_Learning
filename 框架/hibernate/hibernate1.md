## **一、JavaEE三层架构小说明**

![img](https://s1.ax1x.com/2018/12/11/FJTGQK.png)

[回到顶部](https://www.cnblogs.com/chenmingjun/p/9285046.html#_labelTop)

## **二、Hibernate入门**



### **2.1、ORM（持久层）框架**

> **ORM**
>   对象关系映射（英语：(**Object Relational Mapping**，简称**ORM**，或**O/RM**，或**O/R mapping**），是一种程序技术，用于实现面向对象编程语言里不同类型系统的数据之间的转换 。
>
> 　　从效果上说，它其实是创建了一个可在编程语言里使用的“虚拟对象数据库”。
>   面向对象是从`软件工程`基本原则（如耦合、聚合、封装）的基础上发展起来的，而关系数据库则是从数学理论发展而来的，两套理论存在显著的区别。为了解决这个不匹配的现象，对象关系映射技术应运而生。
>   对象关系映射（Object-Relational Mapping）提供了概念性的、易于理解的模型化数据的方法。ORM方法论基于三个核心原则：
>
> 　　　　**简单**：以最基本的形式建模数据。
>
> 　　　　**传达性**：数据库结构被任何人都能理解的语言文档化。
>
> 　　　　**精确性**：基于数据模型创建正确标准化的结构。
>
> 　　典型地，建模者通过收集来自那些熟悉应用程序但不熟练的数据建模者的人的信息开发信息模型。
>
> 　　建模者必须能够用非技术企业专家可以理解的术语在概念层次上与数据结构进行通讯。
>
> 　　建模者也必须能以简单的单元分析信息，对样本数据进行处理。ORM专门被设计为改进这种联系。
>   简单的说：`ORM相当于中继数据`。具体到产品上，例如ADO.NET Entity Framework。DLINQ中实体类的属性[Table]就算是一种中继数据。

  Hibernate：是一个`数据持久化层`的ORM框架。
  Object：对象，java对象，此处特指JavaBean。
  Relational：关系，二维表，数据库中的表。
  Mapping：映射|映射元数据，`对象中属性`与`表的字段`存在的`对应关系`。

![img](https://s1.ax1x.com/2018/12/11/FJTsQf.png)

### **2.2、什么是Hibernate？**

- Hibernate 是`轻量级`JavaEE应用的持久层解决方案，是一个`关系数据库`ORM框架。
  `ORM 就是通过将Java对象映射到数据库表，通过操作Java对象，就可以完成对数据表的操作。`
- Hibernate 提供了对关系型数据库增删改查操作。



### **2.3、主流的ORM框架**

- JPA： Java Persistence API，JPA通过`JDK 5.0注解`或`XML描述对象--关系表的映射关系`。（只有接口规范）
- Hibernate：是最流行的`全自动`ORM框架，通过`对象关系--映射配置`，可以完全脱离底层SQL。（理论上来讲，就是不用写sql语句了）
- MyBatis：本是apache的一个开源项目iBatis，支持`普通SQL查询`、`存储过程`和`高级映射`的优秀持久层框架。是`半自动`ORM框架。
- Apache的DBCPUtil（QueryRunner）、JNDIUtil（QueryRunner）、Spring的JDBCTemplate等等。



### **2.4、Hibernate的优点**

- Hibernate对JDBC访问数据库的代码做了`封装`，大大`简化`了数据访问层繁琐的`重复性代码`。
- Hibernate是一个基于jdbc的主流持久化框架，是一个优秀的orm实现，它很大程度的简化了dao层编码工作。
- Hibernate使用java的`反射`机制。
- Hibernate的性能非常好，因为它是一个`轻量级`框架。`映射的灵活性`很出色。它支持很多关系型数据库，从一对一到多对多的各种复杂关系。

[回到顶部](https://www.cnblogs.com/chenmingjun/p/9285046.html#_labelTop)

## **三、Hibernate入门案例【掌握】**



### **3.1、编写流程**

1. 新建项目
2. 导入jar包
3. 创建数据库和表
4. 编写JavaBean和相应的映射文件`hibernate mapping(*.hbm.xml)`
5. 编写核心配置文件（`hibernate.cfg.xml`）--> 配置获取连接等参数
6. 使用api测试



### **3.2、设计数据库和表**

```sql
CREATE DATABASE day29;

USER day29;

CREATE TABLE t_user(
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(20),
  password VARCHAR(20)
);
```



### **3.3、导入jar包**

版本：3.6.10 --> hibernate 4 建议注解开发，hibernate 4 对 3 不兼容。
目录结构：

![img](https://s1.ax1x.com/2018/12/11/FJT3z6.png)
jar介绍：
![img](https://s1.ax1x.com/2018/12/11/FJT1Rx.png)
项目中的lib结构：
![img](https://s1.ax1x.com/2018/12/11/FJTlJ1.png)

### **3.4、编写JavaBean + 相应的映射文件**

User.java

```
package com.itheima.a_hello;

public class User {
    private Integer id;
    private String name;
    private String password;

    public Integer getId() {
        return id;
    }
    public void setId(Integer id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getPassword() {
        return password;
    }
    public void setPassword(String password) {
        this.password = password;
    }
}
```

  相应的映射文件位置：JavaBean同包
  相应的映射文件名称：JavaBean同名
  相应的映射文件扩展名：*.hbm.xml
具体内容如下：
  先添加约束

![img](https://s1.ax1x.com/2018/12/11/FJTJsO.png)
  我们只要约束的代码，为了引入。约束代码如下：

```xml
<!DOCTYPE hibernate-mapping PUBLIC 
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
```

截图如下：

![img](https://s1.ax1x.com/2018/12/11/FJTYLD.png)
截图如下：
![img](https://s1.ax1x.com/2018/12/11/FJTNee.png)
截图如下：
![img](https://s1.ax1x.com/2018/12/11/FJTUdH.png)

### **3.5、编写核心配置文件hibernate.cfg.xml**

  位置：类路径（classpath、src）--> 或者WEB-INF/classes
  名称：hibernate.cfg.xml
具体内容如下：
  先添加约束
  我们只要约束的代码，为了引入。约束代码如下：

```
<!DOCTYPE hibernate-configuration PUBLIC
    "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
```

截图如下：

![img](https://s1.ax1x.com/2018/12/11/FJTaod.png)

### **3.6、测试**

我们接下来使用junit进行单元测试

```java
public class Lesson2{
  @Test
  public void test1(){
    //保存用户数据
    //1.获取核心配置文件对象,默认是加载src的hibernate.cfg.xm文件
    //2.创建会话工厂
    //3.创建会话
    	//开启事务
    	//保存
    	//提交事务
    //4.关闭会话
    //5.关闭工厂,释放资源
  }
}
```





核心配置文件`hibernate.cfg.xml`中没有配置自动提交的结果：

![img](https://s1.ax1x.com/2018/12/11/FJTwFA.png)
刷新数据库，发现数据并没有提交，那我们就配置上事务控制，再看看：
截图如下：
![img](https://s1.ax1x.com/2018/12/11/FJT0JI.png)
截图如下：
![img](https://s1.ax1x.com/2018/12/11/FJTfFs.png)
  哈哈，添加成功了。
注意：也可以不用在核心配置文件`hibernate.cfg.xml`中添加事务控制的配置，可以在测试类代码中直接添加事务控制代码，如下图所示：
![img](https://s1.ax1x.com/2018/12/11/FJTBWt.png)
截图如下：
![img](https://s1.ax1x.com/2018/12/11/FJTyy8.png)
  哈哈，也添加成功了。

### **3.7、常见异常**

![img](https://s1.ax1x.com/2018/12/11/FJTrSP.png)
解决方案：
  将映射文件添加到核心配置文件中 hbm.xml --> hibernate.cfg.xml
![img](https://s1.ax1x.com/2018/12/11/FJT6OS.png)



## **四、Hibernate的api详解【多练】**



### **4.1、体系结构**

![img](https://s1.ax1x.com/2018/12/11/FJTIS0.png)
  **PO**：Persistent Object 持久化对象，用于与数据库交互数据。dao层（JavaBean + hbm ）
  **BO**：Business Object 业务数据对象。service层。
  **VO**：Value Object 值对象。web层。
**开发中：直接使用 JavaBean 来描述这三个对象。**

### **4.2、Configuration 配置对象**

- Hibernate 的核心，配置文件种类：
  `hibernate.cfg.xml` 通常使用 xml配置文件，可以配置内容更丰富。
  `hibernate.properties` 用于配置 key/value 形式的内容，key不能重复的。配置有很多的局限性。一般不用。
  参考文件所在位置：`hibernate-distribution-3.6.10.Final\project\etc\hibernate.properties`
  提供了核心配置文件常用的配置项及选择参数。
  1、提供构造方法`new Configuration();` hibernate将自动加载`hibernate.properties`文件。
    hibernate.properties文件必须存放在类路径（src）下。
  2、提供无参方法`configure();`将加载src下的`hibernate.cfg.xml`。
  3、扩展api，提供含参方法
    `configure(String);`加载指定目录下的`hibernate.cfg.xml`文件
  4、手动加载配置文件

```
// 手动加载指定的配置文件
conf.addResource("com/itheima/a_hello/User.hbm.xml");
// 手动加载指定类对应的映射文件：User --> User.hbm.xml
conf.addClass(User.class);
```

示例代码如下图所示：

![img](https://s1.ax1x.com/2018/12/11/FJTgeg.png)
开发中：将`hbm.xml映射`配置到`hibernate.cfg.xml`中。
学习中：可以使用手动方式 `addResource` 或 `addClass`。

### **4.3、SessionFactory 工厂**

- SessionFactory：相当于java web的连接池，用于管理所有session。
  获得方式：`conf.buildSessionFactory();`
  SessionFactory：是Hibernate缓存配置信息（数据库配置信息、映射文件、预定义HQL语句等）
  SessionFactory：是线程安全，可以是成员变量，多个线程同时访问时，不会出现线程并发访问的问题。
  提供api：

```
// openSession => 获得一个全新的Session对象（新的一个），即打开一个新的会话Session
factory.openSession();
// getCurrentSession => 获得与当前线程绑定的Session对象（同一个），即获得当前线程中绑定的会话Session
factory.getCurrentSession();
Hibernate支持，将创建的session绑定到本地线程中，底层使用ThreadLocal，在程序之间共享Session。
1、调用getCurrentSession(); 必须在hibernate.cfg.xml中进行如下配置：
    <!-- current_session_context_class：表示将创建的Session与本地线程绑定 ，只有配置了次数，才能使用getCurrentSess();-->
    <property name="hibernate.current_session_context_class">thread</property>
2、如果提交或回滚事务，底层将自动关闭Session。
```

示例代码如下图所示：

![img](https://s1.ax1x.com/2018/12/11/FJT2wQ.png)

### **4.4、Session 会话**

- Session 相当于 JDBC的 Connection => 会话
  通过Session操作PO对象 => 增删改查
  Session是单线程，线程不安全，不能编写成成员变量。
  Session 的api：
    session.save(Object); 保存
    session.update(Object); 更新
    session.delete(Object); 删除
    session.get(Class clazz, Serializable); 通过id查询，如果没有，则返回null
    session.load(Class clazz, Serializable); 通过id查询，如果没有，则抛出异常
    session.createQuery("hql"); 获得Query对象（hql语句）
    session.createCritieria(Class clazz); 获得Criteria对象（类对象）
    session.createSqlQuery("sql"); 获取SQLQuery对象（原生sql语句）

get()和load()的区别，如下图所示：

![img](https://s1.ax1x.com/2018/12/11/FJTRoj.png)

**小问题汇总并解答：**

- **1、load方法，会返回一个代理对象，在获得其内容(属性)时，会查询数据库，是每次访问属性都会查询数据库吗？**
  **答：**不是每次都查。代理对象中有一个标识：是否被初始化的boolean型变量，记录着是否被初始化过，确保只会初始化一次。
- **2、代理都是要基于接口的，用load方法返回的代理，就没有实现任何接口吗？**
  **答：** java中的动态代理是基于接口的。而 Hibernate 是使用javassist-3.12.0.GA.jar 产生动态代理对象的。
     该代理与被代理对象之间的关系是继承关系，与我们学的动态代理不是一种。所以不需要接口。



### **4.5、Transaction 事务**

Session对象：控制如何开启事务
  开启事务：beginTransaction();
  获得事务：getTransaction(); 获取已经开启的事务对象：即在一个Dao中获取另一个Dao中的Transaction事务对象。(开发中很少用，因为事务往往是统一控制的。)
Transaction对象：控制如何关闭事务
  提交事务：commit();
  回滚事务：rollback();

```
try {
    // 开启事务
    // session操作
    // 提交事务
} catch(e) {
    // 回滚事务
}
扩展：不需要手动的管理事务，之后所有的事务管理都交予Spring统一管理了。
```

示例代码如下图所示：

![img](https://s1.ax1x.com/2018/12/11/FJThYn.png)

### **4.6、Query 对象**

- Hibernate执行hql语句
  hql语句：hibernate提供面向对象查询语句，使用对象（类）和属性进行查询。区分大小写。
  获得Query对象方式：session.createQuery("hql");
  Query对象的方法：
    list(); 查询所有
    uniqueResult(); 获得一个结果。如果没有查询到就返回null，如果查询到多条就抛出异常。
    setFirstResult(int); 分页，开始索引数startIndex。
    setMaxResults(int); 分页，每页显示个数pageSize。

示例代码如下图所示：

![img](https://s1.ax1x.com/2018/12/11/FJTolV.png)

### **4.7、Criteria对象(了解)**

QBC（query by criteria），hibernate提供纯面向对象查询语言，提供直接使用PO对象进行操作。
获得Criteria对象方式：Criteria criteria = session.createCriteria(User.class);
条件：
`criteria.add(Restrictions.eq("username", "tom")); // 查找name属性值为tom的记录`
  Restrictions.gt(propertyName, value); 大于
  Restrictions.ge(propertyName, value); 大于等于
  Restrictions.lt(propertyName, value); 小于
  Restrictions.le(propertyName, value); 小于等于
  Restrictions.like(propertyName, value); 模糊查询，注意：模糊查询值需要使用 %

示例代码如下图所示：

![img](https://s1.ax1x.com/2018/12/11/FJTTyT.png)

### **4.8、工具类**

HibernateUtils.java

```
package com.itheima.utils;

import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;

// 完成Hibernate工具类
// 封装配置文件读取操作
// 封装SessionFactroy的创建操作
// 封装Session获取操作
public class HibernateUtils {
    // 会话工厂，整个程序只有一份。
    private static SessionFactory sessionFactory;

    // 静态代码块，放进程级别的操作
    static {
        // 1.读取配置文件对象获得核心配置对象
        Configuration conf = new Configuration().configure();
        // 2.通过配置文件对象，创建session工厂SessionFactory，相当于连接池
        sessionFactory = conf.buildSessionFactory(); // Ctrl+2进行接收快捷键
        // 4.关闭虚拟机时，释放SessionFactory
        Runtime.getRuntime().addShutdownHook(new Thread(new Runnable() { // 多线程中的匿名内部类
            @Override
            public void run() {
                System.out.println("虚拟机关闭!释放资源");
                sessionFactory.close();
            }
        }));
    }

    public static org.hibernate.Session openSession() {
        // 3.获得一个全新的Session对象（新的一个），即打开一个新的会话Session
        return sessionFactory.openSession();
    }

    public static org.hibernate.Session getCurrentSession() {
        // 3.获得与当前线程绑定的Session对象（同一个），即获得当前线程中绑定的会话Session
        return sessionFactory.getCurrentSession();
    }
}
```

测试类：

```
package com.itheima.utils;

public class Test {
    public static void main(String[] args) {
        System.out.println(HibernateUtils.openSession());
    }
}
```

输出结果为：

```
SessionImpl(PersistenceContext[entityKeys=[],collectionKeys=[]];ActionQueue[insertions=[] updates=[] deletions=[] collectionCreations=[] collectionRemovals=[] collectionUpdates=[]])
虚拟机关闭!释放资源
```

[回到顶部](https://www.cnblogs.com/chenmingjun/p/9285046.html#_labelTop)

## **五、核心配置文件详解**



### **5.1、详细配置【多读】**

hibernate.cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE hibernate-configuration PUBLIC
    "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
    <session-factory>
        <!-- 可以在hibernate.properties查询所需要的配置 -->

        <!-- 数据库基本信息配置：4项 -->
        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">root</property>
        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/day29</property>

        <!-- show_sql：表示操作数据库时，会向控制台打印sql语句 -->
        <property name="show_sql">true</property>

        <!-- format_sql：表示在向控制台打印sql语句之前，会将sql语句先格式化 -->
        <property name="format_sql">true</property>

        <!-- hbm2ddl.auto：表示自动生成表结构的策略的配置 
             update(最常用的取值): 如果当前数据库中不存在表结构，那么会自动创建表结构。
                    如果存在表结构，并且表结构与实体一致，那么不做修改。
                    如果存在表结构，并且表结构与实体不一致，那么会修改表结构，即通过hbm映射文件更新表(添加)。会保留原有列。
                    即：会自动创建表结构和自动维护表结构。
             create(很少)：无论是否存在表结构。每次启动Hibernate都会重新创建表结构(数据会丢失)。
             create-drop(极少)：无论是否存在表结构。每次启动Hibernate都会重新创建表结构，每次Hibernate运行结束时，删除表结构。
             validate(很少)：不会自动创建表结构。也不会自动维护表结构。Hibernate只校验表结构，如果表结构不一致将会抛出异常。
        -->
        <property name="hbm2ddl.auto">update</property>

        <!-- 方言：为不同的数据库，不同的版本，生成sql语句（DQL查询语句）提供依据  -->
        <!-- 数据库方言配置 ：org.hibernate.dialect.MySQLDialect (有三个方言，选择最短的) -->
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>

        <!--java web 6.0 存在一个问题：BeanFactory 空指针异常
            异常提示：org.hibernate.HibernateException: Unable to get the default Bean Validation factory
            解决方案：取消bean校验
        -->
        <property name="javax.persistence.validation.mode">none</property>

        <!-- autocommit：表示事务自动提交 -->        
        <property name="hibernate.connection.autocommit">true</property>

        <!-- current_session_context_class：表示将创建的Session与本地线程绑定 ，只有配置了此处，才能使用getCurrentSess(); 方法-->
        <property name="hibernate.current_session_context_class">thread</property>

        <!-- 添加ORM映射文件 ，填写src之后的路径-->
        <mapping resource="com/itheima/a_hello/User.hbm.xml"/>
    </session-factory>
</hibernate-configuration>
```

[回到顶部](https://www.cnblogs.com/chenmingjun/p/9285046.html#_labelTop)

## **六、Hibernate 中持久化类**



### **6.1、JavaBean的编写规则**

- 提供一个无参数的public访问控制符的构造器。
- 提供一个标识属性，映射数据表主键字段。
- 所有属性提供public访问控制符的set和get方法(JavaBean)。
- 标识属性应尽量使用基本数据类型的包装类型（因为基本数据类型有默认值，会给数据库造成误会）。
- 不要用final修饰实体（否则将无法生成代理对象，进行优化）。



### **6.2、持久化对象的唯一标识 OID**

- Java按地址区分同一个类的不同对象。
- 关系数据库用主键区分同一条记录。
- Hibernate使用OID来建立内存中的对象和数据库中记录的对应关系。
  **结论:** 对象的OID和数据库的表的主键对应。为保证OID的唯一性，应该让Hibernate来为OID赋值。



### **6.3、区分自然主键和代理主键**

- 主键需要具备: 不为空/不能重复/不能改变
  自然主键：在业务中，某个属性符合主键的三个要求，那么该属性可以作为主键列。
  代理主键：在业务中，不存符合以上3个条件的属性，那么就增加一个没有意义的列，作为主键。



### **6.4、基本数据与包装类型**

- 基本数据类型和包装类型对应hibernate的映射类型相同。
- 基本类型无法表达null、数字类型的默认值为0。
- 包装类默认值是null。当对于默认值有业务意义的时候需要使用包装类。



### **6.5、类型对应**

如下表所示：

| Java数据类型                       | Hibernate数据类型 | 标准SQL数据类型 (对于不同的DB可能有所差异) |
| :--------------------------------- | :---------------- | :----------------------------------------- |
| byte、java.lang.Byte               | byte              | TINYINT                                    |
| short、java.lang.Short             | short             | SMALLINT                                   |
| int、java.lang.Integer             | integer           | INGEGER                                    |
| long、java.lang.Long               | long              | BIGINT                                     |
| float、java.lang.Float             | float             | FLOAT                                      |
| double、java.lang.Double           | double            | DOUBLE                                     |
| java.math.BigDecimal               | big_decimal       | NUMERIC                                    |
| char、java.lang.Character          | character         | CHAR(1)                                    |
| boolean、java.lang.Boolean         | boolean           | BIT                                        |
| java.lang.String                   | string            | VARCHAR                                    |
| boolean、java.lang.Boolean         | yes/no            | CHAR(1)('Y'或'N')                          |
| boolean、java.lang.Boolean         | true/false        | CHAR(1)('Y'或'N')                          |
| java.util.Date、java.sql.Date      | date              | DATE                                       |
| java.util.Date、java.sql.Time      | time              | TIME                                       |
| java.util.Date、java.sql.Timestamp | timestamp         | TIMESTAMP                                  |
| java.util.Calendar                 | calendar          | TIMESTAMP                                  |
| java.util.Calendar                 | calendar_date     | DATE                                       |
| byte[]                             | binary            | VARBINARY                                  |
| java.lang.String                   | text              | CLOB                                       |
| java.io.Serializable               | serializable      | VARBINARY、BLOB                            |
| java.sql.Clob                      | clob              | CLOB                                       |
| java.sql.Blob                      | blob              | BLOB                                       |
| java.lang.Class                    | class             | VARCHAR                                    |
| java.util.Locale                   | locale            | VARCHAR                                    |
| java.util.TimeZone                 | timezone          | VARCHAR                                    |
| java.util.Currency                 | currency          | VARCHAR                                    |



### **6.6、普通属性**

示例代码：

```
<!-- ORM元数据：对象关系(表)映射文件 -->
<hibernate-mapping  package="com.itheima.a_hello">
    <!--package 用于配置PO类所在包
        例如： package="com.itheima.a_hello" 
        之后配置类名的时候可以使用简单类名了。
    -->

    <!-- 配置 PO类 和 表 之间的对应关系 -->
    <!-- 
        name：PO类全限定类名
            例如：name="com.itheima.a_hello.User"
            如果配置了package，name的取值可以是简单类名 name="Person"
        table：数据库对应的表名
        dynamic-insert="false" 是否支持动态生成insert语句，默认值是false
        dynamic-update="false" 是否支持动态生成update语句，默认值是false
            如果设置true，hibernate底层将判断提供的数据是否为null，如果为null，insert或update语句将没有此项。
     -->
    <class name="User" table="t_user" dynamic-insert="true" dynamic-update="true">

        <!-- 普通属性 -->
        <property name="name" column="name" type="string"></property>
        <property name="password" column="password"></property>
        <!-- 
            name        PO类的属性
            column      表中的列名，默认name的值相同
            length      列的长度。默认值：255
            precision   小数点后的位数
            scale       总位数

            not-null    指定属性的约束是否使用 非空
            unique      指定属性的约束是否使用 唯一
            access      设置映射使用PO类属性或字段
            property    使用PO类属性，必须提供setter、getter方法
            field       使用PO类字段，一般很少使用。
            insert      (一般不用)生成insert语句时，是否使用当前字段。
            update      (一般不用)生成update语句时，是否使用当前字段。
                        默认情况：hibernate生成insert或update语句，使用配置文件所有项

            type        表中列的类型。默认hibernate自己通过getter获得类型，一般情况下不用设置

            表达该属性的类型
            可以用三种方式指定属性:
                java类型            数据库类型指定       Hibernate类型指定
                java.lang.String    varchar             string              

                取值1： hibernate类型
                        string 字符串
                        integer 整形
                取值2： java类型 （全限定类名）
                        java.lang.String 字符串
                取值3：数据库类型
                        varchar(长度) 字符串
                        int 整形

                        <property name="birthday">
                            <column name="birthday" sql-type="datetime"></column>
                        </property>

                        javabean 一般使用类型 java.util.Date

                        jdbc规范提供3中
                            java类型              mysql类型
                            java.sql.Date       date
                            java.sql.time       time
                            java.sql.timestamp  timestamp
                            null                datetime

                            以上三个类型都是java.util.Date子类    

            注意：配置文件如果使用关键字，列名必须使用重音符。   
        -->
    </class>
</hibernate-mapping>
```



### **6.7、主键**

示例代码：

```
        <!-- 配置主键 -->
        <!--  
            name        实体中标识主键的属性名称
            access=""   设置使用属性还是字段(强烈推荐不要用)因为在操作属性时，会直接操作对应的字段，而不是操作get/set方法，破坏了面向对象的封装性（get/set方法中会有一些逻辑控制）
            column=""   主键在表中的列名
            length=""   表中列的数据长度
            type=""     类型
            unsaved-value   （不常用）指定主键是什么值时，才当做null来处理
        -->
        <id name="id" column="id" length="9" >
            <!--固定值：表示主键生成策略，如何生成主键 
                native：由数据库来维护主键（数据库中配置：主键自增）

                generator:主键生成策略
                1.increment 数据库自己生成主键，先从数据库中查询最大的ID值，将ID值加1作为新的主键，不建议使用，存在线程并发问题
                2.identity  依赖于数据库的主键自增功能
                3.sequence  序列，依赖于数据库中的序列功能(在Oracle才有序列功能)
                4.hilo      (纯了解，永远用不到)Hibernate自己实现序列的算法，自己生成主键(hilo算法 )
                5.native    自动根据数据库判断，三选一：identity|sequence|hilo
                6.uuid      生成32位的不重复随机字符串当做主键
                    以上1-6策略是代理主键，由hibernate维护。

                7.assigned  自己指定主键值，当表的主键是自然主键时使用
                    7策略是自然主键，由程序自己维护。
            -->
            <generator class="native"></generator>
        </id>
```