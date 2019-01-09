## kafka-manager 安装

### 1. 安装步骤

1. sudo dpkg -i kafka-manager_1.3.3.15_all.deb 
2. sudo vim /usr/share/kafka-manager/conf/application.conf 
3. kafka-manager.zkhosts="127.0.0.1:2181” 
4. cd /usr/share/ , sudo chown -R chown shopee:shopee kafka-manager
5. su shopee , nohup /usr/share/kafka-manager/bin/kafka-manager &

### 2. 遇到的坑

1. openFile(application/logs/application.log,true) call failed

   详细报错如下：
   java.io.FileNotFoundException: application.home_IS_UNDEFINED/logs/application.log (No such file or directory)

   查看/usr/share/kafka-manager/logs，确实没有application.log这个文件，自己创建一个application.log文件，问题解决。

2. 9000端口被占用

   Caused by: java.net.BindException: Address already in use

   启动时加参数“-Dhttp.port=9001”即可

### 3. 安装后配置cluster

![image-20190109151822173](/Users/wendyzhi/Library/Application Support/typora-user-images/image-20190109151822173.png)

![image-20190109151851289](/Users/wendyzhi/Library/Application Support/typora-user-images/image-20190109151851289.png)