## 1. Compare and Swap
https://www.baeldung.com/cs/aba-concurrency

CAS is a common technique in lock-free algorithms to ensure that **updates to shared memory by one thread fail if another thread has modified the same space in the meantime**.

We achieve this by using two pieces of information in each update: The updated value as well as the original value. The CAS will first compare the existing value to the original. If equal, it then swaps the existing value with the updated one. 

### ABA Problem
One acitivity reads some shared memory, in preparation for updating it. Then, another acitivity temporarily modifies that shared memory and then **restores** it. Following that, once the first acitivity preforms CAS, it will appear as if no change has been made, invalidating the integrity of the check. 

### Double CAS
The idea behind the double CAS method is to **keep track of one more variable, which is the version number**, then use that in the comparsion as well. In this case, the CAS operation will fail if we have the old version number, which is the only possible when another thread modified our variable in the meantime. 

In Java, [AtomicStampedReference](https://www.baeldung.com/java-atomicstampedreference) and [AtomicMarkableReference](https://www.baeldung.com/java-atomicmarkablereference) are the standared implementations of this method. 

## ZooKeeper 分布式锁
ZooKeeper 的数据结构是树。每个节点叫做Znode。每个节点可以通过路径来标识。Zookeeper 通过创建临时节点，因为节点的唯一性，一旦创建成功， 就说明获得锁。一旦获取到锁的客户端挂点， 那么这个临时节点就会自动删除， 其他客户端自动获取锁。
Zookeeper监听机制是通过监听小节点， 从而避免羊群效应（所有阻塞的客户端同时竞争同一个资源）。
