[官方文档](https://redis.io/download)

### 下载提取并且编译

```bash
$ wget http://download.redis.io/releases/redis-5.0.0.tar.gz
$ tar xzf redis-5.0.0.tar.gz
$ cd redis-5.0.0
$ make
$ make install # 默认将redis安装到/usr/local/bin，并且可以在任意命令窗口使用redis相关命令
```

### 启动服务端

```bash
redis-server redis.conf
```

### 客户端连接

```
$ redis-cli -h host -p port -a password # 参数是否添加都是可选的
```

### 关闭Redis

1. 直接暴力杀进程
2. 执行命令：`shutdown`

### 注意事项

1. 权限问题

    如果是自己本地虚拟机Ubuntu最好默认使用root登录，避免权限问题。[Ubuntu18.04默认使用root登录](https://blog.csdn.net/weixin_41923456/article/details/81001179)

2. rdb文件，aof文件，日志文件存放位置

    默认是当前用户的主目录下比如，登录用户是zhangsan,那么默认的上述文件**可能**在/home/zhangsan/下

3. 将redis.conf配置文件复制到工作目录如：/opt/redis

    这样做的目的是为了给默认的配置文件做备份，这是个习惯问题，以防万一。

### 示例

```
root@jason:/usr/local/bin# mkdir /opt/redis
root@jason:/usr/local/bin# cp redis.conf /opt/redis
root@jason:/usr/local/bin# cd /opt/redis
root@jason:/usr/local/bin# redis-server /opt/redis/redis.conf 
root@jason:/usr/local/bin# redis-cli
127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> get k1
"v1"
127.0.0.1:6379> SHUTDOWN
not connected> exit
root@jason:/usr/local/bin# 
```