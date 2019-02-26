#### Druid indexing service

#####Main function

* create segment file --> push
* merge segment file
* delete segment file

#####Arch

* overlord: 统治节点，对外接收请求，对内进行任务分解和下发

  * client向overlord提交任务

    http://<overlord ip>:<port>/druid/indexer/v1/task

  * client向overlord杀掉某个任务

    http://<overlord ip>:<port>/druid/indexer/v1/task/{taskId}/shutdown

  * client向overlord请求查看某个任务的状态

    http://<overlord ip>:<port>/console.html

* middle manager: 中间管理者，负责接收统治节点分配的任务，启动相关苦工来完成具体的任务

* peon: 苦工，干活的



Local mode

```sequence
client->overlord:task request
overlord->peon:doing things
```



Remote mode

```sequence
client->overlord:task request \nfrom restful api
overlord->middle manager:task request
middle manager->peon1:doing things
middle manager->peon2:doing things
middle manager->peon3:doing things
```



