#### Druid的整体架构

##### 1. 什么是LSM tree

LSM tree 即 **Log structured merge tree**，特点是同时使用了两部分类树的数据结构来存储数据，C0和C1。

![image-20190122174354445](/Users/wendyzhi/Library/Application Support/typora-user-images/image-20190122174354445.png)



##### 2. Review 几个节点

* Real-time node
* Coordinator node
* historical node
* broker node

![druid-arch](/Users/wendyzhi/Desktop/druid-arch.png)



##### 3. 读写分离

其实就是将数据库分为了主从库，一个主库用于写数据，多个从库完成读数据的操作，主从库之间通过某种机制进行数据的同步，是一种常见的数据库架构。

![image-20190122175548821](/Users/wendyzhi/Library/Application Support/typora-user-images/image-20190122175548821.png)

