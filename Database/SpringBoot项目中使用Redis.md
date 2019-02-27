# SpringBoot项目中使用Redis
## 个人对redis的理解
Redis是一个基于键值对的数据库，由于它讲数据保存到内存中，因此速度特别快，当然它也会将数据持久化到磁盘中。
 
## 如何在SpringBoot项目中使用Redis
需要在SpringBoot项目中加入以下依赖
``` java
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
            <version>2.0.0.RELEASE</version>
</dependency>
```
Spring为方便我们使用，封装了两个类RedisTemplate，StringRedisTemplate
StringRedisTemplate是继承RedisTemplate的
RedisTemplate中注入了两部分，**序列化策略**，**对不同类型数据的存取**
### **序列化策略**
RedisSerializer是一个接口，spring提供了多种实现 

1. 如果不指定RedisSerializer的实现，
默认采用JdkSerializationRedisSerializer的序列化，不需要无参构造函数，但需要implements Serializable。
用的是将对象转成字节输出流，然后写到byte[]的方式。

2. 若指定StringRedisSerializer，则key和value都必须是String类型，储存的就是原始的字符串

3. 若指定Jackson2JsonRedisSerializer，序列化为json字符串，但是再反序列化的时候会出错

4. 若指定GenericToStringSerializer，序列化为json字符串，没什么问题，但是需要无参构造函数

### **对不同类型数据的存取**
为了操作字符串，散列，列表，集合.RedisTemplate提供了多种Operations，注入不同的Operations能够完成不同数据类型的操作。
 
