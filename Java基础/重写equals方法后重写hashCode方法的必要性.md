## **1、首先我们看看对象默认的（Object）的equals方法和hashcode方法**

```text
public booleanequals(Object obj) {
return(this== obj);
}
public native inthashCode();
```

对象在不重写的情况下使用的是Object的equals方法和hashcode方法，从Object类的源码我们知道，默认的equals 判断的是两个对象的引用指向的是不是同一个对象；而hashcode也是根据对象地址生成一个整数数值；



![img](https://pic1.zhimg.com/80/v2-27b993f8ee6ae4771ee9777c625561a0_hd.jpg)









## **2、重写equals**

案例场景：

定义一个User对象有多个属性值姓名、年龄、身份证；



我们写代码的时候会发现，两个new 出来的User()对象 无论他们的的各项值是否一样两个对象equals 永远都是false，两个对象值完全一样放到HashSet里面它会把这两个值完全一样的对象当成两个不同的对象了，这样的话好像HashSet的特性就丢失了；



其实原因就是我们没有重写User 的equals方法，它会调用Object的equals方法，就如上图一样，Object的equals方法是比较对象的引用对象是否是同一个，两个new出来的对象当然不一样。



好了现在需求来了，我们需要两个对象的各项属性值一样的就认为这两个对象是相等的；那么此时我们就需要重写equals方法了；

代码如下：

```text
public classUser {
privateStringname;//姓名
privateStringIdCard;//身份证
private intage;//年龄
/**
* 重写equals
*@paramobj
*@return
*/
@Override
public boolean equals(Object obj) {
 if (obj instanceof User) {
        User user = (User) obj;
 if (user.getIdCard().equals(this.IdCard) && user.getName().equals(this.name) &&      user.getAge() == this.age) {
 return true;
        } else {
 return false;
        }
    } else {
 return false;
    }
}
//......省略N行代码
}
```

**那么现在关键的地方来了：**现在我们重写了User对象的equals方法，但并没有重写hashcode方法。

**（1）首先测试下equals的正确性**

```text
User user1=newUser();
user1.setName("路西");
user1.setAge(18);
user1.setIdCard("430");
User user2=newUser();
user2.setName("路西");
user2.setAge(18);
user2.setIdCard("430");
System.out.println("user1.equals(user2)="+user1.equals(user2));
user1.equals(user2)测试结果为true;
```

**(2)在两个对象equals的情况下进行把他们分别放入Map和Set中**

在上面的代码基础上追加如下代码：

```text
Set set =newHashSet();
set.add(user1);
set.add(user2);
Map map=newHashMap();
map.put(user1,"user1");
map.put(user2,"user2");
System.out.println("set 长度"+set.size());
System.out.println("map 长度"+map.keySet().size());;
```

测试打印结果为：



![img](https://pic3.zhimg.com/80/v2-304f188f13a4ce4920b49ed8f5b15112_hd.jpg)









好了现在问题来了，明明user1和user2两个对象是equals的那么为什么把他们放到set中会有两个对象（Set特性是不允许重复数据的），还有Map也把两个同样的对象当成了不同的Key（Map的Key是不允许重复的，相同Key会覆盖）；

**（3）这里我先抛出结果，至于原理后面再进行描述**

原因是user1和user2的hashcode 不一样导致的；

![img](https://pic4.zhimg.com/80/v2-38e029a15fe2a9959448ce00c47c8b83_hd.jpg)

因为我们没有重写父类（Object）的hashcode方法,Object的hashcode方法会根据两个对象的地址生成对相应的hashcode；

user1和user2是分别new出来的，那么他们的地址肯定是不一样的，自然hashcode值也会不一样。

Set区别对象是不是唯一的标准是，两个对象hashcode是不是一样，再判定两个对象是否equals;

Map 是先根据Key值的hashcode分配和获取对象保存数组下标的，然后再根据equals区分唯一值（详见下面的map分析）



## **3、重写hashcode方法；**

```text
public classUser  {
privateStringname;//姓名
privateStringIdCard;//身份证
private intage;//年龄
/**
* 重写equals
*@paramobj
*@return
*/
@Override
public boolean equals(Object obj) {
 if (obj instanceof User) {
        User user = (User) obj;
 if (user.getIdCard().equals(this.IdCard) && user.getName().equals(this.name) && user.getAge() == this.age) {
 return true;
        } else {
 return false;
        }
    } else {
 return false;
    }
}

@Override
public int hashCode() {
 int result = name.hashCode();
    result = 31 * result + IdCard.hashCode();
    result = 31 * result + age;
 return result;
}
//......省略N行代码
}
```

我们按之前的流程重新测试一遍结果：

```text
User user1=newUser();
user1.setName("路西");
user1.setAge(18);
user1.setIdCard("430");
User user2=newUser();
user2.setName("路西");
user2.setAge(18);
user2.setIdCard("430");
System.out.println("user1.equals(user2)="+user1.equals(user2));
Set set =newHashSet();
set.add(user1);
set.add(user2);
Map map=newHashMap();
map.put(user1,"user1");
map.put(user2,"user2");
System.out.println("set 长度"+set.size());
System.out.println("map 长度"+map.keySet().size());;
System.out.println("user1的hashcode"+user1.hashCode());
System.out.println("user2的hashcode"+user2.hashCode());
```

打印结果：

![img](https://pic3.zhimg.com/80/v2-04781d020b63164c4e42c38e6695509a_hd.jpg)



## **4、补充HashMap知识**

hashMap组成结构：hashMap是由数组和链表组成；

hashMap的存储：一个对象存储到hashMap中的位置是由其key 的hashcode值决定的;查hashMap查找key: 找key的时候hashMap会先根据key值的hashcode经过取余算法定位其所在数组的位置，再根据key的equals方法匹配相同key值获取对应相应的对象；

案例：

**（1）hashmap存储**

存值规则：把Key的hashCode 与HashMap的容量 取余得出该Key存储在数组所在位置的下标**（源码定位Key存储在数组的哪个位置是以hashCode & (HashMap容量-1)算法得出）**这里为方便理解使用此方式；

//为了演示方便定义一个容量大小为3的hashMap（其默认为16）

HashMap map=newHashMap(3);

map.put("a",1); 得到key 为“a” 的hashcode 值为97然后根据 该值和hashMap 容量取余97%3得到存储位到数组下标为1;

map.put("b",2); 得到key 为“b” 的hashcode 值为98,98%3到存储位到数组下标为2;

map.put("c",3); 得到key 为“c” 的hashcode 值为99，99%3到存储位到数组下标为0;

map.put("d",4); 得到key 为“d” 的hashcode 值为100，100%3到存储位到数组下标为1;

map.put("e",5); 得到key 为“e” 的hashcode 值为101，101%3到存储位到数组下标为2;

map.put("f",6); 得到key 为“f” 的hashcode 值为102，102%3到存储位到数组下标为0;

![img](https://pic2.zhimg.com/80/v2-fceb57a8de2759203613d5bd343dc37d_hd.jpg)



**（2）hashmap的查找key**

得到key在数组中的位置：根据上图，当我们获取key 为“a”的对象时，那么我们首先获得 key的hashcode97%3得到存储位到数组下标为1;

匹配得到对应key值对象：得到数组下表为1的数据“a”和“c”对象， 然后再根据 key.equals()来匹配获取对应key的数据对象；

hashcode 对于HashMapde:如果没有hashcode 就意味着HashMap存储的时候是没有规律可寻的，那么每当我们map.get()方法的时候，就要把map里面的对象一一拿出来进行equals匹配，这样效率是不是会超级慢；



## **5、hashcode方法文档说明**

在equals方法没被修改的前提下，多次调用同一对象的hashcode方法返回的值必须是相同的整数；

如果两个对象互相equals，那么这两个对象的hashcode值必须相等；

为不同对象生成不同的hashcode可以提升哈希表的性能；