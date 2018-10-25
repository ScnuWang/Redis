特点：

- 有序不重复
- 有序集合中的元素不能重复，但是score可以重复

### 1.  常用命令

```
  zadd key score member [score member ...][nx|xx|ch|incr]  # 添加成员
```
> Redis3.2为zadd命令添加了nx、xx、ch、incr四个选项：
>
> - nx：member必须不存在，才可以设置成功，用于添加。
> - xx：member必须存在，才可以设置成功，用于更新。
> - ch：返回此次操作后，有序集合元素和分数发生变化的个数
> - incr：对score做增加，相当于后面介绍的zincrby。
>
> 有序集合相比集合提供了排序字段，但是也产生了代价，zadd的时间 复杂度为O（log（n）），sadd的时间复杂度为O（1）。

- zcard key  #  计算成员个数,和集合类型的 scard命令一样，zcard的时间复杂度为O（1）。
- zscore key member  #  计算某个成员的分数

计算成员的排名:zrank是从分数从低到高返回排名，zrevrank反之

- zrank key member 
- zrevrank key member
- zrem key member [member ...]  #  删除成员
- zincrby key increment member  #  增加成员的分数

返回指定排名范围的成员

- zrange    key start end [withscores] 
- zrevrange key start end [withscores]

有序集合是按照分值排名的，zrange是从低到高返回，zrevrange反之。 

返回指定分数范围的成员

```
zrangebyscore    key min max [withscores] [limit offset count] 
zrevrangebyscore key max min [withscores] [limit offset count]
```

其中zrangebyscore按照分数从低到高返回，zrevrangebyscore反之,同时min和max还支持开区间（小括号）和闭区间（中括号），-inf和 +inf分别代表无限小和无限大

- zcount key min max # 返回指定分数范围成员个数
- zremrangebyrank key start end # 删除指定排名内的升序元素
- zremrangebyscore key min max # 删除指定分数范围的成员

集合间的操作

​	交集

```
zinterstore destination numkeys key [key ...] [weights weight [weight ...]]   [aggregate sum|min|max]
```

> destination：交集计算结果保存到这个键。
> numkeys：需要做交集计算键的个数。
> key[key...]：需要做交集计算的键。
> weights weight[weight...]：每个键的权重，在做交集计算时，每个键中 的每个member会将自己分数乘以这个权重，每个键的权重默认是1。
> aggregate sum|min|max：计算成员交集后，分值可以按照sum（和）、 min（最小值）、max（最大值）做汇总，默认值是sum。

​	并集

```
zunionstore destination numkeys key [key ...] [weights weight [weight ...]]   [aggregate sum|min|max]
```

### 2. 时间复杂度

​	![1540439340153](assets/1540439340153.png)

### 3. 内部编码

- ziplist（压缩列表）：当有序集合的元素个数小于zset-max-ziplistentries配置（默认128个），同时**每个**元素的值都小于zset-max-ziplist-value配 置（默认64字节）时，Redis会用ziplist来作为有序集合的内部实现，ziplist可以有效减少内存的使用。
- skiplist（跳跃表）：当ziplist条件不满足时，有序集合会使用skiplist作 为内部实现，因为此时ziplist的读写效率会下降。

### 4. 使用场景

- 排行榜系统