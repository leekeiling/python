## Python操作Redis

### 建立连接

```python
import redis
r = redis.Redis(host='127.0.0.1', port=6379)
```

### 操作string类型

```python
set(name, value, ex=None, px=None, nx=False, xx=False)
```

- name: 键
- value：值
- ex: 过期时间，单位秒
- px：过期时间，单位毫秒
- nx：设置为True，则只有name不存在时，当前set操作才执行
- xx：如果设置为True，则只有name存在时，当前set操作才执行

- setnx(name, value)

  说明：只有当name不存在时，才能进行设置操作

- setex(name,value, time)

  说明：用于设置键值对和键的过期时间

- mset(*args, **kwargsd)

  说明：用于批量设置键值对

- mget(keys, *args)

  用于批量获取键值

- getset(name, value)

  用于设置新值并获取原来的值

- getrange(key, start, end)

  根据字节获取子字符串

- setrange(name, offset, value)

  从指定字符串索引开始向后修改字符串的内容

- setbit(name, offset, value)

  对name对应值的二进制形式进行位操作

- getbit(name, offset)

  获取那么对应值的二进制形式中某一位的值

- bitcount(key, start=None, end=None)

  获取name对应值的二进制形式中1的个数

- strlen(name)

  返回name对应值的长度

- append(key, value)

  在那么对应值之后追加内容

### 操作hash类型

- hset(name, key, value)

  设置name对应hash中一个键值对，如果不存在则创建；否则，进行修改

- hmset(name, mapping)

  在name对应的hash中批量设置键值对

  r.hmset('student', {‘name': 'lee', 'age': 20})

- hge(name, key)

  获取name对应的hash中key的值

- hmget(name, keys, *args)

  批量获取name对应的hash对应的key的值

### 操作list类型

- lpush(name, values)

  在name对应的list中添加元素，每个新的元素都添加到列表的最左边

- linsert(name, where, refvalue, value)

  name对应的列表的某一个值前或后插入一个新值

  where: before 或者 after

  refvalue: 在它前后插入数据

  value： 插入的数据

- lset(name, index, value)

  对name对应list中的某一个索引位置赋值

- lrem(name, value, num)

  在name对应的list中删除指定的值

  num: 第num次出现。当怒骂=0，删除列表中所有指定值

- lpop(name)

### 操作set类型

- sadd(name, values)

  为name集合添加元素

- scard(name)

  获取name对应集合中元素的个数

- smembers(name)

  获取name对应集合中所有成员

- sdiff(keys, *args)

  获取多个name对应集合的差集

- sinter(keys, *args)

  获取多个name对应集合的交集

- sunion(keys, *args)

  获取多个name对应集合的并集

### 操作sorted set类型

- zadd(name, *args, **kwargs)

  在name对应的有序集合中添加元素和元素对应的分数

  r.zadd("z_num", num1=11, num2=22)

- zcard(name)

  获取元素个数

- zrange(name, start, end, desc=False, withscores=False, score_cast_func=float)

  按照索引返回获取name对应的有序集合的元素

  withscores：是否获取元素的分数，默认之获取元素的值

  score_cast_func: 对分数进行数据转换的函数。

- zrem(name, values)

  删除name对应有序集合中值是values的成员