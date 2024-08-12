# 非关系型数据库

内存型：存在内存中，速度快，容量小。`Redis`,`Memcached数据库`

存储型：存在硬盘中，熟读满，容量大。`MongoDb`

应用于高并发，高速读写

## Redis

```
keys *
exist key
del key1 [key2 _]

set foo 1   //set key value,类型为string
append foo 2  //返回 ’12‘
type foo

incr num1   //自增，类型为integar
incrby num1 10  //增加10
incrbyfloat num1 0.3 //增加浮点数 0.5

decr num1  //自减1
decrby num1 10  //增加10

strlen num1  //获取长度，一个汉字为3

mset  num1 1 num2 2   /设置/获取多个值
mget  num1 num2  

gitbit num1 2 //第二位的值
```

散列结构：（类似于字典）

```
hset car:1 price 500
hset car:1 name 宝马

hget car:1 name

hgetall car:1

hincrby car:1 price 10

hexists car:1 name

hdel car:1 price   //删除字段
```

列表结构：

有序的字符串列表

```
lpush numlist 1  //左边添加
rpush numlist 2

lpop numlist
rpop numlist

llen numlist   //获取元素个数

lrange numlist 0 1   // 0-1 闭区间的片段
lrange numlist 0 -1   // 显示全部

ltrin numlist 0 1  //只保留0-1闭区间的内容

lrem numlist 1 3    //从左往右删 ，删一个，把3删除

lindex numlist 1   //获取指定index对应的内容
lset numlist 1 3   //设置index对应的内容

linsert numlist after/before 2 1  //在值为2的元素前面或后面插入1

rpoplpush numlist1 numlist2  //从一个列表弹出，另一个列表插入
```

集合结构：

```
sadd numset a  //添加元素 a

srem numset a

smembers numset    //显示所有元素

sismember numset a   //查看是否在其中

sdiff numset1 numset2   //差集

sinter numset1 numset2   //交集

sunion numset1 numset2   //并集

sdiffstore storeset numset1 numset2   //获取差集并存储

scard numset  //元素个数

srandmember  numset 1  //随机获取1个不重复元素    负数表示可以重复
```

有序集合类型：

```
zadd zset 1 tom  //tom 元素分数1，按元素排序

zscore zset tom

zrange zset 0 1  withscore //返回范围内的元素，升序

zrevrange zset 0 1   //降序排序

zrangebyscore zset 80 (100   //左开右毕区间

zrangebyscore zset 80 +inf   //正无穷

zrangebyscore zset 80 +inf  limit 0 3 //正无穷，从0开始，返回前3个

zincrby zset 4 tom    //给tom 加 4分

zcard zset  //获取元素数量

zcount zset 80 100  //返回80 -100的数量

zrem zset tom   //删除元素

zremrangebyrank zset 0 2  //按照排名范围删除
zremrangebyscore zset 80 200  //按照分数范围删除

zrank zset tom   //获取排名
zrevrank

zinterstore setstore zset1 zset2 [weights 1 10 ] [aggregate min/max/sum]  //区交界，权重，合并规则，默认等权求和
```

