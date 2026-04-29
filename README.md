# 基于OpenEuler系统通过docker部署Hadoop集群
## 项目介绍
1. 集群架构
项目部署了一个完整的 Hadoop 集群，包含以下节点：

- NameNode (172.20.0.10) - HDFS 名称节点，端口 9870(Web UI)、9000(HDFS)
- SecondaryNameNode (172.20.0.12) - 辅助名称节点，端口 9868
- ResourceManager (172.20.0.11) - YARN 资源管理器，端口 8088
- DataNode1 (172.20.0.21) - 数据节点 1
- DataNode2 (172.20.0.22) - 数据节点 2
- NodeManager1 & 2 - 节点管理器（在 workers 中定义） 2. 技术栈
- 操作系统 : OpenEuler 24.03
- JDK : OpenJDK 11.0.2
- Hadoop : 3.4.2
- 容器化 : Docker + Docker Compose 3. 关键配置特点
HDFS 配置 ( hdfs-site.xml ):

- 副本数设置为 2（适合小型集群）
- 禁用权限检查（ dfs.permissions.enabled=false ）
- 关闭 IP 主机名检查以适应容器环境
YARN 配置 ( yarn-site.xml ):

- 内存配置：2048MB
- 关闭虚拟内存检查（容器环境常用优化）
- 启用日志聚合功能
网络配置 :

- 独立的 Docker 网络 hadoop-net
- 子网：172.20.0.0/16
- 静态 IP 分配确保节点间稳定通信 4. 数据持久化
通过卷挂载实现数据持久化：

- NameNode 元数据： ./data/namenode/name
- DataNode 数据： ./data/datanode1/data 、 ./data/datanode2/data
- 日志文件：各节点的 logs 目录
- 时间同步：挂载宿主机的 /etc/localtime

### 操作
进入Hadoop目录执行
`docker compose up -d --build`
