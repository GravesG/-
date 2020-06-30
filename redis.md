> redis 数据类型

- String
- Hash
- List
- Set
- ZSet



> redis的高级功能

消息队列、过期自动删除、数据持久化、分布式锁



> redis 几个 比较主要的可执行文件

- redis-server    启动redis
- redis-cli        redis客户端
- redis-benchmark    redis基准调试工具
- redis-check-aof     redis  AOF 持久化文件检测和恢复工具
- redis-check-dump   redis RDB 持久化文件检查和修复工具
- redis-sentinel      启动redis sentinel



> 如何停止redis服务

``` shell
redis-cli shutdown nosave | save
```



> redis 为什么快？单线程？

-  redis 使用了单线程架构和I/O多路复用模型
- 纯内存访问
- 单线程避免了线程切换带来的开销



> RDB

RDB 持久化是把当前进程数据生成快照保存到硬盘的过程。（以二进制方式写入磁盘）



手动触发：

- save ： 阻塞当前redis服务器，知道RDB过程完成为止，如果数据比较大的话，会造成长时间的阻塞，线上不建议使用
- bgsave： 进程执行fork操作创建子进程，持久化由子进程负责，完成后自动结束，阻塞只发生在fork阶段。一般时间很短
- save xsencends n： 表示在X秒内，至少由n个键发生变化，才会触发RDB。也就是说满足条件才会就会触发持久化
- flushall：主动同步触发



优点：

- RDB 是一个紧凑的二进制文件，代表redis在某个时间上的数据快照
- 适用于备份，全量复制的场景，对于灾难回复非常有用
- Redis 加载RDB回复数据的速度快于AOF

缺点：

- rdb 无法做到实时持久化，可能会丢失数据
- RDB需要fork创建子进程，属于重量级操作，可能导致redis卡顿若干秒



> AOF 

AOF 是为了解决不能实时持久化的问题，以独立的日志方式记录把每次命令记录到aof文件中

如何查询是否开启：

````shell
config get appendonly
````

如何开启：

- 实时生效，但重启后失效

````shell
config set appendonly
````

- 需要重启生效，重启后依然生效

````shell
appendonly yes
````



AOF工作流程

1. 所有写入命令追加到aof_buf缓存区。
2. AOF缓存区根据对应的策略向磁盘同步
3. 随着AOF文件越来越大，需要定期对AOF文件进行重写，达到压缩的目的
4. 当redis服务器重启时，可以加载AOF文件进行数据回复



> 为什么AOF要先把命令追加到缓存区中？

Redis使用单线程响应命令。如果每次写入文件命令都直接添加到磁盘，性能就会取决于磁盘的负载。如果使用缓存区，redis提供多种缓存区策略，在性能和安全性方面做出平衡。



> AOF 持久化时如何触发的？

- 自动触发： 满足设置策略和满足重写触发

  https://gitee.com/yizhibuerdai/Imagetools/raw/master/images/image-20200427101221526.png

- 手动触发：bg rewrite aof



> AOF 优点

- 提供了三种保存策略：每秒保存/跟系统策略/每次操作保存。实时性比较高，一般选择每秒保存，因此意外发生时最多失去一秒的数据。
- 文件以追加的形式写入，所以文件很少有损坏问题，如果发生意外少写了数据,可以通过aof-check-aof工具恢复
- AOF由于时纯文本形式，直接采用协议格式，避免二次处理开销，对于 修改也比较灵活。



> AOF 缺点

- AOF 文件比RDB文件大
- 由于执行频率比较高，所以负载高时，性能没有RDB好



> 混合持久化

- 优点： 加速加载的同时，避免了丢失过多的数据
- 缺点：1. 可读性差   2.兼容性（需要4.0以后才支持）



> Redis中key的过期操作

设置key的生存时间为n秒

```
expire key nseconds
```

设置key的生存时间为nmilliseconds秒

```
pxpire key milliseconds
```

设置key的生存时间为timestamp所指定的秒数时间戳

```
expireat key timespamp
```

设置key的生存时间为timestamp毫秒时间戳

```
pexpireat key millisecondsTimestamp
```



> 获取当前最大内存

获取最大内存

```
config get maxmemory
```

设置最大内存

```
config set maxmemory 1GB
```



> 内存溢出策略

1. noveiction(默认策略)：拒绝写入操作并返回客户端错误信息OOM
2. volatile-lru：根据LRU算法删除设置了超市属性的键
3. allkeys-lru：根据LRU算法删除所有键
4. allkeys-random：随机删除所有键
5. volatile-random：随机删除过期键
6. volatile-tth：根据键值对象的ttl属性，删除最近要过期的数据





