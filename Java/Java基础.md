# Java基础知识和语法
## Java有哪几种基本类型以(对应的包装类型)，分别占用字节是多少？
byte(Byte) 1字节
boolean(Boolean) 1字节
char(Character) 2字节
short(Short) 2字节
int (Integer) 4字节
float(Float) 4字节
double(Double) 8字节
long(Long) 8字节

## 关于String类

# Immutable类
> String和Integer等包装类都属于Immutable类，能够保证多线程环境中对象状态不改变，而且不使用锁机制就能被其他线程共享。

## Immutble类设计原则
1. Immutable对象的状态在创建之后就不能发生改变，任何对它的改变都应该产生一个新的对象。
2. Immutable类的所有的属性都应该是final的。
3. 对象必须被正确的创建，比如：对象引用在对象创建过程中不能泄露(leak)。
4. 对象应该是final的，以此来限制子类继承父类，以避免子类改变了父类的immutable特性。
5. 如果类中包含mutable类对象，那么返回给客户端的时候，返回该对象的一个拷贝，而不是该对象本身（该条可以归为第一条中的一个特例）。


# Java集合
## 集合的继承关系

> Collection(Iterable)  Map

## HashMap
### HashMap的底层实现原理
> HashMap是一个由数组和链表组成的存储结构，存储内容是键值对。主要方法是put()和get()。put()方法通过计算key的hashCode找到对应table数组的下标地址，如果地址上的数据为空则直接存放，如果不为空，那么就以链表的方式插入这个数据，以此处理hash冲突。get()方法也是通过计算key的hashCode找到对应table数据的下标地址，在以这个地址为头节点的链表中查找匹配key的键值对。table的初始长度是16，扩容阈值是16*0.75=12，当table长度到达阈值，内部会调用resize()方法，扩容到原容量的两倍，同时迁移oldtab上的数据，rehash到newtab。

#### JDK8中的优化
- 当链表长度到达8时，会把链表转成红黑树，查找时间复杂度从O(N)优化到O(logN)。当链表长度缩小到6时，会从红黑树退化成链表。
- 

### HashMap中table的长度为什么是16，或者2的幂次方？
> index = hashCode(key) & (tab.length - 1);  
计算下标的hash算法通过 &(长度-1)的方式完成取模运算，2的幂次方-1能够保证二进制位全部是1，比如16-1=15，15的二进制是1111，可以说key值的下标位置其实是完全取决于后几位的，用2的幂次方取模比%运算符要高。

### HashMap线程不安全的情形？
> 多线程情况下进行rehash。

### Redis中字典的设计如何处理扩容和收缩？

### 哈希算法
#### MurmurHash

## ConcurrentHashMap
### 


# JUC包
## CountDownLatch原理
> 实例化CountDownLatch对象时需要传入count值，表示要等到count个线程完成后才能继续往下执行。CountDownLatch内部有一个sync静态内部类，继承自AQS，CountDwonLatch的构造函数会把count传给sync的state，任何一个线程调用这个对象的countDown()方法，都会把state-1，此时线程继续执行的条件是获取AQS的共享锁，而获取条件是state==0，当count个线程都执行了countDown()方法后，等待线程就能获取共享锁，继续往下执行了。
