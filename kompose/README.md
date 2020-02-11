
# Kubenetes集群部署方案

#### 部署介绍

船屋服务使用Kubenetes集群进行部署，包含测试环境以及生产环境，不同环境之间使用命名空间进行隔离。


#### 部署描述

| 文件  | 说明 | 访问方式 | 
| ------------ | ------------ | ------------ |
| client-deployment && svc  | 创建船屋点餐系统部署资源对象 (Deployment、Service)  | Loader Balancer |
| management-deployment && svc  | 船屋餐饮管理系统部署资源对象 (Deployment、Service)  | Loader Balancer |
| statistics-service-api-deployment && svc  | 统计服务API系统部署资源对象 (Deployment、Service)   | ClusterIP |
| statistics-service-redis-deployment && svc | 统计服务Redis缓存系统部署资源对象 (Deployment、Service)   |ClusterIP |
| statistics-service-worker-deployment && svc  | 统计服务Worker系统部署资源对象 (Deployment、Service)   |ClusterIP |
| statistics-service-db-deployment && svc  | 统计服务数据库系统部署资源对象  (Deployment、Service)  |ClusterIP |
| product-service-api-deployment && svc  | 产品服务API系统部署资源对象 (Deployment、Service)   |Loader Balancer |
| product-service-db-deployment && svc  | 产品服务数据库系统部署资源对象 (Deployment、Service、PVC)   |ClusterIP |

#### 部署准备

###### 1.由于部署的Docker镜像采用的私有仓库，Kubenetes节点在拉取镜像的过程中需要授权，所以需要先创建镜像仓库访问密钥，确保kubenetes有足够的权限拉取镜像

```shell
kubectl create secret docker-registry regcred --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```

###### 2.初始化产品服务数据库

```shell
# 1. 进入容器交互式操作终端
kubectl exec -it <your-pod-name> -c product-service-db sh -n <your-namespace>
# 2. 进入mysql命令控制台
mysql -u root -pP2ssw0rd
# 3. 执行SQL初始化脚本
```


###### 3.一键启动：

`
kubectl apply -f kompose/ -n boatouse-dev
`

