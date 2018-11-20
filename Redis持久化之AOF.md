### 定义
以独立日志的方式记录每次写命令，重启时再重新执行AOF文件中的命令达到恢复数据的目的。AOF的主要作用是解决了数据持久化的实时性，目前已经是Redis持久化的主流方式。

### 使用AOF

开启AOF功能需要设置配置：appendonly yes，**默认不开启**。AOF文件名 通过appendfilename配置设置，默认文件名是appendonly.aof。保存路径同 RDB持久化方式一致，通过dir配置指定。

### 持久化流程

1. 命令写入 （append）

    所有的写入命令会追加到aof_buf（缓冲区）中，AOF命令写入的内容直接是文本协议格式

2. 文件同步（sync）

    AOF缓冲区根据对应的同步策略向硬盘做同步操作。

    appendfsync always

    appendfsync everysec （默认）

    appendfsync no

3. 文件重写（rewrite）

    随着AOF文件越来越大，需要定期对AOF文件进行重写，达到压缩的目的。

4. 重启加载 （load）

    当Redis服务器重启时，可以加载AOF文件进行数据恢复。

当AOF和RDB都存在的时候，启动时先加载AOF文件

可使用redis-check-aof检查AOF文件，如果有异常数据，自动修复

check-aof --fix appendonly.aof

### 优势

数据完整性相对RDB更高

同步策略：
​    appendfsync everysec：每秒同步 ：会丢失一秒内的数据
​    appendfsync always：每修改同步：性能差
​    appendfsync no：不同步
​    

### 缺点

相同数据集的数据，aof文件远大于rdb文件，恢复速度慢于rdb

aof的运行效率慢于rdb,每秒同步效率较好，appendfsync no时效率与rdb相同


### 重写原理

### 触发机制
