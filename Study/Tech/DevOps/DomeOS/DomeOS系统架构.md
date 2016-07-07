## 各组件说明

### 控制台组件
- DomeOS Server：
一套自主开发的用来管理DomeOS系统资源的**WEB管理系统**。主要提供了DomeOS的Web交互方式和可供访问的API接口。
- Mysql：
用来存储DomeOS中的项目、部署、集群、用户等所有信息及其相关的配置。DB中相关的数据库表结构是DomeOS预定义的，配置并启动MySQL后，执行对应的初始脚本，就会创建相应的数据库和表结构。

### Docker组件
- docker： 容器引擎
- flannel：[flannel](https://github.com/coreos/flannel) is a virtual network that gives a subnet to each host for use with container runtimes. Flannel是一个覆盖网络工具，其设计目的就是为集群中的所有节点重新规划IP地址的使用规则，使得不同节点上的容器能够获得**同属一个内网**且**不重复的**的IP地址，并让属于不同节点上的容器能够直接通过内网IP通信。
- etcd：flannel是基于[etcd](https://github.com/coreos/etcd)的。etcd是一个高可用的键值存储系统，主要用于共享配置和服务发现。
- registry：私有仓库，用于存储和分发用户的所有docker镜像
 
### Kubernetes组件
[kubernetes](http://kubernetes.io/) is an open-source system for automating deployment, scaling, and management of containerized applications.