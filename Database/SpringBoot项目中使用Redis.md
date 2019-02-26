# SpringBoot项目中使用Redis
## 个人对redis的理解
Redis是一个基于键值对的数据库，由于它讲数据保存到内存中，因此速度特别快，当然它也会将数据持久化到磁盘中。
## 
## RedisTemplate，StringRedisTemplate
**为了方便我们对redis进行操作，spring搞了两个类 RedisTemplate，StringRedisTemplate**
1. RedisTemplate<k,v> 也就是说存放的key和value都是对象；而StringRedisTemplate，则key和value都是String
RedisTemplate的操作，set的时候是将k和v都进行序列化然后储存，比如
```java
redisTemplate.opsForValue().set("kkk",new User("kkkk"));
```
进入DefaultValueOperations的set方法
```java
    public void set(K key, V value) {

        byte[] rawValue = rawValue(value);
        execute(new ValueDeserializingRedisCallback(key) {

            @Override
            protected byte[] inRedis(byte[] rawKey, RedisConnection connection) {
                connection.set(rawKey, rawValue);
                return null;
            }
        }, true);
    }
```1. rawValue(value)会将value进行序列化。变成byte[]。

首先看value是不是byte[]的子类，若是直接强转返回byte[]。否则调用interface RedisSerializer 的serialize() 方法。该接口有5个实现类，默认会调用JdkSerializationRedisSerializer类的实现。

缺点是必须要value实现了 Serializable接口，否则GG
```java
    public void serialize(Object object, OutputStream outputStream) throws IOException {
    if (!(object instanceof Serializable)) {
        throw new IllegalArgumentException(getClass().getSimpleName() + " requires a Serializable payload " +
                "but received an object of type [" + object.getClass().getName() + "]");
    }
    ObjectOutputStream objectOutputStream = new ObjectOutputStream(outputStream);
    objectOutputStream.writeObject(object);
    objectOutputStream.flush();
}
```
然后完成value的序列化。
之后也会调用JdkSerializationRedisSerializer对key,"kkk"进行序列化，最终会调用DefaultedRedisConnection的下面方法完成set操作
```
default Boolean set(byte[] key, byte[] value) {
        return stringCommands().set(key, value);
    }
```
2. StringRedisTemplate