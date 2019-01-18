#### Druid broker node

##### Main function

* provide query service 

##### Work flow

```sequence
clients->broker node:query request
broker node->realtime node:query request
broker node->historical node:query request
realtime node->broker node:resp1
historical node->broker node:resp2
Note left of broker node:merge resp1&2
broker node->clients:resp
```



##### Notes

* Usually one druid cluster has 2 broker nodes, one for daily query, and the other for backup. However, when one cluster has many broker nodes, we have to consider load balancing between each node. In that case, we could use nginx as an API gateway to make sure cluster HA.