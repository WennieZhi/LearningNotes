#### Druid coordinator node

##### Main function

* coordinator node is only the master of **historical node**
  * responsible for the load balance between historical nodes



##### Notes

* 副本实现Segment的高可用性

  想象一下如果一个segment只有一份，如果为他负责的历史节点down掉了，协调节点会重新安排另一个历史节点去load这个segment。但是在重新assign给另外一个历史节点之前这段时间就是一个黑暗时期。

  如果每个segment都有副本，且副本存在在不同的历史节点上，就完美解决可这个单点失效的问题。



