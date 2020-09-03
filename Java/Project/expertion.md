### 关于使用zookeeper和dubbo

1. zookeeper连不上:

   - zookeeper contected is not yet
   - zookeeper 根本没启动： ./zkServer.sh status 
   - IP和端口不对
   - 防火墙：阿里云需要额外开2181端口

2. dubo服务不可用

   1. 登上你的zk，然后执行ls / 看是否有dubbo目录

   2. 停止你所有的java服务

   3. 删除zk里所有的dubbo目录

      rmr / dubbo

      ls /

   4. 只启动你的内容提供者

      ls /dubbo




## 知识点

1. [jdbc批量添加（入库)](https://blog.csdn.net/whucyl/article/details/20838079)