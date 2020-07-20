> Lock 

三部曲：

1. new ReentrantLock();
2. lock.lock)(); // 加锁
3. finally = > lock.unlock(); // 解锁



> synchronized 和 Lock 的区别

1. synchronized 是内置的javba关键字，Lock是一个java类

2. synchronized 无法判断获取锁的状态，Lock可以判断是否获取了锁
3. synchronized 会自动释放锁，lock必须要手动释放锁！ 如果不释放锁，**死锁**
4. synchronized 线程1（获得锁，阻塞）、线程2（等待，傻傻的等）；Lock锁久不一定会等待下去
5. synchronized 可重入锁，不可以中断的，非公平；Lock，可重入锁，可以判断锁，非公平（可以自己设置）
6. synchronized 适合少量的代码同步问题，lock适合大量的同步代码！



### 生产者和消费者问题

面试的： 单例模式，排序算法，生产者和消费者，死锁



>生产者消费者问题 synchronized版

![image-20200719170023124](C:\Users\ZeTao\AppData\Roaming\Typora\typora-user-images\image-20200719170023124.png)

此时可能产生虚假唤醒问题



**要用whiel替换if**  （if只判断一次） 

![image-20200719170158099](C:\Users\ZeTao\AppData\Roaming\Typora\typora-user-images\image-20200719170158099.png)



> JUC版本的生产者消费者问题

![image-20200719170231188](C:\Users\ZeTao\AppData\Roaming\Typora\typora-user-images\image-20200719170231188.png)



> JUC版本生产者消费者问题

![image-20200719170449204](C:\Users\ZeTao\AppData\Roaming\Typora\typora-user-images\image-20200719170449204.png)







> 并发下ArrayList安全吗?

**不安全,** 会有ConcurrentModificationException 异常

解决方法:

1. List<String> list = new Vector<>();
2. List<String> list = Collections.synchronizedList(new ArrayList<>());
3. List<String> list = new CopyOnWriteArrayList<>()；

// CopyOnWrite 写入时复制 COW 计算机程序设计领域的一种优化策略；
// 多个线程调用的时候，list，读取的时候，固定的，写入（覆盖）
// 在写入的时候避免覆盖，造成数据问题！
// 读写分离
**CopyOnWriteArrayList 比 Vector Nb 在哪里？**

Vector 用的是Synchronized,  而 CopyOnWriteArrayList用的是 Lock,效率更好



> 并发下Set安全吗?

**不安全**, 会出现 ConcurrentModificationException 异常

解决办法:

1. ​	Set<String> set = Collections.synchronizedSet(new HashSet<>());



>  **hashSet 底层是什么？**

```java
public HashSet() {
map = new HashMap<>();
}
// add set 本质就是 map key是无法重复的！
public boolean add(E e) {
return map.put(e, PRESENT)==null;
}
private static final Object PRESENT = new Object(); // 不变得值！
```





> Map 不安全



解决方法:

1. Map<String, String> map = Collections.synchronizedMap(new HashMap());
2. Map<String, String> map = new ConcurrentHashMap<>();



> 常用辅助类

- CountDownLatch  -->减法

  原理：
  countDownLatch.countDown(); // 数量-1

  countDownLatch.await(); // 等待计数器归零，然后再向下执行

  每次有线程调用 countDown() 数量-1，假设计数器变为0，countDownLatch.await() 就会被唤醒，继续执行！

- CyclicBarrier  --> 加法

  达到一定的数量，再触发await

- Semaphore  --> 信号量

  原理：
  semaphore.acquire() 获得，假设如果已经满了，等待，等待被释放为止！

  semaphore.release(); 释放，会将当前的信号量释放 + 1，然后唤醒等待的线程！
  作用： 多个共享资源互斥的使用！并发限流，控制最大的线程数！

