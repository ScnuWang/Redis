[在线体验地址](http://try.redis.io/)
## 一、基础

1. SET-if-not-exists（在Redis上称为SETNX）仅在key尚不存在时设置key，INCR以原子方式递增存储在给定key的数字
2. Redis的INCR是原子操作
3. 设置有效时间
    EXPAIR   key  time
    TTL key # 检查剩余有效时间，返回值为-2，表示key不存在；返回值为-1，表示永不过时
## 二、常用数据类型
### 1. List
特点：有序可重复
相关命令：RPUSH,LPUSH,LLEN,LRANGE,LPOP,RPOP
LRANG list_key start end # end=-1表示取到List最后一个元素
LLEN list_key：返回当前list的长度
LPOP list_key:  删除第一元素并且返回
RPOP list_key：删除最后一个元素并且返回
### 2. Set
特点：元素无序且不重复
相关命令：SADD,SREM,SISMEMBER,SMEMBERS,SUNION
SADD key value # 添加
SREM key value # 删除
SISMEMBER key value # 测试值是否存在set里面，返回1表示存在，返回0表示不存在
SMEMBERS key # 将set集合用list的形式返回回来
SUNION key1 key2 # 将两个set组合在一起，并以list的形式返回
### 3. Sorted Set
redis 1.2 引入
特点：元素有序且不重复
有序集类似于常规集，但现在每个值都有一个关联的分数。此分数用于对集合中的元素进行排序。
相关命令ZADD，ZRANGE
ZADD key score value # 默认根据score进行升序排列
ZRANGE key start end # 以list的方式返回
### 4. Hashes
哈希是字符串字段和字符串值之间的映射,所以它们是表示对象的完美数据类型
特点：
相关命令：HSET,HGETALL,HGET，HINCRBY,HDEL
HSET key field1 value1 field2 value2 # 添加数据
HGETALL key # 返回所有存入的数据，包括field
HGET key field1 # 获取value1，如果添加时的数据是数字型，仍然返回字符串类型
HINCRBY key field number # 原子性操作，number 表示要加的数字大小
