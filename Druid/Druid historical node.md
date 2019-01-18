#### Druid historical node

##### Main function

* Load segment file for queries

##### Work flow

when a new historical node added to the druid cluster...

```sequence

new historical node->zookeeper: Hey, I'm a new historical node
zookeeper->coordinator node: This node is a new histoical node
coordinator node->new historical node:This is the segment that you should be responsible
new historical node->deep storage:pull this segment
deep storage->new historical node:done
new historical node->zookeeper:I'm responsible for this segment

```

When a historical node is removed from the druid cluster

```sequence
historical node A->zookeeper:I'm gonna leave
zookeeper->coordinator node:this node is gonna leave
coordinator->historical node B: alloc A's segment to you
historical node B->zookeeper:I'm responsible for A's segment
```

##### Notes

* **data temperature** reflect how frequently it is accessed.
  * hot data
  * warm data
  * cold data

