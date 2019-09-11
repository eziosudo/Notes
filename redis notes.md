# Redis布隆过滤器
## 用途
- 去重。用来判断一个元素是否在一个集合中。
## 过程
1. 用一个位数组arr去记录数据存在/不存在。一个长度为10000的位数组只占用10000/8=1250B空间。
2. 此时请求发送一个字符串A，用f1,f2,f3这三个哈希函数对A计算，得到n1,n2,n3。
3. 在位数组中把arr[n1],arr[n2],arr[n3]置为1。
4. 请求再发送字符串A，用f1,f2,f3计算出n1,n2,n3，发现arr中三个值都是1，那么说明A存在。

## 注意点
- 随着数据量增大，可能有多个数据计算出来的结果是一样的，就可能导致布隆过滤器误判。
- 布隆过滤器可能误判“存在”,但是如果过滤器中不存在，那就一定是不存在。

## 实际应用
- 

# Redis集群
- Redis主从架构 -> 读写分离 -> 支持10w+读QPS
  
## Redis replica基本原理
- master节点采用异步方式复制数据到slave。slave节点会周期性地向master确认自己的复制量。
- 一个master节点可以配置多个slave节点，一个slave节点也可以和其他slave节点通信。
- slave节点复制的时候不阻塞master节点的工作
- slave节点复制的时候不影响自己的查询，它会用旧的数据提供查询服务，等所有数据复制完成后，删除旧数据，复制数据集，此时会暂停对外服务（毫秒到秒）。
- slave节点主要用来做水平扩容，做读写分离。
- master fail后，slave节点可以自动接管master。

### sentinel?? 如果sentinel没有及时检测到master fail，那么master重启后，还是可能导致slave数据被master同步清空。

### 主从架构，master节点必须持久化
- 如果master节点被关掉而没有持久化，那么在master节点挂掉重启后，数据是空的，然后经过复制，所有slave节点上的数据也空了。

# Redis过期策略？
## 定期删除和动态删除。

# Redis怎么保证高并发和高可用？

# Redis主从原理？

# Redis哨兵机制？

# Redis持久化？

# Redis数据类型有哪些?
## String 字符串(sds.h)

- 可以直接获取字符串长度O(1)。
- 相比C字符串，SDS实现了空间预分配和空间惰性释放。SDS会给字符串预留一定的分配空间，避免每次字符串增长时都去请求新的内存空间。SDS会记录总空间长度，已使用字符串长度和未使用字符串长度，避免字符串在增长或者缩短时频繁计算。

- 二进制安全。有了字符串长度作为有效数据的指标，避免有些特殊数据格式（比如图片，音频等二进制数据）会因为'\0'字符导致后面的字符串无效。即可以保存文本也可以保存二进制数据。

## List 链表
### 用法
- lrange 可以做查询的分页，比如评论，粉丝列表。

## Hash 字典(dict.h)


## Set 集合
### 无序集合，自动去重。对数据做交集，并集，差集。

## ZSet 跳表
### 可以按照分数字段给数据排序。比如排行榜。




