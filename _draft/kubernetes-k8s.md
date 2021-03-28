---
title: kubernetes-k8s
categories: java web
---

https://github.com/kubernetes/kubernetes
https://juejin.im/post/5db8c2b46fb9a020256692dc

https://www.katacoda.com/courses/kubernetes/playground k8s web playground
https://www.katacoda.com/courses/kubernetes

https://blog.csdn.net/qq_19734597/article/details/108337499 命令行简写

https://github.com/helm/helm 包管理器

<!--more-->
<!-- TOC -->

- [面试问题](#面试问题)
- [what and why](#what-and-why)
- [集群结构](#集群结构)
  - [一个 master 节点](#一个-master-节点)
  - [一群worker节点](#一群worker节点)
- [逻辑结构](#逻辑结构)
  - [pod](#pod)
  - [volume](#volume)
  - [controllers](#controllers)
  - [service](#service)
  - [flannel](#flannel)
  - [ingress](#ingress)
  - [name namespace](#name-namespace)
- [资源描述文件](#资源描述文件)
  - [通过命令生成 yml](#通过命令生成-yml)
  - [pod 的  yaml 描述文件](#pod-的--yaml-描述文件)
  - [deployment 的描述文件](#deployment-的描述文件)
  - [service](#service-1)
- [标签](#标签)
- [注解](#注解)
- [命名空间](#命名空间)
- [pod 管理](#pod-管理)
  - [探测 pod 是否健康](#探测-pod-是否健康)
  - [pod 副本](#pod-副本)
  - [运行单个任务 Job](#运行单个任务-job)
  - [定时任务 CronJob](#定时任务-cronjob)
- [pod 通信](#pod-通信)
  - [proxy](#proxy)
  - [Service](#service)
  - [pod 间通信](#pod-间通信)
  - [通过 Service 连接外部服务](#通过-service-连接外部服务)
    - [通过手动生成 endpoint](#通过手动生成-endpoint)
    - [通过 externalName 类型的 Service](#通过-externalname-类型的-service)
  - [将服务暴露给外部](#将服务暴露给外部)
    - [将服务的类型设置成 NodePort](#将服务的类型设置成-nodeport)
    - [将服务的类型设置成LoadBalance](#将服务的类型设置成loadbalance)
    - [创建一 个Ingress资源](#创建一-个ingress资源)
- [命令](#命令)
  - [安装](#安装)
  - [kind](#kind)
  - [minikube](#minikube)
  - [使用 kubectl](#使用-kubectl)
    - [基本信息](#基本信息)
    - [创建 create expose](#创建-create-expose)
    - [执行命令到容器 exec](#执行命令到容器-exec)
    - [修改](#修改)
    - [资源列表 get](#资源列表-get)
    - [查询详细描述 describe](#查询详细描述-describe)
    - [查询日志 logs](#查询日志-logs)
    - [删除 delete](#删除-delete)
  - [kubeadmin](#kubeadmin)
- [可视化界面](#可视化界面)
- [vagrant 搭建 k8s 环境](#vagrant-搭建-k8s-环境)
- [cka 证书](#cka-证书)
- [k3s](#k3s)

<!-- /TOC -->

## 面试问题

为什么需要 pod? 为什么不能简单地把所有进程都放在一个单独的容器中?

- 如果在单个容器中运行多个不相关的进程， 那么保持所有进程运行(如失败重启)、 管理它们的日志会困难(日志会混在一起)。




为什么要将一个 app 的 前端服务器 和 后端服务器 分为 两个 pod 部署?

- 造成计算资源浪费: 如果部署到同一个 pod, 那么对于 双节点 k8s, 将总是只能利用一个节点

- 扩缩容不便: k8s 扩容基本单位就是 pod, 前后端若部署到同个 pod, 会造成扩缩容粒度太大. 而且对于 数据库服务器, 扩缩容无法这样简单处理




如何决定是否将多个 container 划分到一个 pod?

- 扩缩容是否能够一起进行
- 是否可以在同个主机运行


## what and why

管理 docker image  的部署， 监控。 抽象了数据中心的硬件基础设施，使得对外暴露的只是 个巨大的资源池， k8s 自动决定组件部署到具体的哪一台 服务器节点。

只需要告诉 k8s 要做什么 (比如保持某个 pod 有三个副本), 而不必告诉它怎么做, 就像 sql 只告诉 数据库要查询那些数据, 而没有告诉数据库怎么查数据

为什么使用: 

当 container 太多, 需要管理, 单机情况下, 还能依靠 docker-compose 这些工具, 当有很多机器时, 就必须通过 k8s 来将 容器合理地分配到这些机器中去 

- 服务发现和负载均衡
- 自动部署 and 回滚
- 替换失败容器 (自我修复)

## 集群结构

### 一个 master 节点

master 节点：控制 and 管理整个集群

通常会有多个master node 高可用 , 故障转移

包含这些组件

- etcd: 分布式数据存储. 用来存储主节点的数据 (键值数据库)

- api server：网关, 和外界交互, 供客户端调用

- scheduler：调度， 决定哪个 pod 部署到哪个节点; 监视那些新创建的未指定运行节点的 Pod，并选择节点让 Pod 在上面运行

- controller manager：整个集群的管家，用来管理资源对象;如 复制节点， 持续跟踪工作节点

    - node controller 在节点出现故障时进行通知和响应
    - Replication Controller 维护正确数量的 Pod
    - Endpoints Controller填充端点(Endpoints)对象(即加入 Service 与 Pod)

Minikube 管理的是单节点 k8s， master node， 和 worker node 都在一台物理主机上


### 一群worker节点

worker 节点：运行实际部署的 容器

可以只有一个 worker node

包含这些组件    

- kubelet: 和 master 节点的 api server 通信 and 管理所在的 节点/机器上的 pod (创建、修改、监控、删除)

    相当于节点的代理, 和 master 节点交互

- kube-proxy: 负责 pod 间的负载均衡 网络代理

- Fluentd，主要负责日志收集、存储与查询

- container runtime (docker) 创建容器



## 逻辑结构

### pod


pod: 由 一个或多个相互关联的 container 组成, 是 k8s 管理的基本单位, 代表着集群中的一个虚拟主机

一个pod 只可能存在于一个 worker node 机器上, 不可跨节点

一个 pod 内的所有容器共享相同的 Linux network 命名空间. 这样每个 pod 就像一台独立的 主机, 有自己的 ip (集群内部地址), 主机名, 端口 (pod 中的 containers 共享 该 ip 以及 端口空间, 具有相同的 loopback 网络接口,可使用 localhost相互通信). 

可以和 docker-compose 启动的多个 container 类比

### volume

在 pod 中可访问的文件目录

可被挂载到 pod 中的一个/多个 container 的指定路径 下

### controllers 

pod 控制器, 部署/管理 pods

- replicaSet 创建/复制  pod 实例, 确保 pod 副本数量, 用来代替 replication Controller , 但是不能直接使用Replica Sets设置，而是要使用Deployments。Deployments包装了Replica Sets，在应用程序中设置并添加回滚更新升级功能

- Deployment 无状态应用部署, 指定一组 pod 的副本数量, 版本; 一次 deployment 会产生多个 pods

    管理 ReplicaSet, 支持滚动更新

- StatefulSet 有状态应用部署 (如 MySQL)

- daemonset 确保每个 node  都运行一个指定 pod

- job 一次性任务

- cronjob 定时任务

- replication Controller: 用于创建/复制  pod 实例. 确保 pod 实例数符合要求

    `kubectl run ...` 会创建一个 rc， 然后 rc 创建 pod （所以删除这样的 pod， 需要先删除 rc）

    工作流程：寻找 符合 label selector 的pod， 比较找到的 pod 和期望数量， 少了就从当前模板创建新的 pod 副本 ， 多了， 就删除多的 pod

    三个部分：

    - label selector： 用于确定ReplicationController作用域中有哪些 pod

    - replica count (副本个数）， 指定应运行的pod 数量

    - pod template (pod模板）， 用于创建新的pod 副本

    在新版的 K8s 中，建议使用 ReplicaSet 作为副本控制器，ReplicationController 不再使用了


### service

暴露 集群内部的一组 pods 的外部访问接口, 供外部访问, 以负载均衡. 有自己的静态 ip

类似 网关的角色, 通过某个 service, 可访问一组提供相同服务的 pods

另一个方法是： 端口转发 `kubectl port-forward`, 用于临时调试

### flannel

是CoreOS团队针对Kubernetes设计的一个网络规划服务，简单来说，它的功能是让集群中的不同节点主机创建的Docker容器都具有全集群唯一的虚拟IP地址, 让属于不同节点上的容器能够直接通过内网IP通信

### ingress

https://mritd.com/2017/03/04/how-to-use-nginx-ingress/

用来负载均衡多个 service, 相当于 nginx, 常用的实现是 ingress-nginx

原理: Ingress Contronler 通过与 Kubernetes API 交互，动态的去感知集群中 Ingress 规则变化，然后读取它，按照自定义的规则，规则就是写明了哪个域名对应哪个service，生成一段 Nginx 配置，再写到 Nginx-ingress-control的 Pod 里，这个 Ingress Contronler 的pod里面运行着一个nginx服务，控制器会把生成的nginx配置写入/etc/nginx.conf文件中，然后 reload 一下 使用配置生效。以此来达到域名分配置及动态更新的问题。


### name namespace

k8s resources name , every resouece have its own name, kind, metadata, spec, status...

namespace: isolate all all kinds of resources. resources of the same kind cannot have the same name in the same namespace

default init namespece: default, kube-system

## 资源描述文件

### 通过命令生成 yml

```sh
kubectl create deployment tomcat6 --image=tomcat:6.0.53-jre8 --dry-run -o yaml > tomcat6.yml

kubectl apply -f xxx.yaml


```

### pod 的  yaml 描述文件

```yml
apiVersion : v1 #(k8s api server version)
# Deployment、Job、Ingress、Service
kind: Pod #(对象类型)
metadata: #包括名称、命名空间、标签和关于该容器的其他信息
    name: kubia-manual
    # optional, default ns: default
    namespace: xxx
    # optional
    labels:
        xxx_key: xxx
        yyy_key: yyy
# 包括一些container，storage，volume, 探针...
spec: 
    containers:
    - image: xxx
      name: xxx
      ports:
      - containerPost: 8080 # 容器会监听的端口, 仅仅是展示, 没有 指定作用
        protocal: TCP
    resources:
        requests:
            cpu: 100m
            memory: 100Mi
    # 存活探针， 用于 pod
    # 在生产中 运行的pod, 一定 要定义 一 个存 活探针
#   optional
    livenessProbe:
    httpGet:
        path: /
        port: 8080
    # 如果没有设置初始延迟，探针将在启动时立即开始探测容器， 这通常会导致探测失败， 因为应用程序还没准备好开始接收请求。 如果失败次数超过阅值，在应用程序能正确响应请求之前， 容器就会重启。
    initialDelaySeconds: 15 # 在第一次探测前， 等待 15s
    [delay: 0] # 示在容器启动后立即开始探测。
    [timeout: 1] # 容器必须在1秒内进行响应， 不然这次探测记作失败
    [period: 10] # 每10秒探测一次容器
    [failure: 3] # 探测连续三次失败(#fa辽ure= 3)后重启容器
            
status: #包含运行中的 po 的当前信息，例如 po 所处的条件 每个容器的描述和状态，以及内部 IP 和其他基本信息

```

### deployment 的描述文件

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  ## optional
  nodeSelector:
    gpu: "true" #使用 标签选择器使得 pod 只被调度到符合要求的 worker node 
  # optional
  # select pods that will be affected by this deployment
  # have to match labels that specified in template.metadata.labels
  selector:
    matchLabels:
      app: nginx
  # optional
  replicas: 2 # tells deployment to run 2 pods matching the template
  # define pods
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80

```

### service

```yml
apiVersion: v1
kind: Service
metadata:
    name: nginx-service
spec:
    type: LoadBalancer
    ports:
    - name: nginx
      port: 80
      targetPort: 80
    selector:
        app: nginx
```

## 标签

标签 (label)：标签是可以附加到资源 (pod, node ...) 的任意键值对. 用来对 资源进行 分类管理

标签选择器：可以选出符合要求的 pod 子集

- 包含/不包含 特定 key 的 资源
- 包含特定 key， value 的 资源
- 包含 特定 key， 且 value 和指定的不同的资源

## 注解

注解: TODO


## 命名空间

K8S namespace: 为 对象资源提供了一个作用域, 在不同的 作用域中可以使用相同的资源名， 但这相同的资源名代表了不同的资源。 类比 java package

- 通过 label 对 pod 进行分组, 每个 组 可能有 pod 重叠, 通过 k8s namespace 分组可以完全避免重叠

- ns 只隔离资源， 不隔离运行的对象 （资源实例）， 如 不同ns 中的 pod 仍然可通信

- 适用于多用户 使用 k8s， 限制某个用户的资源使用量

从 ymal 文件创建 ns：

```yml
apiVersion: vl
kind : Namespace
metadata:
    name: custome-ns
```

`kubectl create -f custom-ns.yaml`

通过命令创建ns： `kubectl create namespace custom-namespace`

切换 ns： `kubectl config set-context $(kubectl config current-context) --namespace <other ns>`

## pod 管理

### 探测 pod 是否健康

k8s 会保持 pod 始终正常运行， 如果容器的主进程崩溃， Kubelet 将重启容器。 当容器被强行终止时，会创建一个全新的容器—-而不是重启原来的容器

但是 进程没有崩溃， 有时应用程序也会停止正常工作。 例如， 具有内存泄漏的 Java 应用程序将开始抛出 OutOfMemoryErrors, 但 JVM 进程会一直运行

- 方法1: 从 app 内部自己检测, 可以在应用中捕获这类错误， 并在错误发生时退出该进程。

    存在问题: 应用因为无限循环或死锁而停止响应, 这个方法就无效了

- 方法2: 须从外部检查应用程序的运行状况

    - 存活探针 (liveness probe) - 检查容器是否还在运行

        - HTTP GET 探针, 返回 2xx, 3xx 则代表正常, 不重启

        - TCP套接字探针, 和 pod 正常建立 链接, 正常, 不重启

        - Exec探针 , 进入 pod 某个 container, 执行任意命令, 并检查命令的退出状态码。如果状态码是 0, 则探测成功

```yml
apiVersion: vl
kind : Pod
metadata:
    name: custome-pod
spec:
    containers:
        - image: xxx
          name: xxx
          livenessProbe:
            httpGet:
                path: /
                port: 8080
```

### pod 副本

ReplicationController 是一种Kubernetes资源，负责创建和管理 pod 副本

被 ReplicationController 创建的 pod 为 “托管 pod”， 直接创建的 pod 为 “非托管 pod”。托管 pod 异常退出 （如 worker node 崩溃， 或 该 pod 崩溃）后， ReplicationController 会 创建新的 pod 代替

```yml
apiVersion: vl
kind: Replicationcontroller
metadata:
    name: kubia
spec:
    replicas: 3
    # 定义ReplicationController时不要指定 选择器，让Kubemetes从pod模板中提取它。这样YAML更简短。
    # rc 创建后， label selector 不会再更改
    # 一般创建后会更改 template
    [selector:
        app: kubia]
    # 创建新 pod 所用的 模板
    template:
        metadata:
            # 这里的 标签必须和 前面的 标签选择器 匹配， 否则将无休止地创建新的容器
            labels:
                app: kubia
            spec:
                containers:
                    - name: kubia
                      image: luksa/kubia
                      ports:
                        - containerPort: 8080


```

kubectl create -f kubia-rc.yaml

rc 创建后，由于没有任何pod有app=kubia标签， ReplicationController会根据pod模板启动三个新的pod

由ReplicationController创建的pod并不是绑定到ReplicationController。 在任何时刻， ReplicationController管理与标签选择器匹配的pod。 通 过更改pod的标签， 可以将它 从ReplicationController的作用域中添加或删除 

尽管一个 pod 没有绑定到一个 ReplicationController ，但 pod 在 metadata.ownerReferences 中引用它 以轻松使用它 来找到一 pod 的 Replication controller

升级 pod 可以这么做：编辑 rc 的template 后， 删除 所有要升级的 pod， rc 会自动创建升级后的 pod

================水平缩扩容：

方法1：`kubectl scale re kubia --replicas=3 ` 

方法2：`kubectl edit re kubia` 更新 `spec.replicas` 值


======================新的替代者 ReplicaSet

ReplicaSet：ReplicaSet 的行为与ReplicationController 完全相同， 但标签选择器的表达能力更强。

- 单个 rc 无法将 pod 与 （ label env=prod 和 env=dev ） 同时匹配，它只能匹配带有 env=devel 标签的 pod 或带有env=dev 标签的 pod
- rs 可以仅仅根据 是否有 label key 存在 来匹配

```yml
apiVersion: apps/v1beta2
kind: ReplicaSet
metadata:
    name: kubia
spec:
    replicas: 3
    selector:
        # 指定了多个表达式，则所有这些表达式都必须为true才能使选择器与
        # pod匹配。如果同时指定matchLabels和matchExpressions, 则所有标签都
        # 必须匹配，并且所有表达式必须计算为true以使该pod与选择器匹配
        [matchLabels:
            hah: hoo]
        matchExpressions:
            - key: hah # 要求 pod 包含 key 为 hah 的 label
              # 要求 label 值为 hoo
              # operator 可选 In (label 值需要匹配 value 中的一个), NotIn （label 值需要不在 values 中），Exists （label 必须有指定 key，不指定 values）
              # DoesNotExist （pod不得包含有指定名称的标签。values属性不得指定）
              operator: In 
              values: 
                - hoo
    # 创建新 pod 所用的 模板
    template:
        metadata:
            # 这里的 标签必须和 前面的 标签选择器 匹配， 否则将无休止地创建新的容器
            labels:
                hah: hoo
            spec:
                containers:
                    - name: kubia
                      image: luksa/kubia
                      ports:
                        - containerPort: 8080


```

================ DaemonSet 在每个 worker node 上部署一个 pod 

用来运行系统服务，如 kube-proxy

DaemonSet 管理下的 pod 不是随机分布在 worker node 上， 而是保证 每个 node 只存在一个 pod。 

若 node 下线， DaemonSet 不会在其他地方创建新的 pod， 但是当某个 node  的 pod 被意外删除，会重新创建新的 pod 代替

但是 当新的 node 添加进 集群， DaemonSet 会立即部署一个新 pod 到这个 node。

通过 `spec.nodeSelector` (节点选择器，原理是给 node 打标签) 只在特定 node 部署 pod

======================= Deployment ======= 更加方便的管理 Replica Set

Deployment 额外的特性：

- 事件，状态查看：可以查看Deployment的升级详细进度和状态
- 回滚：当升级pod镜像或者相关参数的时候发现问题，可以使用回滚操作回滚到上一个稳定的版本或者指定的版本
- 版本记录: 每一次对Deployment的操作，都能保存下来，给予后续可能的回滚使用
- 暂停和启动：对于每一次升级，都能够随时暂停和启动
- 多种升级方案：

    Recreate：删除所有已存在的pod,重新创建新的; 

    RollingUpdate：滚动升级，逐步替换的策略，同时滚动升级时，支持更多的附加参数，例如设置最大不可用pod数量，最小升级间隔时间等等。

================= HPA 弹性伸缩 根据资源的使用情况自动缩扩容

### 运行单个任务 Job

到目前为止， 我们只谈论了需要持续运行的 pod， 这些 pod 由 rc， rs， ds 管理。 你会遇到只想运行完成工作后就终止任务的清况， 这种 pod 由 Job 管理

该 pod 在内部进程成功结束时， 不重启容器。一旦任务完成， pod 就被认为处千完成状态

```yml
apiVersion: batch/v1
kind: Job
metadata:
    name: ba七ch-job
spec:
    [completions: 5] # 顺序运行 5 个 pod
    [parallelism: 2] # 最多可以 2 个 pod 并行运行
    [activeDeadlineSeconds: 120] # job 超时时间，若 pod 超时还没结束，系统将终止 pod， 标记 job 为失败
    [backOffLimit: 6] # job 标记为失败前的重试次数， 默认 6
    template:
        metadata:
            # Job 的 pod 选择器 将自动根据这里的 labels 创建
            labels:
                app: batch-job
        spec:
            # 重启策略， 指定 容器中进程结束后， k8s 做什么
            # 不能是 默认的 Always
            # 这里 Never 亦可
            restartPolicy: OnFailure
            containers:
                - name: main
                  image: luksa/batch-job

```

### 定时任务 CronJob

TODO

## pod 通信 

### proxy


### Service 

pods 有自己的内部地址, 但是无法从 cluster 外部访问, 通过 service 可以暴露到 cluster 外部

service 是一个逻辑结构, 定义一系列 pods, 以及他们的访问策略(通过类型指定)

- ClusterIP (default): 只能在 cluster 内部访问到 service
- NodePort: 外部可以使用 `<NodeIP>:<NodePort>` 访问 service
- LoadBalancer: 给 service 分配一个静态 ip, 可以负载均衡多个 pods
- ExternalName: 通过随机名字访问 service (可通过 externalName 在配置中指定). This type requires v1.7 or higher of kube-dns.

a default service named kubernetes will be created with type ClusterIP



可以定义配置文件, 指定标签选择器, 选择包含哪些 pods

```sh

```

### pod 间通信 

Service

作用：

- 服务的主要目标就是使集群内部的其他pod可以访问当前这组pod
- 对外暴露服务

如何创建？

方式1：`kubectl expose rc kubia --type=LoadBalancer --name kubia-http`

方式2：通过 `kubectl create -f xxx-service.yml`

```yml
apiVersion: vl
kind: Service
metadata:
    name: kubia
spec:
    # 配置会话亲和性
    # 特定客户端产生的所有请求每次都指向同 一个 pod
    # 还可选 None， 默认值。不支持 cookie
    [sessionAffinity: ClientIP]
    ports:
      - port: 80 # service 的可用端口
        # 可以引用 Pod 中 的 port name
        targetPort: 8080 # service将链接转发到 容器的端口
        # 不是必须， 多端口映射必须
        [name: xxx]
    selector:
        app: kubia
```

多端口映射

```yml
apiVersion: vl
kind: Service
metadata:
    name: kubia
spec:
    # 配置会话亲和性
    # 特定客户端产生的所有请求每次都指向同 一个 pod
    # 还可选 None， 默认值。不支持 cookie
    [sessionAffinity: ClientIP]
    ports:
      - port: 80 # service 的可用端口
        targetPort: 8080 # service将链接转发到 容器的端口
        name: xxx
      - port: 443
        targetPort: 8443
        name: yyy
    selector:
        app: kubia
```

如何发现 service

- 通过环境变量发现服务

    每个 pod 内会有环境变量表示 Service ip （`<pod name>_SERVICE_HOST`）， port (`<pod name>_SERVICE_PORT`)

- 通过  dns

    kube-system ns 中有个 pod 为 kube-dns，就是 dns server。在集群 中的其他 pod 都被配置成使用其作为 dns ( Kubemetes 通过修改容器的 /etc/resolv.conf 实现）。运行在 pod 上的进程 DNS 查询都会被 Kubemetes 自身的 DNS 务器响应

    pod 是否使用内部的 DNS 服务器是根据 pod 的 spec.dnsPolicy 属性来决定的

- 通过 FQDN （全限定域名） 连接服务

    `curl http://kubia.default.svc.cluster.local `

### 通过 Service 连接外部服务

pod  如何访问 k8s 外部 服务？

#### 通过手动生成 endpoint

Service 和 pod 中间不是直接相连， 而是插着 Endpoint，就是一个 ip:port 列表，在创建 Service 时 根据 该服务的 label selector 产生

如果 Service 没有指定 pod selector， 就不会自动生成 endpoints，需要手动生成，这时可以指定地址为任意 外部的 ip：

demo-service.yml:

```yml
apiVersion: vl
kind: Service
metadata:
    name: demo-service
spec:
    # 没有配置 selector

    # 将接收端口80上的传入连接
    ports:
      - port: 80
```

demo-service-endpoints.yml

```yml
apiVersion: vl
kind: Endpoints
metadata:
    # Endpoint的名称必须和 service 的名称相同
    name: demo-service
subsets:
    # Service将把请求重定向到 下面的 地址
    - addresses:
        - ip: 11. 11. 11. 11
        - ip: 22.22.22.22
      ports:
        - port: 80
```

如果稍后决定将 外部服务迁移到Kubemetes 中运行的pod, 可以为服务添加选择器，从而对Endpoint进行自动管理

#### 通过 externalName 类型的 Service

另外的方式来访问外部服务：创建ExtemalName类型的 Service

```yml
apiVersion: vl
kind: Service
metadata:
    name: external-service
spec:
    # 默认为 ClusterIP，只能 pod 间通信
    # 还有很多种 type
    type: ExternalName
    externalName: someapi.somecompany.com
    ports:
      - port: 80
```

然后在 pod 中通过 `external-service[.default.svc.cluster.local]` 访问  someapi.somecompany.com

在以后如果将其指向不同的服务，只需简单地修改 externalName 属性，或者将类型重新变回 ClusterIP 并为服务创建 Endpoint

### 将服务暴露给外部

#### 将服务的类型设置成 NodePort

每个集群节点都会在节点上打开一个端口(端口号相同)。并将传入的连接转发给 pod

```yml
apiVersion: vl
kind: Service
metadata:
    name: kubia-nodeport
spec:
    type: NodePort
    ports:
      - port: 80
        targetPort: 8080
        # 通过 集群任意节点的 30123 端口可以访问 这个 service
        # 如果不指定，Kubernetes将选择一个随机端口
        nodePort: 30123
    selector:
        app: kubia
```

worker node ip 如何获取：`kubectl get nodes -o jsonpath='...'`

#### 将服务的类型设置成LoadBalance

NodePort类型的一 种扩展, 在 client 和 worker node 间 放了一个 LoadBalancer， 防止节点单点故障

负载均衡器拥有自己独一无二的可公开访问的 IP 地址， 并将所有连接重定向到服务。可以通过负载均衡器的 IP 地址访问服务

```yml
apiVersion: vl
kind: Service
me七adata:
    name: kubia-loadbalancer
spec:
    type: LoadBalancer
    # 仅将外部通信重定向到接收连接的节点上运行的pod
    # 如果没有本地pod存在， 则连接将挂起。因此， 需要确保负载平衡器将连接转发给至少具有一个pod的节点。
    # 可避免额外的网络跳转。但是可能会导致跨节点的 pod 的负载分布不均衡
    [externalTrafficPolicy: Local ]
    portS:
        - port: 80
          targetPort: 8080
    selector:
        app: kubia
```

#### 创建一 个Ingress资源

通过一 个IP地址公开多个服务，它运行在 HTTP 层（网络协议第7 层）上， 因此可以提供比工作在第4层的服务更多的功能

为什么需要 Ingress：每个 LoadBalancer 服务都需要自己的负载均衡器，每个 loadbalancer 都需要自己的 ip， 而 Ingress 只需要一个公网 IP 就能为许多服务提供访问

当客户端向 Ingress 发送 HTTP 请求时， Ingress 会根据请求的主机名和路径决定请求转发到的服务。

Ingress 工作在 应用层 基于 cookie 的会话亲和性 (session affinity)

已经确认集群中正在运行 Ingress 控制器， 因此现在可以创建一个 Ingress 资源：

```yml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name : kubia 
spec :
    rules:
        # Ingress 将这个域名映射到你的 service
      - host: kubia . example.com
        http :
            paths:
              # 将请求发送到  kubia-nodeport 服务的 80 端口
              - path: /
                backend: 
                    serviceName: kubia-nodeport
                    servicePort: 80

```

`kubectl get ingresses ` 获取 Ingress 控制器 内网ip，配置到 dns server 

为 ingress 创建 tls 认证 TODO


## 命令

### 安装

```sh

```

### kind

https://github.com/kubernetes-sigs/kind/

在 docker 中运行 k8s, 更快更方便

https://www.cnblogs.com/laochiji/p/13813743.html 多节点


```sh
# install
# install golang && install docker
docker pull kindest/node:v1.19.1


# --name: specify a name for the cluster. default name : kind
#  --wait 30s: wait until the cluster reaches a ready status
# --wait 5m
# --kubeconfig: specify cluster config file; default location : ${HOME}/.kube/config
kind create cluster [--name kind] [--wait]

# 构建镜像
docker build -t my-custom-image:unique-tag ./my-image-dir
# 加载到 kind
kind load docker-image my-custom-image:unique-tag
kubectl apply -f my-manifest-using-my-image:unique-tag

# context name 规则: <kind>-<cluster_name>
# 仅仅一个 cluster 时, 可省略 context
kubectl cluster-info [--context kind-kind]

# 默认删除 kind 名字的 cluster
kind delete cluster [--name kind]

# 加载 images 到 cluster
# 默认 将 镜像加载到 kind 名字的 cluster
kind load docker-image <img_name> [--name cluster_name]

# 进入 kind 启动的 docker 内部, 执行 crictl images, 可以看
# 到用来模拟 k8s 的一系列镜像
docker exec -it kind-control-plane crictl images
```

### minikube

create single node k8s cluster for dev, 原理是 创建虚拟机, 

```sh

minikube version

minikube start


minikube dashboard

# 可用的扩展/插件
minikube addons list

# 开启插件
minikube addons enable metrics-server
# 关闭插件
minikube addons disable metrics-server


minikube stop

minikube delete

# 当前 node ip
minikube ip

```

### 使用 kubectl

https://kubernetes.io/docs/tasks/tools/install-kubectl/

#### 基本信息

```sh
kubectl version
kubectl cluster-info
kubectl get nodes
# 获取kube-scheduler和kube-controller-manager组件状态
kubectl get cs # 健康状况
```


#### 创建 create expose 

```sh

# create depend on a spec file.
kubectl create -f xxx.yml



# >>> deployment, 

# dep-name 不能随便设置, 必须符合域名命名规范.
# 自动创建的 ReplicaSet 名字为 dep_name + random uuid
# 自动创建 pod 名字为 ReplicaSet-name + random uuid
# 镜像地址 include the full repository url
# 自动创建 label -> run={dep name}
# auto  create selector -> run={dep name}
kubectl create deployment <dep name> --image=k8s.gcr.io/echoserver:1.4
kubectl create deployment tomcat6 --image=tomcat:6.0.53-jre8 # 不指定 url 前缀, 默认从 docker hub 拉取

# >>> proxy

#  pods 默认仅仅能被 同个集群的 pods, service  访问
# 创建临时的 代理, 可以将外部命令转发到 cluster 内部的 pod
# Ctrl +c 关闭
kubectl proxy
# 查 版本
curl http://localhost:8001/version
# 访问指定 pod 的服务
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/



# >>> pod

kubectl run <pod name> --image=nginx --restart=Never --port=80
# --generator=run/vl  表示同时创建 rc 管理 pod
kubectl run <pod name> --image=luksa/kubia --port=8080 --generator=run/vl 
# 从描述文件创建 资源， 可以 pod， rc。。。， 可指定 ns， 默认 default
kubectl create -f kubia-manual.yaml [-n xxx_ns]



# >>> service

# 创建 service, 名字同 deployment
# The LoadBalancer type makes the Service accessible through the minikube service command
# Port 8080 is specified in image code, so cannot be other port here
# --port : service 暴露在 cluster ip 下的端口, 通过这个端口,  可以在 cluster 内部访问到多个 pod. 通过这个 service 负载均衡了 多个 pod
# --targetPort 容器的端口, 默认=== port, 与制作镜像时暴露的端口一致, 不能随便指定
# --type: service 类型, 
#       NodePort: 创建一个 service 并暴露到 cluster 外部, service 的端口还是 --port 指定, 同时 service 有自己的 cluster url, node 会指定一个随机端口映射到 service 的 端口, 若指定了 --nodePort, 就用指定端口映射到 service 端口; 如 type=NodePort，而且配置 nodePort=30001, 那么外部机器 可以 scheme://nodeIP:30001访问到该服务
#       LoadBalancer: 
kubectl expose deployment <dep name> --type=LoadBalancer --port=8080 

# 查找暴露的随机端口
kubectl describe service mywebservice | grep NodePort

# service port 80 will mapping to random node port ,and this service can be visited by someone outside cluster
# 若扩容, 则在各个有 pod 运行的节点都能通过 node 端口访问  这个 service
kubectl expose deploy tomcat6 --port=80 --target-port=8080 --type=NodePort

# 创建一个负载均衡服务 service 对象. 可以从外部访问 pod
# rc 为 replicationController 缩写
kubectl expose rc <po name> --type=LoadBalancer --name kubia-http


# >>> label

# 语法: kubectl label <obj_type, eg: pod, service> <obj_name> <label, eg: app=v1>
kubectl label pod $POD_NAME app=v1


# >>> 命名空间 

# 创建
kubectl config set-context Kubernetes --namespace=beta
# 验证
kubectl config view | grep namespace command
```


#### 执行命令到容器 exec

```sh
# 仅有 一个容器在 pod, 无需指定容器
kubectl exec [-c container_name] $POD_NAME env

# 进入 容器内部
kubectl exec -ti $POD_NAME bash
```


#### 修改

```sh

# >>> 扩容

# 指定 replicas 可以缩扩容
$ kubectl scale deployments/<dep_name> --replicas=4
kubectl scale deployment.apps/tomcat6 --replicas=2
kubectl scale deployment tomcat6 --replicas=2


# >>> 滚动更新

# 更新 image version
$ kubectl set image deployments/<dep_name> <dep_name>=jocatalin/kubernetes-bootcamp:v2
# 验证更新是否成功
$ kubectl rollout status deployments/kubernetes-bootcamp

# >>> 回滚

# 取消之前的部署，这将执行反向操作，将负载移动至之前版本的容器
$ kubectl rollout undo deployments/kubernetes-bootcamp
```


#### 资源列表 get

单复数均可

```sh

# 所有
kubectl get all


# >>> deployment

kubectl get deployments


# >>> pods

# --show-labels 同时显示所有标签。 
# -A 查看所有 pods, 包括 kube-system 命名空间 下的 pods
# --all-namespaces 所有空间的 pods
kubectl get pods [--show-labels]

# -o 输出格式; wide 表示输出文本信息, 列出 ip , 节点 name
# -o yaml 
kubectl get pods -o wide

# 通过标签过滤
kubectl get pods -l <label>

# 获取 pod 完整 yaml/json 文件
# 缩写 po
kubectl get po <pod name> -o yaml
kubectl get po <pod name> -o json
kubectl get node

# 根据模板获取 信息
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')


# >>> service


# 查看 service  的 endpoint 列表
kubectl get endpoints <service name>
# 查看所有 svc
# 缩写 svc
kubectl get services


# >>> ReplicaSet

kubectl get rs




# 端口转发， 将会运行一个端口转发代理
# 将 local port 8888 转发到 pod 的 8080
kubectl port-forward <pod name> 8888:8080

# 标签
# 列出 pods 的所有标签
kubectl get pods --show-labels
# 只列出感兴趣的标签， 标签按照列展示
kubectl get po -L creation_method,env
# 添加
kubectl label po <pod name> creation_method=manual
# 修改
kubectl label po <pod name> creation_method=manual --override

# 标签选择器
kubectl get po -1 creation_method=manual
kubectl get po -1 creation_method!=manual
kubectl get po -1 env
# 列出没有 env 的pod
kubectl get po -1 '!env'
# 选择带有env标签且值为prod或devel的pod
kubectl get po -1 env in (prod, devel)
kubectl get po -1 env notin (prod, devel)

# 使用 label 分类 worker node
kubectl get nodes
kubectl label node <node name> gpu=true # 每个 node 都有一个默认 label， kubernetes.io/hostname:主机名 
kubectl get nodes -l gpu=true

# 命名空间
# 查看所有 ns, 有 default， kube-public， kube-system，默认我们使用的是 default
kubectl get ns 
# 列出只属于该命名空间的 pod, 可使用 -n
kubectl get po --namespace kube-system


# 编辑 rc 的 yaml
# 一般仅仅编辑 rc 的 template， 不动 label selector
# 用来升级 pod
kubectl edit rc kubia

# 增加 副本数， 水平扩容
kubectl scale rc kubia --replicas=3 
# 等同 parallelism
kubectl scale job xxx_job --replicas 3 

# 远程在 pod 中某个 container 执行命令
# “--” 表示 kubectl 命令结束， 后面的是在 pod 内执行的
kubectl exec <pod name> -- curl -s http://10.23... 
# 列出环境变量，如 Service 的 ip， 端口
kubectl exec kubia-3inly env
# 在 pod 中执行 shell
kubectl exec -it kubi-3inly bash


# >>> 配置文件

kubectl config view



# >>> 事件

kubectl get events

# 通过 describe 也能得到 object 相关的事件

```


#### 查询详细描述 describe

```sh
# >>> pods

# 查看资源描述， 附加信息， 作为 kubectl get xxx xxx 补充, 可用于排查问题
# 如 pod 是否正在运行， 重启原因， 退出码 137，143 表示 pod 被迫终止
kubectl describe pods [pod_name]


# >>> service

kubectl describe services/<srv name>

# >>> deployment

$ kubectl describe deployment


```


#### 查询日志 logs

```sh
# 查指定 pod 下的 container 日志
# 若pod 只有一个 container, 无需指定 container
kubectl logs $POD_NAME
kubectl logs -f POD-NAME


# 需要 ssh 到 pod 所在的 worker node
docker logs <container_id>
# 无需 ssh, 可指定某个 container， 可指定看重启前的版本的日志
kubectl logs <pod name> [-c container_name] [--previous]




# 查看 k8s 资源
kubectl explain pods
kubectl explain pod.spec

```

#### 删除 delete

```sh
# delete by spec file
kubectl delete -f flask.yaml

# 删除 pod
# 无法直接删除, 只要 replicaSet 没有删除, 删了 po, 还会重现创建 po
# 可直接 通过删除 deployment 删除
kubectl delete po kubia-gpu [pod2 [pod3]]
# 删除当前 ns 下 all pod， 保留 ns
kubectl delete po --all
# 按照标签删除
kubectl delete po -1 creation_method=manual

kubectl delete service hello-node
kubectl delete deployment hello-node

#  删除 ns, 同时会删除 ns 下的所有 pod
kubectl delete ns custom-namespace
# 删除所有资源， 包括 rc， pod， service
kubectl delete all --all
# 删除 rc， rc 管理的 pods 也会删除
kubectl delete rc <rc name>
# 删除 rc， 但是保留 pods
kubectl delete rc <rc name> --cascade=false
kubectl dele七e rs kubia 


```

### kubeadmin

```sh
kubeadm init # create a master node

kubeadm join <master_id:port> # add a node to cluster
```

## 可视化界面

kubernetes-dashboard

更推荐 kubesphere, 打通 devops 全套流水线, 对 cluster 要求高  https://github.com/kubesphere/kubesphere


(kuboard 也行, 对 cluster 要求不高)

https://www.zhihu.com/question/348609092 rancher 和 kubesphere对比



## vagrant 搭建 k8s 环境

一个 master node: docker, kubeadm, kubelet, kubectl
两个 worker node: 

Vagrantfile:

```vagrantfile

Vagrant.configure("2") do |config|
   (1..3).each do |i| 
        config.vm.define "k8s-node#{i}" do |node|
            # 设置虚拟机的Box 
            node.vm.box = "centos7"

            # 设置虚拟机的主机名
            node.vm.hostname="k8s-node#{i}"

            # 设置虚拟机的IP
            # 192.168.56.xxx 需要在 virtualbox 先创建 host only network 192.168.56.1
            node.vm.network "private_network", ip: "192.168.56.#{99+i}/24", netmask: "255.255.255.0"

            # 设置主机与虚拟机的共享目录
            # node.vm.synced_folder "~/Documents/vagrant/share", "/home/vagrant/share"

            # VirtaulBox相关配置
            node.vm.provider "virtualbox" do |v| 
                # 设置虚拟机的名称
                v.name = "k8s-node#{i}"
                # 设置虚拟机的内存大小
                v.memory = 4096
                # 设置虚拟机的CPU个数
                v.cpus = 4 
            end 
        end 
   end 
end

```

虚拟机准备


```sh
# 登录 machine sshd, 保证可以通过账号密码登录 (默认仅仅 ssh 登录)
# 若使用 tmux + ssh config 则无需无需这一步, 直接通过 ssh 登录即可
vagrant ssh k8s-node1
sudo su
vi /etc/ssh/sshd_config # 修改 为 PasswordAuthentication yes
service sshd restart


# 设置默认网卡

# show default SoftEther
# We can find that the default soft ether is eth0, the three machine have the same eth0 ip.
# This is because eth0 default to 网络地址转发(nat) 类型. in order  to make the machines can 
# access to each other, etho should be type: Natnetwork
ip route show
# 操作 virtualbox 图形界面, 全局新建 natnetwork 网络. 为每个 machine 默认网卡1 即 eth0 指定 这个类型, 刷新高级设置
# 里的 网卡mac 地址
# 然后在各个 machine 内 互相 ping 是否通

# close firewall
systemctl stop firewalld
systemctl disable firewalld

# disable selinux (永久)
sed -i 's/enforcing/disabled/' /etc/selinux/config # 替换了 enforcing 为 disable
# 当前会话关闭 selinux (临时)
setenforce 0

# 关闭 swap 永久
sed -ri 's/.*swap.*/#&/' /etc/fstab # 其实就是注释掉一行   #/swapfile none swap defaults 0 0
# 关闭 swap (内存交换) 临时
swapoff -a

# 添加 主机名 and ip 映射
# hostname 会显示 主机名 k8s-node1/2/3
# 若 hostname 显示不正常可以修改: hostnamectl set-hostname <new hostname>
vi /etc/hosts
# eth0 网卡就是 默认网卡, 用来machine 互联的
# <node1 eth0 ip> k8s-node1
# <node2 eth0 ip> k8s-node2
# <node3 eth0 ip> k8s-node3
10.0.2.15 k8s-node1
10.0.2.4 k8s-node2
10.0.2.5 k8s-node3

# 将桥接的 ipv4 的流量传递到 iptables 的链
cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
# 验证
sysctl --system

# 同步三台机器的时间
yum install ntpdate -y
ntpdate time.windows.com

# 将只读文件系统 改为 读写
mount -o remount rw /

```

安装 docker 等

```sh
# docker参照官网即可

# 安装 k8s 需要 aliyun 镜像 推荐
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
        http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF

# ustc 的镜像
cat <<EOF > /etc/apt/sources.list.d/kubernetes.list
deb http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial main
EOF
# centos
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
deb http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial main
EOF



systemctl enable kubelet
systemctl start kubelet


# >>> 在 master node 上

# 需要拉去 k8s 相关的镜像, 默认仓库 k8s.gcr.io 背墙, 这里重新指定
# service-cidr 是 service 间通信的网段/子网
# pod network-cidr 是 pod 间通信网段/子网
kubeadm init --apiserver-advertise-address=<master node eth0 ip> \
--image-repository registry.cn-hangzhou.aliyuncs.com/google_containers \
--kubernetes-version v1.19.4 \
--service-cidr=10.96.0.0/16 \
--pod-network-cidr=10.244.0.0/16

# 按照 输出提示, 如下步骤需要做:
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# master 生成的 命令, 在 worker node 执行即可加入 cluster.
# 2h 内有效
kubeadm join 10.0.2.15:6443 --token 0tpws3.y1uhbzg0rgal4okz \
    --discovery-token-ca-cert-hash sha256:0a25a082e0babbc18d1646ebfa9954d82838fa86ec66ee3d88c0d57fc9c0f35e 
# 过期之后, 如下生成永久的 token
kubeadm token create --print-join-command
kubeadm token create --ttl 0 --print-join-command # 复制生成的 token 到上面命令即可

# 创建网络
# 这里选定 flannel 提供的网络扩展插件 (addons)
# 具体有哪些可用的插件: https://kubernetes.io/docs/concepts/cluster-administration/addons/
curl -L https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml -o flannel_network.yml
# 上传到 master node
scp ~/flanel.yml root@xxx/root
# 子节点加入到 cluster 后, master 会同步 flanel 相关的 pod 到 子节点上, 可以监控, 等待一段时间再查看 nodes
kubectl apply -f flannel.yml
# 查看 flanel 相关 的 pods 是否启动
kubectl get po --all-namespaces
# 如果是 失败的, 那么小替换 flanel.yml 中 image 相关地址, 到 docker hub 上找, 如 https://hub.docker.com/r/easzlab/flannel
# 删除 , 重新创建
kubectl delete -f flannel.yml

 
# 此时可以在子节点上执行添加 node 命令了   >>> 在 worker node 上


# 监控 所有节点上的 指定ns 的 pod
# 等待 所有节点 上 flanel 相关的 pod 都正常启动后再在 master 上查看 nodes 状态, 保证所有 node 状态为 ready 则 cluster 搭建成功
watch kubectl get po -n kube-system -o wide

# 安装 ingress controller, 
# 有多重实现, 选择一种即可 https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/
# 这里选择 ingress-nginx https://kubernetes.io/docs/concepts/services-networking/ingress/
# ref: https://www.cnblogs.com/minseo/p/12455320.html


```


## cka 证书


https://training.linuxfoundation.cn/certificate/details/1 中国区

https://zhuanlan.zhihu.com/p/138796893
https://www.jianshu.com/p/629525af31c4
https://www.kubernetes.org.cn/peixun 考试要点
https://zhuanlan.zhihu.com/p/138796893 , https://www.zhihu.com/zvideo/1302356091591372800 真题


## k3s

轻量级的 k8s, 内核机制还是和 K8s 一样，但是剔除了很多外部依赖以及 K8s 的 alpha、beta 特性

将其应用于 IoT 设备（比如树莓派）

https://rancher.com/docs/k3s/latest/en/ 官网

https://www.rancher.cn/k3s/ 中文官网

https://zhuanlan.zhihu.com/p/269556628 简介