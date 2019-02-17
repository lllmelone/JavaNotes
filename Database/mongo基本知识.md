# mongo基本知识
## mongo的安装（linux）
1. 首先去官网下载，由于我是阿里云服务器，我直接在服务器上下载 
选择一个喜欢的文件夹就可以下载了，
```
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-4.0.6.tgz
```

2. 下载完就解压 
```
tar -xzf mongodb-linux-x86_64-4.0.6.tgz
```
 
3.  我的安装路径上/usr/local/software/mongodb-linux-x86_64-4.0.6，首先在其中新建两个文件夹，一个存放数据，一个存放日志信息。
```
cd /usr/local/software/mongodb-linux-x86_64-4.0.6
mkdir data
mkdior log
touch mongo.log
```
4. 启动mongo
```
cd bin 
./mongod --dbpath /usr/local/mongodb/data/ --logpath /usr/local/mongodb/log/mongodb.log 
./mongod --dbpath /usr/local/mongodb/data/ --logpath /usr/local/mongodb/log/mongodb.log --fork
./mongo
``` 
当出现success即成功 然后就可以进入mongo命令行了
5. 设置开机启动
```
vi /etc/init.d/mongod
```
在其中写入
```
#!/bin/bash

export MONGO_HOME=/usr/local/software/mongodb-linux-x86_64-4.0.6
#chkconfig:2345 20 90
#description:mongod
#processname:mongod
case $1 in
          start)
              $MONGO_HOME/bin/mongod --config $MONGO_HOME/config/mongodb.conf
              ;;
          stop)
              $MONGO_HOME/bin/mongod  --shutdown --config $MONGO_HOME/config/mongodb.conf
              ;;
          status)
              ps -ef | grep mongod
              ;;
          restart)
              $MONGO_HOME/bin/mongod  --shutdown --config $MONGO_HOME/config/mongodb.conf
              $MONGO_HOME/bin/mongod --config $MONGO_HOME/config/mongodb.conf
              ;;
          *)
              echo "require start|stop|status|restart"
```
然后
```
chmod 755 /etc/init.d/mongod
chkconfig –add mongod
chkconfig mongod on
```
Ok
注：以上命令有参考 [linux  安装mongo - 东北大亨 - 博客园](https://www.cnblogs.com/northeastTycoon/p/9298302.html)[Linux 安装MongoDB并设为开机启动 - wuhulala的休息室 - CSDN博客](https://blog.csdn.net/u013076044/article/details/80251720)
 
