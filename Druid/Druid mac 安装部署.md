### Druid mac 安装部署



##### 1. 先决条件

查看java版本，需要>= java 7

```
java -version 
```



##### 2. 安装Druid

```bash
curl -O http://static.druid.io/artifacts/releases/druid-0.9.2-bin.tar.gz 
tar -xzf druid-0.9.2-bin.tar.gz 
cd druid-0.9.2 
```

你可以看到以下目录: 

LICENSE - 许可文件。 
bin/ - 用于快速启动的脚本。 
conf/* - 以集群方式启动的配置模板。 
conf-quickstart/* - 快速启动的配置文件。 
extensions/* - 所有Druid的扩展概念。 
hadoop-dependencies/* - Druid的hadoop依赖 
lib/* - Druid核心功能所需的包。 
quickstart/* - 用于快速启动的文件。

 


##### 3、安装zookeeper 

```bash
curl http://www.gtlib.gatech.edu/pub/apache/zookeeper/zookeeper-3.4.6/zookeeper-3.4.6.tar.gz -o zookeeper-3.4.6.tar.gz 
tar -xzf zookeeper-3.4.6.tar.gz 
cd zookeeper-3.4.6 
cp conf/zoo_sample.cfg conf/zoo.cfg 
```

```bash
vi zoo.cfg 
```

添加一行配置项 

```
server.1=localhost:2888:3888 
```

重新启动zookeeper: 

```bash
sh zkServer.sh start 
```

再查看运行状态是成功了 



##### 4、启动Druid程序 

进入DRUID_HOME目录下，按顺序执行下面命令启动相应程序 

```bash
nohup java `cat conf-quickstart/druid/historical/jvm.config | xargs` -cp "conf-quickstart/druid/_common:conf-quickstart/druid/historical:lib/*" io.druid.cli.Main server historical &

nohup java `cat conf-quickstart/druid/broker/jvm.config | xargs` -cp "conf-quickstart/druid/_common:conf-quickstart/druid/broker:lib/*" io.druid.cli.Main server broker &

nohup java `cat conf-quickstart/druid/coordinator/jvm.config | xargs` -cp "conf-quickstart/druid/_common:conf-quickstart/druid/coordinator:lib/*" io.druid.cli.Main server coordinator &

nohup java `cat conf-quickstart/druid/overlord/jvm.config | xargs` -cp "conf-quickstart/druid/_common:conf-quickstart/druid/overlord:lib/*" io.druid.cli.Main server overlord &

nohup java `cat conf-quickstart/druid/middleManager/jvm.config | xargs` -cp "conf-quickstart/druid/_common:conf-quickstart/druid/middleManager:lib/*" io.druid.cli.Main server middleManager &
```

查看是否有异常日志

```bash
vim nohup.out
```



##### 5、验证程序部署是否成功 

以上五个druid程序都正常启动后: 

可以通过web查看数据批量导入Druid的任务执情况：http://localhost:8090/console.html 

访问http://localhost:8081/ 查看任完成进度、数据分片情况、索引创建等 
