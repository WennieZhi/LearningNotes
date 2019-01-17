#### Druid real-time node

##### Main Function

- Injest streaming data 
  - Module: Fire Hose 
    - Consume real-time data from MQ, such as kafka. (druid-kafka-eight firehose)
- Generate segment file
  - Module: Plumber
    - Generate segment file

##### Work flow

```sequence
realtime node->deep storage: segment file
realtime node->MySQL(metadata store): segment metadata
MySQL(metadata store)->coordinator node: we have this segment(metadata)
coordinator node->history node: we have this segment(metadata)
history node->deep storage: I want this segment
deep storage->history node: segment file
history node->zookeeper: I'm responsible for this segment
Note right of zookeeper: Announcement: this\n history node is responsible \nfor this segment 
Note left of realtime node: Announcement accepted, stop \n query serive for this segment
```

##### Notes

A batch of real-time nodes consist of a consumer group, each node will actively report their consume progress to zookeeper. In that case, when one node is down, zookeeper will know this is happening and realloc partitions to other nodes.

##### But theres still a flaw

When a node is down with partial segment file, we can make sure the unconsumed part is OK since we record its offset, but what about the consumed part? 

There are 2 solutions:

1. Try to make this down node get back to live ASAP.
2. Use Tranquility and indexing service to backup all kafka parttitions. and if node A, who is responsible for topic A - partition A - replica A, is down, we can find a node who is responsible for topic A - partition A - replica B.





###### 

