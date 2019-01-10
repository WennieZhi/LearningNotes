# ES 常用命令

### 目录

- [Cluster Health](#cluster-health)
  - [Index Level](#index-level)
  - [Shard Level](#shard-level)
- [Nodes Overview](#nodes-overview)
  - [Who is the master](#who-is-the-master)
- [Indices Overview](#indices-overview)
  - [View One Index](#view-one-index)
  - [View Multi-Indices](#view-multi-indices)
- [Cluster Maintance](#cluster-maintance)
- [Setting](#setting)
- [Query](#query)



### Cluster Health

```json
curl -XGET http://localhost:9200/_cluster/health?pretty

{
  "cluster_name" : "test_cluster",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 3,
  "number_of_data_nodes" : 3,
  "active_primary_shards" : 28,
  "active_shards" : 74,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}
```



#### Cluster Health: Index Level

```
curl -XGET http://localhost:9200/_cluster/health?level=indices
```



#### Cluster Health: Shard Level

```
curl -XGET http://localhost:9200/_cluster/health?level=shards
```



### Nodes Overview

```
curl -XGET http://localhost:9200/_cat/nodes?v
```



#### Who is the master?

```
curl -XGET http://localhost:9200/_cat/master?v
```



### Index Overview

```
curl -XGET http://localhost:9200/_cat/indices?v
```



#### View One Index

```
curl http://localhost:9200/_cat/indices/shopee_id_v2?v
```



#### View Muti-Indices

```
curl http://localhost:9200/_cat/indices/shopee_id_v*?v
```



### Cluster Maintance

...

### Setting

...

### Query

...