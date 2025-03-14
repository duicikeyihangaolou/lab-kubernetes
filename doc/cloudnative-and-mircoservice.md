# 云原生和微服务

## 注意 ⚠️

- _斜体表示引用_
- **未经允许，禁止转载**

## 学习本课程前，你应该具备如下知识：

- Linux 系统的基本配置和命令
- Web 开发经验
- Docker 命令
- K8S 基本配置和 kubectl 命令
- 对存储和网络有基本了解更佳

## 课程目录

| Date  | Time | Title                | Content                                            |
| ----- | ---- | -------------------- | -------------------------------------------------- |
| 第 1 天 | 上午   | [1. 云原生](#1-云原生)     | [1.1 什么是云原生](#11-什么是云原生)                           |
|       |      |                      | [1.2 容器化和持续交付](#12-容器化和持续交付)                       |
|       | 下午   |                      | [1.3 微服务和 API 设计](#13-微服务和-api-设计)                 |
|       |      |                      | [1.4 有状态服务](#14-有状态服务)                             |
|       |      |                      | [1.5 WebSocket](#15-websocket)                     |
|       |      |                      | [1.6 视频流](#16-视频流)                                 |
| 第 2 天 | 上午   |                      | [1.7 基于 K8S API 的云原生](#17-基于-k8s-api-的云原生)         |
|       |      | [2. 微服务](#2-微服务)     | [2.1 微服务应该有多微](#21-微服务应该有多微)                       |
|       | 下午   |                      | [2.2 服务注册和服务发现](#22-服务注册和服务发现)                     |
|       |      |                      | [2.3 微前端](#23-微前端)                                 |
|       |      |                      | [2.4 云安全](#24-云安全)                                 |
|       |      |                      | [2.5 事件](#25-事件溯源和-cqrs)                           |
| 第 3 天 | 上午   | [3. istio](#3-istio) | [3.1 微服务框架](#31-微服务框架)                             |
|       |      |                      | [3.2 环境搭建](#32-环境搭建)                               |
|       |      |                      | [3.3 流量监控和治理](#33-流量监控和治理)                         |
|       | 下午   |                      | [3.4 istio 运维建议](#34-istio-运维建议)                   |
|       |      |                      | [3.5 基于 KubeSphere 的服务网格](#35-基于-kubesphere-的服务网格) |
|       |      |                      | [3.6 灰度发布](#36-灰度发布)                               |
|       |      |                      | [3.7 服务治理](#37-服务治理)                               |

## 1. 云原生

[返回目录](#课程目录)

### 1.1 什么是云原生

[返回目录](#课程目录)

**把业务上云，然后只关注业务本身**。

云基座解决业务无关的问题：

- 基础算力（K8S/OpenStack）：虚拟机、裸机、容器
- 基础运维（K8S）：自动调度、自愈、快速扩缩容
- 基础服务治理（K8S）：服务注册、服务发现、配置管理、后台任务管理
- 中间件：有状态服务，MQ、DB、Cache、对象存储、文件存储等
- 高级服务治理（Istio）：故障注入、流量治理、灰度发布、熔断

### 1.1.1 云计算的发展

计算和虚拟化的发展

1. 60-70 IBM，大型机：集中式计算
2. 80-90 VMWare，个人 PC：分布式计算
3. 2005-至今 Amazon，云计算+移动互联网：泛在计算。5G 边缘计算还在路上

云计算的分类

1. IaaS：是最为主流的云计算，以 AWS 为代表，是一种基于虚拟化技术的自助式服务平台
2. PaaS：GAE
3. SaaS：Office 365

云计算的上半场：

1. 百花齐放：开源云计算框架 + 开源生态，群雄逐鹿
2. 功能单一：IaaS 占绝大多数体量，PaaS / SaaS 在试水
3. 虚拟化 / 裸机 / 容器三者相对分离

云计算的下半场：

1. 公有云倾销私有云
   - 公有云推本地化部署，称混合云（有哪些适用场景？），挤压私有云市场
   - 开源云计算框架热度减退，也趋于稳定，**自主可控，开放原生，信创兼容，可定制**成为最主要卖点
2. 融合和统一
   - 算力统一调度成为主流：VMWare 的传统优势和新变化
   - 接口统一：Kubenetes API 成为主流
   - 广泛的服务和中间件
3. 更智慧
   - AI 的发展衍生了各类智慧类垂直领域云，GPU 渲染云，AI 平台云等
   - 算力调度云边协同等
   - AIOps

### 1.1.2 云原生的基本原则

云原生的 3 原则

- **简单**
- **易于自动化**
- **易于发布**

与之对应的解决方案：**API + 微服务 + 容器化**

[云原生的十二要素法则](https://12factor.net)

1. 用同一份受版本控制的基准代码，进行多处部署
2. 显式声明依赖关系
3. 配置信息依赖应用环境存储（即配置管理），与代码分开
4. 后端服务视作附加资源（Web Service / DB / MQ）
5. 严格分离构建和运行步骤
6. 以一个或多个无状态进程运行应用
7. 通过端口绑定提供服务
8. 通过进程模型进行服务扩缩容
9. 确保快速启动和优雅中止来最大化健壮性
10. 尽可能保持开发、预发布、线上环境一致
11. 日志处理成事件流（方便后续统一处理）
12. 用一次性进程运行后台管理任务

### 1.1.3 如何判断你的技术栈和代码实现是否云原生？

1. 所做的任何事（编辑和编译工具、语言和框架）是否简单？
   - IDE 是否可选？强制性是不足取的，比如代码必须用特定的 IDE 才能生成或者编译。
   - 能否通过命令行（即能否自动化）构建和部署？
   - 团队新成员能否快速理解代码？
2. 是否测试驱动开发（本质上即是否有信心交付）？
   - 测试是 trade off 的艺术，编程人员只需要为他们认为不自信的代码编写单元测试
   - 单元测试只对程序员有意义，代码覆盖率是单元测试的客观指标，太低不合适，一般认为需要覆盖 60% 以上
   - 测试工程师需要为所有的 API 测试和功能测试、界面测试负责，尽可能自动化，测试工程师不考虑代码覆盖率，而是考虑测试用例覆盖率
   - 一个 bug 被研发自己发现，和被测试人员发现，和被现场使用者发现，cost 不可同日而语
3. 尽早发布、频繁发布
   - 代码应该少量提交，多次提交，每次提交应该涵盖一个完整的小功能或者 bug 修复
   - 尽早发布、频繁发布才能让产品研发不畏惧发布，减少发布风险
   - 只有足够的自动化测试，才能实现尽早发布、频繁发布
4. 一切能自动化的，都应自动化
   - 任何每天要做的事，都应该被自动化
   - 流程中任何时常重复的部分，如果不能被按钮或者脚本代替，那么就属于过于复杂、脆弱的设计
5. 一切事物都是服务，包括应用
   - 一切服务都是微服务，micro 的前缀完全没必要
   - 单体为什么不好？
     - 每次变更都需要全量发布整个应用程序，维护困难
     - 启动和停止速度慢
     - 依赖关系耦合紧密

     因此，反之，如果应用恰好没有这三个问题，那么单体就是最好的设计，不需要硬拆微服务做过度设计。

### 1.2 容器化和持续交付

[返回目录](#课程目录)

#### 1.2.1 应用容器化（虚拟化到容器）

虚拟化的目的：1. 资源隔离，2. 资源限制

虚拟化使得在一台物理服务器上可以跑多台虚拟机。

虚拟化共享物理机的 CPU、内存、IO 硬件资源，但逻辑上虚拟机之间是相互隔离的物理机我们一般称为宿主机（Host），宿主机上面的虚拟机称为客户机（Guest）Hypervisor 程序将 Host
的硬件虚拟化，并提供给 Guest 使用，分为：全虚拟化（一型虚拟化）和半虚拟化（二型虚拟化）。

全虚拟化 Hypervisor 是一个特殊定制的 Linux 系统，直接安装在物理机上。比如 Xen 和 VMWare 的 EXSi。全虚拟化一般对硬件虚拟化功能做了特别优化，性能更好。

半虚拟化先安装标准操作系统，Hypervisor 作为 OS 上的一个程序模块运行，并对虚拟机进行管理。比如 KVM，VirtualBox，VMWare WorkStation，VMWare
Player，VMWare Fusion 等。半虚拟化基于普通操作系统，比较灵活，比如支持虚拟机嵌套（在 KVM 虚拟机中再运行 KVM）。

**容器化是轻量级的虚拟化**。

![](/image/container-evolution.png)

Docker 容器

![](/image/docker-undertech.png)

![](/image/docker-arch.png)

##### 1.2.1.1 Docker 和 Containerd

Containerd 是从 Docker 中分离出来的一个项目，是一个工业级标准的容器运行时，它强调简单性、健壮性和可移植性。

![](/image/k8s-cri-docker.png)

![](/image/k8s-cri-containerd.png)

![](/image/docker-and-kubernetes-use-containerd-2000-opt.png)

实验：[应用容器化和 Docker 基本操作](kubernetes-best-practices.md#213-docker-部署和基本使用)

![](/image/k8s-containerd-tools.png)

实验：[Containerd 相关工具：nerdctl](kubernetes-best-practices.md#2223-nerdctl)

**容器技术栈**

![](/image/k8s-cri-tools.png)

##### 1.2.1.2 K8S

K8S 的操作要记得参考：<https://kubernetes.io/>

[K8S 有哪些组件](https://kubernetes.io/zh/docs/concepts/architecture/#)？api-server、kube-scheduler、kube-controller、etcd、coredns、kubelete、kubeproxy

组件结构图

![](/image/k8s-architecture.png)

实验：[K8S 基本操作](kubernetes-best-practices.md#3111-单节点集群部署)

实验：部署应用到 K8S
平台：[Github](https://github.com/99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#29-%E5%90%AF%E5%8A%A8%E4%B8%80%E4%B8%AA-pod)
或
[Gitee](https://gitee.com/dev-99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#29-%E5%90%AF%E5%8A%A8%E4%B8%80%E4%B8%AA-pod)

#### 1.2.2 弹性容器实例（ECI）

弹性容器实例的功能项包括：

- 同时兼容原生容器和虚拟机的使用体验
- 比虚拟机轻量的资源分配能力，以方便资源快速申请、弹性
- 类似虚拟机的使用体验，可登陆，可任意安装组件
- 有固定 IP 地址，胖容器从创建到删除，IP 地址保持不变
- 可以通过 SSH 远程登陆系统。
- 严格的资源隔离，如 CPU、内存等

实现是一般是提供一个 Pod，Pod 中的容器可以是 OCI VM（比如 Kata），或者单纯容器。

哪些云厂商提供 ECI？

- [博云胖容器](https://mp.weixin.qq.com/s?__biz=MzIzNzA5NzM3Ng==&mid=2651860132&idx=1&sn=cb0eac52be444c162fb505ebbdef6c0f&chksm=f329546bc45edd7d4789aa85ae5edb8a9f8d8313dd1ca273946ef019e0997b39665d29e24c8c&scene=21#wechat_redirect)
- [Kata 和它的朋友们](kubernetes-best-practices.md#24-kata-和它的朋友们)
- [阿里云 ECI](https://eci.console.aliyun.com/#/eci/cn-shanghai/list)：[从 Docker Hub 拉取镜像创建实例](https://help.aliyun.com/document_detail/119093.html)

#### 1.2.3 弹性 K8S 服务（EKS）

K8S 作为一个整体的资源（类同 VM / 存储 / VPC）对外提供。公有云，私有云已广泛提供
EKS。比如：[阿里云容器服务 ACK](https://cs.console.aliyun.com/#/k8s/cluster/list)

#### 1.2.4 CI/CD

发布令人畏惧（深夜里所有 IT
人员一起在电话里讨论，各种失败情况的回退预案，发布后仍战战兢兢如同劫后余生，不知道工作日时是否有问题会突然冒出来），因为对应用程序是否能如预期一样运行没有信心。只有时刻发布才能不畏惧发布，才能不加班、不熬夜、不四处救火，“善战者无赫赫之功”。

托管的 CI/CD 包括：github workflow，阿里云 devops 云效

自建的 CI/CD 包括：jenkins，drone，建木，tekton, algoCD（gitOps）

Jenkins 是最使用最广的自动化任务管理框架。它有以下缺点：

- Jenkins 配置文件难以通过版本控制进行管理。这一点 OpenStack Zuul 致力于解决。
- Jenkins 基于 Java，需要资源比较多。
- Jenkins 集中式 CI/CD 的代表，所有 CI/CD 逻辑在 Jenkins 侧统一存储和管理。项目代码与 CI/CD 代码分离。

云原生建议项目代码与 CI/CD 代码放在一起，一同进版本控制，这样 CI/CD 自动化框架本身就可以是无状态的，非常适合扩展，也因此具备良好的鲁棒性。

参考：[Github](https://github.com/open-v2x/roadmocker/tree/master/.github/workflows) 或
[Gitee](https://gitee.com/open-v2x/roadmocker/tree/master/.github/workflows)

### 1.3 微服务和 API 设计

[返回目录](#课程目录)

#### 1.3.1 微服务设计

##### 1.3.1.1 微服务的定义和兴起

参考：<http://blog.wuwenxiang.net/Design-Pattern>，微服务遵循设计模式 6 原则中的**单一责任原则（SRP）**，即一个服务只负责一个功能。

在 [1.1.3 章节中](#113-如何判断你的技术栈和代码实现是否云原生)，提到：一切服务都是微服务，micro 的前缀完全没必要。

由上可知，微服务是一个宽泛的计算机编程哲学理念，核心就是 SRP。

面向服务编程（SOA）的理念兴起于亚马逊 Amazon，参考 [程序员的呐喊](https://book.douban.com/subject/25884108/)：

Amazon 的 SOA 法则：

- 从今天起，所有的团队都要以服务接口的方式，提供数据和各种功能。
- 团队之间必须通过接口来通信。
- 不允许任何其他形式的互操作：不允许直接链接，不允许直接读其他团队的数据，不允许共享内存，不允许任何形式的后门。唯一许可的通信方式，就是通过网络调用服务。
- 具体的实现技术不做规定，HTTP、Corba、PubSub、自定义协议皆可。
- 所有的服务接口，必须从一开始就以可以公开作为设计导向，没有例外。这就是说，在设计接口的时候，就默认这个接口可以对外部人员开放，没有讨价还价的余地。
- 不遵守上面规定，就开除。

SOA 的事件教训：

1. SOA架构的错误定位，非常麻烦。

   一个请求可能要经过 20 次服务器调用，才能找到问题的真正所在。通常，单单是问题的定位就要花费 15 分钟到几个小时，除非搭建大量的外围监控和报警措施。

2. 同事也是潜在的 DOS 攻击者。

   公司内部某个小组，会突然对你的服务发起大量请求。除非每个服务都设有严格的用量和限量措施，否则根本无法保证可用性。

3. 监控和质量保障（QA）是两回事。

   监控一个服务的时候，可能会得到“一切正常”的回复。但是很有可能，整个服务唯一还正常工作的部分，就是这个回应“一切正常”的模块。只有完整地调用服务，才能确定服务是正常的。

   这意味着，真正监控一个服务，必须做到对所有的服务和数据进行完整的语意检查，否则是看不出问题的。如果做到了这一点，本质上就是在做自动化 QA 了。

4. 必须有服务发现机制。

   面对成百上千的服务时，没有服务发现机制是不可想象的。这又离不开服务注册机制，而它本身也是一个服务。亚马逊有一套统一的服务注册机制，可以通过编程的方式找到所有服务，包括一个服务有哪些
   API，目前是不是运行正常，在什么位置等。

5. 必须有沙箱用来调试

   如果代码中调用了他人服务，查找问题的难度要高很多，除非有统一的方式在沙箱里运行所有服务，否则几乎不可能进行任何调试。

6. 不能信任任何人

   团队采用服务的方式进行合作以后，基本上就不能信任其他团队了，正如不能信任第三方工程师一样。

##### 1.3.1.1 典型的微服务框架：OpenStack

可以认为，迄今最成功的微服务和面向服务应用厂商，就是亚马逊。OpenStack 从创始至今，就一直在追随亚马逊（本质上所有的云厂商都在追随亚马逊的云计算服务神迹），从产品设计到架构实现。

基本的设计原则：

- 整个 OpenStack 拆分成若干模块，各模块中包含若干服务
- 模块中必须有一个 API 通信服务，模块之间通过该 API 服务彼此通信
- 模块内通过消息队列通信
- 服务必须能横向扩展

![](/image/openstack-map-v20221001.jpeg)

![](/image/openstack-arch-design.png)

#### 1.3.2 Restful API

Restful API
设计：[Github](https://github.com/duicikeyihangaolou/training-python-public/blob/master/doc/autotest.md#41-rest-api-%E6%8E%A5%E5%8F%A3%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
或
[Gitee](https://gitee.com/wu-wen-xiang/training-python/blob/master/doc/autotest.md#41-rest-api-%E6%8E%A5%E5%8F%A3%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)

Restful Demo:

- Flask：[Github](https://github.com/duicikeyihangaolou/rest_api_demo) 或
  [Gitee](https://gitee.com/wu-wen-xiang/rest_api_demo)
- Pecan：[Github](https://github.com/duicikeyihangaolou/restful-api-demo) 或
  [Gitee](https://gitee.com/wu-wen-xiang/restful-api-demo)
- Django：[Github](https://github.com/duicikeyihangaolou/Training-Django-Public/tree/master/09-RestAPI) 或
  [Gitee](https://gitee.com/wu-wen-xiang/training-django/tree/master/09-RestAPI)
- FastAPI：[Github](https://github.com/duicikeyihangaolou/fastapi-demo) 或
  [Gitee](https://gitee.com/wu-wen-xiang/fastapi-demo)

[FastAPI Demo](#132-restful-api) 用到了脚手架，是 Python
的，类似这样的生成代码脚手架（golang）还有：[blade](https://github.com/x893675/blade)，后面还会降到
[kubebuilder](#172-kubebuilder) 脚手架。

##### 1.3.2.1 blade demo

Install golang: <https://go.dev/doc/install>

```console
$ go version
go version go1.20.3 darwin/amd64
```

blade：<https://github.com/x893675/blade>

```bash
# 安装 blade 工具
curl -sfL https://oss.hanamichi.wiki/install.sh | bash
mv blade /usr/local/bin/

# 初始化项目，设置作者，仓库名，项目名
blade init --owner "duicikeyihangaolou" --repo github.com/duicikeyihangaolou/blade-test --project-name blade-test
```

```console
$ tree
.
├── Dockerfile
├── Makefile
├── PROJECT
├── README.md
├── bin
│   └── swag
├── config.yaml
├── docs
│   ├── docs.go
│   ├── swagger.json
│   └── swagger.yaml
├── go.mod
├── go.sum
├── hack
│   └── boilerplate.go.txt
├── main.go
└── pkg
    ├── config
    │   └── config.go
    ├── errdetails
    │   └── error.go
    ├── healthz
    │   └── healthz.go
    ├── logger
    │   ├── encode_type.go
    │   ├── logger.go
    │   └── options.go
    ├── server
    │   ├── filters
    │   │   ├── log.go
    │   │   └── skip.go
    │   ├── param
    │   │   └── common.go
    │   ├── server.go
    │   └── validate
    │       └── validate.go
    ├── signal
    │   ├── signal.go
    │   ├── signal_posix.go
    │   └── signal_windows.go
    ├── utils
    │   └── sets
    │       ├── sets.go
    │       └── string.go
    └── version
        └── version.go

17 directories, 30 files
```

```bash
# 添加 API, API 路由为 /auth/v1/token
blade create api --group auth --version v1 --kind Token
```

```diff
$ git diff pkg/server/server.go
diff --git a/pkg/server/server.go b/pkg/server/server.go
index 8f12e6c..241c979 100644
--- a/pkg/server/server.go
+++ b/pkg/server/server.go
@@ -32,6 +32,7 @@ import (
        "github.com/duicikeyihangaolou/blade-test/pkg/healthz"
        "github.com/duicikeyihangaolou/blade-test/pkg/logger"
        "github.com/duicikeyihangaolou/blade-test/pkg/server/filters"
+       authv1 "github.com/duicikeyihangaolou/blade-test/pkg/server/handler/auth/v1"
        "github.com/duicikeyihangaolou/blade-test/pkg/server/validate"
        //+blade:scaffold:imports
 )
@@ -99,6 +100,7 @@ func (s *Service) buildHandlerChain(ctx context.Context) error {
        // TODO(user): use can add custom health check here
        healthz.InstallHealthCheck(s.e)
 
+       authv1.RegisterRouter(s.e)
        //+blade:scaffold:routers
 
        if s.config.Debug {
```

```bash
$ git st
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   pkg/server/handler/auth/docs.go
	new file:   pkg/server/handler/auth/v1/handler.go
	new file:   pkg/server/handler/auth/v1/registry.go

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   PROJECT
	modified:   docs/docs.go
	modified:   docs/swagger.json
	modified:   docs/swagger.yaml
	modified:   pkg/server/server.go
```

```bash
# 添加另一组 API /user
blade create api --group user --version v1 --kind User
# 在同一 GROUP 下添加另一个资源对象
blade create api --group user --version v1 --kind Group
```

```bash
$ git st
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   PROJECT
	modified:   docs/docs.go
	modified:   docs/swagger.json
	modified:   docs/swagger.yaml
	modified:   pkg/server/handler/user/v1/handler.go
	modified:   pkg/server/handler/user/v1/registry.go
```

```diff
$ git diff pkg/server/handler/user/v1/registry.go
diff --git a/pkg/server/handler/user/v1/registry.go b/pkg/server/handler/user/v1/registry.go
index 45dee0f..bc25e32 100644
--- a/pkg/server/handler/user/v1/registry.go
+++ b/pkg/server/handler/user/v1/registry.go
@@ -32,6 +32,11 @@ func RegisterRouter(e *echo.Echo) {
        g.GET("/user/:id", h.DescribeUser)
        g.PUT("/user/:id", h.UpdateUser)
        g.DELETE("/user/:id", h.DeleteUser)
+       g.POST("/group", h.CreateGroup)
+       g.GET("/group", h.ListGroup)
+       g.GET("/group/:id", h.DescribeGroup)
+       g.PUT("/group/:id", h.UpdateGroup)
+       g.DELETE("/group/:id", h.DeleteGroup)
        //+blade:scaffold:registry
        // TODO(user): add user custom router here
 }

$ git diff pkg/server/handler/user/v1/handler.go
diff --git a/pkg/server/handler/user/v1/handler.go b/pkg/server/handler/user/v1/handler.go
index 3baad31..b7216d6 100644
--- a/pkg/server/handler/user/v1/handler.go
+++ b/pkg/server/handler/user/v1/handler.go
@@ -109,4 +109,84 @@ func (h *handle) DeleteUser(c echo.Context) error {
        return c.String(http.StatusOK, c.Path())
 }
 
+// CreateGroup Create Group
+//
+//     @Summary      Create Group
+//     @Description  Create Group
+//     @Tags         user
+//     @Accept       json
+//     @Produce      json
+//     @Success      200  {object}  param.PageableResponse{} "desc"
+//     @Failure      500  {object}  errdetails.BizError
+//     @Router       /user/v1/group [post]
+func (h *handle) CreateGroup(c echo.Context) error {
+       // TODO(user): custom logic here
+       return c.String(http.StatusOK, c.Path())
+}
+
...
```

如果 --orm，会带上 go-entity

```bash
blade create api --group user --version v1 --kind Group --orm
```

```console
$ git st
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   PROJECT
	modified:   docs/docs.go
	modified:   docs/swagger.json
	modified:   docs/swagger.yaml
	modified:   go.mod
	modified:   go.sum
	modified:   pkg/server/handler/user/v1/handler.go
	modified:   pkg/server/handler/user/v1/registry.go

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	pkg/ent/
	pkg/models/
	pkg/mtime/
```

然后可以直接运行

```console
$ make help

Usage:
  make <target>

General
  help             Display this help.

Build Dependencies
  swagger          Download swag locally if necessary. If wrong version is installed, it will be overwritten.

Development
  generate         Generate code containing Swagger, Ent etc.
  fmt              Run go fmt against code.
  vet              Run go vet against code.

Build
  build            Build binary.
  run              Run a server from your host.
  build-image      Build docker image with the blade-test.


$ make run
test -s /Users/wuwenxiang/local/test/bin/swag && /Users/wuwenxiang/local/test/bin/swag --version | grep -q v1.8.9 || \
	GOBIN=/Users/wuwenxiang/local/test/bin go install github.com/swaggo/swag/cmd/swag@v1.8.9
/Users/wuwenxiang/local/test/bin/swag init --parseDependency
2023/04/25 16:13:10 Generate swagger docs....
2023/04/25 16:13:10 Generate general API Info, search dir:./
2023/04/25 16:13:13 Generating param.PageableResponse
2023/04/25 16:13:13 Generating errdetails.BizError
2023/04/25 16:13:13 create docs.go at  docs/docs.go
2023/04/25 16:13:13 create swagger.json at  docs/swagger.json
2023/04/25 16:13:13 create swagger.yaml at  docs/swagger.yaml
go generate ./pkg/...
go fmt ./...
go vet ./...
go run ./main.go
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:94	please visit 0.0.0.0:8080/swagger/index.html to get swagger docs
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "GET", "path": "/user/v1/group"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "PUT", "path": "/auth/v1/token/:id"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "DELETE", "path": "/auth/v1/token/:id"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "GET", "path": "/auth/v1/token"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "GET", "path": "/auth/v1/token/:id"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "GET", "path": "/user/v1/user/:id"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "PUT", "path": "/user/v1/user/:id"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "POST", "path": "/user/v1/group"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "GET", "path": "/swagger/*"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "GET", "path": "/healthz"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "GET", "path": "/user/v1/group/:id"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "DELETE", "path": "/user/v1/group/:id"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "POST", "path": "/user/v1/user"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "DELETE", "path": "/user/v1/user/:id"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "PUT", "path": "/user/v1/group/:id"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "POST", "path": "/auth/v1/token"}
2023-04-25T16:13:20+08:00	DEBUG	server/server.go:110	API Router	{"method": "GET", "path": "/user/v1/user"}
⇨ http server started on [::]:8080
```

#### 1.3.3 API 测试

参考自动化测试：[Github](https://github.com/duicikeyihangaolou/training-python-public/blob/master/doc/autotest.md)
或 [Gitee](https://gitee.com/wu-wen-xiang/training-python/blob/master/doc/autotest.md)

- 单元测试：unittest / pytest
- 接口测试：requests & json / gabbi / tarven

### 1.4 有状态服务

#### 1.4.1 RDS 服务

[返回目录](#课程目录)

公有云和私有云都有 RDS，参考：

- 私有云：OpenStack 的 Trove
- 公有云：[阿里云 RDS 服务](https://rdsnext.console.aliyun.com/dashboard/)

#### 1.4.2 K8S 怎么处理有状态服务

参考
[Github](https://github.com/99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#84-k8s-%E6%80%8E%E4%B9%88%E5%A4%84%E7%90%86%E6%9C%89%E7%8A%B6%E6%80%81%E6%9C%8D%E5%8A%A1)
或
[Gitee](https://gitee.com/dev-99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#84-k8s-%E6%80%8E%E4%B9%88%E5%A4%84%E7%90%86%E6%9C%89%E7%8A%B6%E6%80%81%E6%9C%8D%E5%8A%A1)

### 1.5 WebSocket

[返回目录](#课程目录)

#### 1.5.1 应用服务提供 WebSocket

参考 FastAPI：[WebSocket](https://fastapi.tiangolo.com/zh/advanced/websockets/)

WebSocket 是有状态的长连接

1. 一旦应用关闭，所有的 WebSocket 都会断开
2. WebSocket 的横向扩展会非常麻烦

```text
web socket 长连接

        ┌────────────┐  ┌─────────────┐
        │   Server   │  │    Server   │
        └─┬─────┬────┘  └┬─────┬─────┬┘
          │     │        │     │     │
        ┌─┴─┐ ┌─┴─┐    ┌─┴─┐ ┌─┴─┐ ┌─┴─┐
        │   │ │   │    │   │ │   │ │   │
        │   │ │   │    │   │ │   │ │   │
        └───┘ └───┘    └───┘ └───┘ └───┘
```

#### 1.5.2 消息队列直接提供 WebSocket

解决上述问题的方法是增加中间件：消息队列

```text
用消息队列提供 web socket 服务

      ┌────────────┐  ┌────────────┐  ┌─────────────┐
      │  Server    │  │   Server   │  │   Server    │
      └─────┬──────┘  └──────┬─────┘  └──────┬──────┘
            │                │               │
───────────┬┴────┬─────┬─────┼─────┬─────┬───┴─┬─────────  Message Queue
           │     │     │     │     │     │     │
         ┌─┴─┐ ┌─┴─┐ ┌─┴─┐ ┌─┴─┐ ┌─┴─┐ ┌─┴─┐ ┌─┴─┐
         │   │ │   │ │   │ │   │ │   │ │   │ │   │
         │   │ │   │ │   │ │   │ │   │ │   │ │   │
         └───┘ └───┘ └───┘ └───┘ └───┘ └───┘ └───┘
```

参考 OpenV2X Roadmocker，[Github](https://github.com/open-v2x/roadmocker) 或
[Gitee](https://gitee.com/open-v2x/roadmocker)

### 1.6 视频流

视频流同样是长链接，也需要考虑云原生：无状态 + 可扩展

参考：[hippocampus](https://gitee.com/open-v2x/hippocampus)，[Online Demo](http://47.100.126.13:2288/traffic/map?type=1)

### 1.7 基于 K8S API 的云原生

#### 1.7.1 控制器

Kubernetes 的核心就是控制理论，Kubernetes 控制器中实现的控制回路是一种闭环反馈控制系统。

该类型的控制系统基于反馈回路将目标系统的当前状态与预定义的期望状态相比较，二者之间的差异作为误差信号产生一个控制输出作为控制器的输入，以减少或消除目标系统当前状态与期望状态的误差。这种控制循环在
Kubernetes 上也称为**调谐循环**。

![](/image/kubernetes-co-loop.webp)

对 Kubernetes 来说，无论控制器的具体实现有多么简单或多么复杂，它基本都是通过定期重复执行如下3个步骤来完成控制任务。

1. 从 API Server 读取资源对象的期望状态和当前状态。
2. 比较二者的差异，而后运行控制器中的必要代码操作现实中的资源对象，将资源对象的真实状态修正为Spec中定义的期望状态，例如创建或删除 Pod 对象，以及发起一个云服务 API 请求等。
3. 变动操作执行成功后，将结果状态存储在 API Server 上的目标资源对象的 status 字段中。

![](/image/kubernetes-controller-loop.webp)

**水平触发**：任务繁重的 Kubernetes 集群上同时运行着数量巨大的控制循环，每个循环都有一组特定的任务要处理，为了避免 API Server
被请求淹没，需设定控制回路以较低的频率运行，分钟级别，比如每 5 分钟一次。

**边缘触发**：同时，为了能及时触发由客户端提交的期望状态的更改，控制器向 API Server 注册监视受控资源对象，这些资源对象期望状态的任何变动都会由 Informer
组件通知给控制器立即执行而无须等到下一轮的控制循环。控制器使用工作队列将需要运行的控制循环进行排队，从而确保在受控对象众多或资源对象变动频繁的场景中尽量少地错过控制任务。

![](/image/kubernetes-controller-trigger.png)

出于简化管理的目的，Kubernetes 将数十种内置的控制器程序整合成了名为 kube-controller-manager
的单个应用程序，并运行为独立的单体守护进程，它是控制平面的重要组件，也是整个 Kubernetes 集群的控制中心。

参考代码：[replica_set.go](https://github.com/kubernetes/kubernetes/blob/master/pkg/controller/replicaset/replica_set.go#L553)

可以看到：

1. 控制器会比较指定的副本数量与当前正在运行的副本数量
2. diff < 0，副本数不够，创建一些
3. diff > 0，副本数太多，删除一些
4. getPodsToDelete 中，选择了影响面较小的 pod 进行删除
5. 扩展一下，资源不一定是 Kubernetes 的一部分，比如 s3 bucket

#### 1.7.2 kubebuilder

参考 kubebuilder：[Github](https://github.com/kubernetes-sigs/kubebuilder) 或
[Guide](https://cloudnative.to/kubebuilder/quick-start.html)

```bash
# 域外机器 2C/4G/60G Ubuntu 20.04，或 export GOPROXY=https://goproxy.cn
# 配置 SSH

# 安装 KubeClipper
# https://github.com/kubeclipper/kubeclipper
curl -sfL https://oss.kubeclipper.io/get-kubeclipper.sh | KC_REGION=cn bash -
kcctl deploy

# docker login

# 安装 kustomize
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash
mv kustomize /usr/bin/
kustomize version

# 安装 golang
wget https://go.dev/dl/go1.20.3.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.3.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
vi /etc/environment 
go version

# 安装 kubebuilder
curl -L -o kubebuilder https://go.kubebuilder.io/dl/latest/$(go env GOOS)/$(go env GOARCH)
chmod +x kubebuilder && mv kubebuilder /usr/local/bin/

# 创建 operator
mkdir -p ~/projects/guestbook
cd ~/projects/guestbook
kubebuilder init --domain my.domain --repo my.domain/guestbook
kubebuilder create api --group webapp --version v1 --kind Guestbook
make manifests

# 本地运行
make install
make run # 可能会端口被占用
# netstat -putln | grep 8080

# CRD 部署到 cluster
kubectl apply -f config/samples/
make docker-build docker-push IMG=99cloud/test-operator
make deploy IMG=99cloud/test-operator

# 检查 CRD
kc get crd
kubectl get crd | grep book
kubectl get guestbooks.webapp.my.domain 
kubectl get guestbooks.webapp.my.domain guestbook-sample -o yaml
```

更多参考：[programming-kubernetes](https://github.com/programming-kubernetes)

## 2. 微服务

[返回目录](#课程目录)

### 2.1 微服务应该有多微

[返回目录](#课程目录)

这是一个 Tradeoff，没有固定答案，但

1. 一个微服务应该只做一件事
2. 它是独立的，很容易改动和升级，改动它不会影响到其它服务

### 2.2 服务注册和服务发现

[返回目录](#课程目录)

#### 2.2.1 服务发现在 ETSI 标准中的应用

![](/image/etsi-arch.png)

![](/image/service-discover-arch.png)

#### 2.2.2 API Gateway vs Middleware

![](/image/apigateway-servicediscover.png)

基于 Nginx 的 API Gateway
应用：[Skyline](https://opendev.org/openstack/skyline-apiserver/src/branch/master/skyline_apiserver/templates/nginx.conf.j2)

基于 [OpenResty](https://openresty.org/cn/) 的 Kong

![](/image/microservice-apigateway-kong.png)

Kong QuickStart，参考：

- [Getting started Kong API Gateway](https://www.popularowl.com/api-first/kong-api-gateway-getting-started/)
- [Kong 3.0.x Get Started](https://docs.konghq.com/gateway/3.0.x/get-started/)

![](/image/kong-concepts.png)

![](/image/apigateway-workflow.png)

![](/image/apigateway-arch.png)

另一种解决思路：Middleware，比如 [OpenStack Keystone](https://opendev.org/openstack/keystonemiddleware)

- [Middleware Architecture](https://docs.openstack.org/keystonemiddleware/latest/middlewarearchitecture.html)
- [Audit middleware](https://docs.openstack.org/keystonemiddleware/latest/audit.html)

Keystone
怎么处理服务注册和服务发现？参考：[Github](https://github.com/99cloud/lab-openstack/blob/master/doc/class-01-OpenStack-Administration.md#33-keystone-%E5%90%84%E5%8A%9F%E8%83%BD%E7%9A%84%E5%AE%9E%E7%8E%B0%E6%9C%BA%E7%90%86%E6%98%AF%E6%80%8E%E6%A0%B7%E7%9A%84)
或
[Gitee](https://gitee.com/dev-99cloud/lab-openstack/blob/master/doc/class-01-OpenStack-Administration.md#33-keystone-%E5%90%84%E5%8A%9F%E8%83%BD%E7%9A%84%E5%AE%9E%E7%8E%B0%E6%9C%BA%E7%90%86%E6%98%AF%E6%80%8E%E6%A0%B7%E7%9A%84)

#### 2.2.3 K8S 服务发现

K8S：

1. 通过 Service
   实现服务注册和服务发现：[Github](https://github.com/99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#37-daemonset--statefulset)
   或
   [Gitee](https://gitee.com/dev-99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#37-daemonset--statefulset)
2. 有状态应用和无头服务：[Github](https://gitee.com/dev-99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#72-%E5%AE%9E%E9%AA%8C%E5%8F%91%E5%B8%83%E6%9C%8D%E5%8A%A1)
   或
   [Gitee](https://gitee.com/dev-99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#72-%E5%AE%9E%E9%AA%8C%E5%8F%91%E5%B8%83%E6%9C%8D%E5%8A%A1)
3. 通过 Ingress 实现 API
   Gateway：[Github](https://github.com/99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#73-%E4%BB%80%E4%B9%88%E6%98%AF-ingress)
   或
   [Gitee](https://gitee.com/dev-99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#73-%E4%BB%80%E4%B9%88%E6%98%AF-ingress)

   ![](/image/ingress-service.drawio.svg)

   - 集群内部客户端，理论上也能走 Ingress，将其当作 API Gateway 来用，但比较麻烦，因为 Ingress 是用域名和路由来区分应用，需要域名解析。
   - Ingress 只能做到 API Gateway 中的服务汇聚功能，多个服务端点汇聚成一个服务端点，然后在这个服务端点中通过 url 路由来区分不同的子服务。实际应用中，API
     Gateway 还有统一认证、鉴权、限流、计量、分析等业务无关的通用能力。所以实际应用中一般会有单独的 API Gateway 存在，比如 Kong，或者服务提供商自己提供一个 API
     Gateway 供自己的服务使用。
   - Ingress 到 Pod，声明时会经过 Service 关联，但实际实现时不一定经过 Service，Ingress 可能会直接转发到 Pod。

4. 通过 ConfigMap 和 Secret
   实现配置管理：[Github](https://github.com/99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#61-%E4%BB%80%E4%B9%88%E6%98%AF-configmap--secret)
   或
   [Gitee](https://gitee.com/dev-99cloud/training-kubernetes/blob/master/doc/class-01-Kubernetes-Administration.md#61-%E4%BB%80%E4%B9%88%E6%98%AF-configmap--secret)
5. 通过 [Job](https://kubernetes.io/zh-cn/docs/concepts/workloads/controllers/job/) 和
   [CronJob](https://kubernetes.io/zh-cn/docs/concepts/workloads/controllers/cron-jobs/) 实现一次性后台任务

### 2.3 微前端

[返回目录](#课程目录)

qiankun：<https://qiankun.umijs.org/zh>

Demo：<https://github.com/moweiwei/react-app-qiankun/>

### 2.4 云安全

[返回目录](#课程目录)

#### 2.4.1 认证

认证方式：

1. HTTP Basic
2. Cookie / JWT Token
3. Windows 身份认证（NTLM and Kerberos），基于 AD
4. SAML（ADFS / AD Azure / Shibboleth 等），安全断言标记语言是一种 bearer token 技术，其中包含用于授权 SAML token
   的身份信息提供者和验证他们的应用程序
5. OAuth/OpenID

OAuth 的优势

1. 操作系统无关
2. 允许 Web 上的不同系统中共享授权声明
3. 简单易用
4. 坚实可靠（但 Keystone 打死不对接 OAuth2）
5. 提供广泛的语言和类库实现
6. 提供免费、开源的身份信息提供商服务器软件（Github 等）
7. 有权使用基于云的身份信息提供商（Google、Auth0、StormPath）

Keystone Token

参考：[Github](https://github.com/99cloud/lab-openstack/blob/master/doc/class-01-OpenStack-Administration.md#32-keystone-%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%E6%9C%89%E5%93%AA%E4%BA%9B)
或
[Gitee](https://gitee.com/dev-99cloud/lab-openstack/blob/master/doc/class-01-OpenStack-Administration.md#32-keystone-%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%E6%9C%89%E5%93%AA%E4%BA%9B)

### 2.5 事件溯源和 CQRS

[返回目录](#课程目录)

事件溯源：`f(status_A, event_1) = status_B`

1. 幂等
2. 可测试

原则：拥抱最终一致性

CQRS（命令查询责任分离），比如：SQL 读写分离

## 3. istio

### 3.1 微服务框架

#### 3.1.1 微服务框架的发展历史

1. 拆解单体服务，自建微服务框架
2. 使用 SpringCloud / Dubbo 等侵入式微服务框架
3. 使用服务网格，Sidecar 服务框架

#### 3.1.2 不同微服务框架的适用场景

Istio 解决了 SpringCloud 等同行的如下遗留问题：

1. 多语言技术栈不统一：C++、Java、PHP、Go。Spring Cloud 无法提出非 Java 语言的微服务治理。
2. 服务治理周期长：微服务治理框架与业务耦合，上线周期长，策略调整周期长。
3. 产品能力弱：Spring Cloud 缺乏平台化和产品化的能力，可视化能力弱。

企业什么情况适合使用 SpringCloud？

1. 企业的开源语言主要是 Java
2. 治理策略变更不频繁（没有动态服务治理强需求），通过上线可以解决问题
3. 更新升级不频繁（微服务框架升级与业务升级不相互耦合）
4. 无过多高级治理功能需求（不需要混沌调试，智能调参，容器间网络可编程）
5. 业务规模不是非常大（核心业务不一定是微服务框架）

反之，应该选择 K8S 或 K8S + istio。

**选择 Istio 一般认为有 3ms 量级的额外延迟**，因此，对网络性能，响应速度敏感的场景要慎重引入 Istio。

#### 3.1.3 服务框架的对比

| 功能列表            | 描述                                  | SpringCloud                                    | Istio                                                         | K8S + SpringCloud              |
| --------------- | ----------------------------------- | ---------------------------------------------- | ------------------------------------------------------------- | ------------------------------ |
| 服务注册与发现         | 部署应用时，自动进行服务的注册，其他调用方可以即时获取新服务的注册信息 | 支持，基于Eureka、Consul等组件实现，提 供 Server 和 Client 管理 | K8S Service                                                   | K8S Service                    |
| 配置中心            | 管理微服务的配置                            | Spring Cloud Config 组件                         | ConfigMap                                                     | ConfigMap                      |
| 租户隔离            | 基于租户隔离微服务                           | N/A                                            | Namespace 隔离                                                  | NameSpace 或基于其上的封装             |
| 微服务间路由管理        | 微服务之间相互访问的管理                        | 基于网关 Zuul 实现， 需要代码级别配置                         | 基于声明配置文件，最终转化成路由规则实现，Istio Virtual Service 和 Destination Rule | 基于 Camel 实现                    |
| 负载均衡            | 客户端发起请求在微服务端的负载均衡                   | Ribbon 或 Feigin                                | Envoy，基于声明配置文件，最终转化成路由规则实现                                    | Service 负载均衡（一般 Kubeproxy 实现 ） |
| 应用日志收集          | 收集微服务的日志                            | 支持，提供 Client 对接第三方日志系统，例如 ELK                  | EFK                                                           | EFK                            |
| 对外访问 API 网关     | 为所有客户端请求的入口                         | 基于 Zuul 或者spring-cloud-gateway 实现              | 基于 Ingress/Egress网关实现入口和出口的管理                                 | 基于 Camel 实现                    |
| 微服务调用链路追踪       | 可以生成微服务之间调用的拓扑关系图                   | 基于 Zipkin 实现                                   | 基于 Istio 自带的 Jaeger 实现，并通过 Kiali 展示                           | 基于 Zipkin 或 Jaeger 实现          |
| 无源码修改方式的应用迁移    | 将应用迁移到微服务架构时不修改应用源代 码               | 不支持                                            | 部署的容器化应用的时候进行 Sidecar 注入                                      | 不支持                            |
| 灰度、蓝绿发布         | 实现应用版本的动态切换                         | 需要修改代码实现                                       | Envoy实现，基于 声明配置文件，最终转化成路由规则实现                                 | Ingress 或者 Router              |
| 灰度上线            | 允许上线实时流星的副本，客户无感知                   | 不支持                                            | Envoy，基于声明配置文件，最终转化成路由规则实现                                    | 不支持                            |
| 安全策略            | 实现微服务访问控制的 RBAC，对于微服务入口流量可设省加密访问    | 支持，基于 Spring Security 组件实现，包括认证，鉴权等，支持通信加密     | Istio 的认证和授权                                                  | RBAC + 加密 Router（Ingress）      |
| 性能监控            | 监控微服务的实施性能                          | 支持，基于 Spring Cloud 提供的监控组件收集数据，对接第三方的监控数据存储    | Prometheus + Grafana                                          | Prometheus + Grafana           |
| 支持故障注入          | 模拟微服务的故障，增加可用性                      | 不支持                                            | 支持退出和延迟两类故障注入                                                 | 不支持                            |
| 支持服务间调用限流、熔断    | 避免微服务出现雪崩效应                         | 基于 Hystrix 实现，需要代码注释                           | Envoy，基于声明配置文件，最终转化成路由规则实现                                    | 基于 Hystrix 实现，需要代码注释           |
| 实现微服务见的访问控制黑白名单 | 灵活设置微服务之间的相互访问策略                    | 需要代码注释                                         | Envoy，基于声明配置文件，最终转化成路由规则实现                                    | CNI Network Policy             |
| 支持服务问路由控制       | 灵活设置微服务之间的相互访问策略                    | 需要代码注释                                         | Envoy，基于声明配置文件，最终转化成路由规则实现                                    | 需要代码注释                         |
| 支持对外部应用的访问      | 微服务内的应用可以访问微服务体系外的应用                | 需要代码注释                                         | ServiceEntry                                                  | Service Endpoint               |
| 支持链路访问数据可视化     | 实时追踪微服务之间访问的链路，包括流量、成功率等            | 不支持                                            | 基于 Istio 自带的 Kiali 实现                                         | 不支持                            |

#### 3.1.4 微服务化的实施步骤

1. 公共服务中台化

   用户中心，认证中心独立为中台，能力服用，数据统一

2. 数据库表设计可横向扩展（多增加几台服务器，一起服务）

   关系型数据库，用主从复制、集群和分片，减少数据耦合，联合查询，存储过程

3. 基础设施容器化

   基于虚拟机和物理机平台部署容器平台

4. CI/CD

   基于容器的持续集成和快速迭代

5. 服务发现

   服务之间相互调用

6. 业务服务改造

   用户和认证调用中台

7. 中间件 PaaS 化

   RabbitMQ，Redis，独立的团队进行维护，PaaS 会自维护自修复

8. 服务治理

   高并发下的服务可用性保证

9. 性能管理

   高并发下的服务的性能瓶颈发现

10. 界面统一团队框架

    成立统一的前端开发组，维护统一的前端框架，统一风格，减少 Bug

参考 [网易微服务方案](https://sf.163.com/product-nsf?tag=M_csdn_88820525)

#### 3.1.5 Service Mesh 简介

Service Mesh 是什么：

- 本质：基础设施层
- 功能：请求分发
- 部署形式：网络代理
- 特点：透明

![](/image/istio-arch.png)

控制平面：

- 不直接解析数据包
- 与控制平面中的代理通信、下发策略和配置
- 负责网络行为的可视化
- 通常提供 API 或者命令行工具，可用于配置版本化管理，便于 CI/CD

数据平台：

- 通常按照无状态目标设计，但实际上为提高转发效率，需要缓存一些数据
- 直接处理入站和出站数据包，如转发、路由、健康检查、负载均衡、认证、鉴权、监控数据等
- 对应用来说透明，可以做到无感知部署（如果要实现方法级别调用的链路追踪，需要通过清理级的 SDK 来实现 TraceID 的传递，因此此时对代码层面来说，不是完全零侵入）

Kubeproxy vs Service Mesh SideCar Proxy

1. KubeProxy 拦截的是进出 K8S 节点的流量，Sidecar Proxy 拦截的是进出该 Pod
   的流量。优点是能进行细粒度的流量管理，缺点是必须添加一系列新的抽象，导致学习成本上升，以及导致大量配置分发、同步和最终一致性问题。
2. KubeProxy 通过 Deployment 做金丝雀发布，该方法本质上是通过修改 Pod 的 label，将不同 Deployment 的 Pod 划归到同一个
   Service。Sidecar Proxy 通过流量治理实现金丝雀发布。

   ```text
   ┌───────────────┐  Label-A ┌─────────┐
   │ Deployment A  ├──────────┤  Pod A  ├─────┐
   └───────────────┘          └─────────┘     │ Label-C ┌───────────┐
                                              ├─────────┤ Service C │
                                              │         └───────────┘
   ┌───────────────┐  Label-B ┌─────────┐     │
   │ Deployment B  ├──────────┤  Pod B  ├─────┘
   └───────────────┘          └─────────┘
   ```

#### 3.1.6 Istio

参考：<https://istio.io/v1.11/zh/docs/concepts/what-is-istio/>，Istio 功能：

- 流量管理
  - 请求路由和流量转移
  - 弹性功能：熔断、超时、重试
  - 调试功能：故障注入和流量镜像
- 策略控制：通过 Mixer 组建和各种适配器实现访问控制系统、遥测捕获、配额管理和计费等策略
- 可观测性
- 安全认证：Citadel 组件做密钥和证书管理

参考：[流量管理 CRD](https://istio.io/v1.11/zh/docs/concepts/traffic-management/)

- Istio Gateway
- Virtual Service：将 K8S 服务连接到 Istio Gateway，并且可以定义一组流量规则，以便在主机寻址时应用
- DestinationRule：定义流量如何路由（LB 配置、连接池 Size、外部检测配置等）
- EnvoyFilter：代理服务过滤器，一般使用中用不到
- ServiceEntry：在 Istio 内部的服务注册表中加入额外条目，让服务网格中的服务能够访问和路由到额外注入的条目

### 3.2 环境搭建

[返回目录](#课程目录)

#### 3.2.1 硬件环境

- 硬件：4Core / 8G / 40G
- 操作系统：Ubuntu 20.04 / CentOS 7.9
- 机器需要能从 github 上下载 release 包（istio 安装需要）

#### 3.2.2 安装 K8S 1.23.6

安装 [KubeClipper](kubernetes-best-practices.md#314-kubeclipper) v1.3.2

```bash
curl -sfL https://oss.kubeclipper.io/kcctl.sh | KC_REGION=cn KC_VERSION=v1.3.2 bash -

kcctl deploy
```

然后通过浏览器访问服务器（注意，如果从命令行起集群，默认 CRI 是 containerd），`http://<Server-IP>`，`admin` / `Thinkbig1`，创建集群：

- 去掉 Master 污点，方便调度业务 Pod
- 选 Docker，方便使用 Docker 命令
- 默认会选 ipvs，也可以选 iptables（方便使用 iptables 查看 nat 记录）

其它保持默认，创建 AIO 集群。

#### 3.2.3 安装 istio 1.11.2 版本

参考：<https://istio.io/latest/zh/docs/setup/getting-started/>

1.11 版本可以参考：<https://istio.io/v1.11/zh/docs/setup/getting-started/>

```bash
# curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.11.2 TARGET_ARCH=x86_64 sh -
curl -L https://kubeclipper.oss-ap-southeast-1.aliyuncs.com/tools/downloadIstio | ISTIO_VERSION=1.11.2 TARGET_ARCH=x86_64 sh -

cd istio-1.11.2
echo "export PATH=$PWD/bin:\$PATH" >> ~/.bashrc
. ~/.bashrc

#  demo 配置组合包含了一组专为测试准备的功能集合，另外还有用于生产或性能测试的配置组合。
istioctl install --set profile=demo -y

# 给命名空间添加标签，指示 Istio 在部署应用的时候，自动注入 Envoy 边车代理：
kubectl label namespace default istio-injection=enabled
```

```console
root@meshlab:~# istioctl install --set profile=demo -y
✔ Istio core installed
✔ Istiod installed
✔ Egress gateways installed
✔ Ingress gateways installed
✔ Installation complete
Thank you for installing Istio 1.11.  Please take a few minutes to tell us about your install/upgrade experience!  https://forms.gle/kWULBRjUv7hHci7T6

root@meshlab:~# kubectl label namespace default istio-injection=enabled
namespace/default labeled

root@meshlab:~# kubectl get svc -n istio-system
NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                                                                      AGE
istio-egressgateway    ClusterIP      172.25.0.254   <none>        80/TCP,443/TCP                                                               8m2s
istio-ingressgateway   LoadBalancer   172.25.0.190   <pending>     15021:32094/TCP,80:31204/TCP,443:30721/TCP,31400:30724/TCP,15443:31264/TCP   8m2s
istiod                 ClusterIP      172.25.0.19    <none>        15010/TCP,15012/TCP,443/TCP,15014/TCP                                        8m14s

root@meshlab:~# kubectl get po -n istio-system
NAME                                    READY   STATUS    RESTARTS   AGE
istio-egressgateway-7fdbfcc9f-9r8qc     1/1     Running   0          11m
istio-ingressgateway-69944847bc-kwh2r   1/1     Running   0          11m
istiod-5ff694cc8-lx6c7                  1/1     Running   0          12m
```

#### 3.2.4 bookinfo demo

参考：[bookinfo 应用](https://istio.io/latest/zh/docs/examples/bookinfo/)

v1.11 版本：<https://istio.io/v1.11/zh/docs/examples/bookinfo/>

bookinfo 应用是 istio 的官方推荐学习用例。

bookinfo 可以在无服务网格环境直接部署

```bash
# wget https://raw.githubusercontent.com/istio/istio/release-1.11/samples/bookinfo/platform/kube/bookinfo.yaml
wget https://kubeclipper.oss-ap-southeast-1.aliyuncs.com/tools/bookinfo.yaml

kubectl delete ns bookinfo
kubectl create ns bookinfo

kubectl -n bookinfo apply -f bookinfo.yaml
kubectl get po -n bookinfo
```

确认上面的操作都正确之后，运行下面命令，通过检查返回的页面标题，来验证应用是否已在集群中运行，并已提供网页服务：

```console
root@meshlab:~# kubectl exec "$(kubectl get pod -n bookinfo -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -n bookinfo -c ratings -- curl -s productpage:9080/productpage | grep -o "<title>.*</title>"
<title>Simple Bookstore App</title>
```

配置 NodePort，令外部可以访问到上述 `/productpage` 页面

`kubectl edit svc productpage -n bookinfo`，然后查找 type: ClusterIP，改成 type: NodePort

```console
[root@meshlab ~]# kubectl get svc -n bookinfo
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
productpage   ClusterIP   10.104.42.127   <none>        9080/TCP   167m

[root@meshlab ~]# kubectl edit svc productpage -n bookinfo
service/productpage edited

[root@meshlab ~]# kubectl get svc -n bookinfo
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
productpage   NodePort    10.104.42.127   <none>        9080:32373/TCP   168m
```

可以看到随机分配的 NodePort 端口是 32373，每次可能不同，可以从如下命令取得访问路径：

```console
[root@meshlab ~]# export PRODUCTPAGE_PORT=$(kubectl get svc productpage -n bookinfo -o jsonpath='{.spec.ports[?(@.name=="http")].nodePort}')

[root@meshlab ~]# echo "http://<external-ip>:${PRODUCTPAGE_PORT}"
http://<external-ip>:32373
```

这样从外部即可访问 book info 页面。

每次访问 `http://<external-ip>:32373/productpage` 都会呈现不同的效果，有时书评的输出包含星级评分，有时则不包含。这是因为 review 服务采用了 K8S
原生的金丝雀发布，同时运行 3 个版本（Service 根据每个版本 pod 的数量作为权重进行流量分发）。

##### 3.2.4.1 部署 bookinfo 应用到服务网格

```console
root@meshlab:~# kubectl apply -f bookinfo.yaml
service/details created
serviceaccount/bookinfo-details created
deployment.apps/details-v1 created
service/ratings created
serviceaccount/bookinfo-ratings created
deployment.apps/ratings-v1 created
service/reviews created
serviceaccount/bookinfo-reviews created
deployment.apps/reviews-v1 created
deployment.apps/reviews-v2 created
deployment.apps/reviews-v3 created
service/productpage created
serviceaccount/bookinfo-productpage created
deployment.apps/productpage-v1 created
```

应用很快会启动起来。当每个 Pod 准备就绪时，Istio 边车代理将伴随它们一起部署。

```console
root@meshlab:~# kubectl get services
NAME          TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
details       ClusterIP   172.25.0.143   <none>        9080/TCP   6m7s
kubernetes    ClusterIP   172.25.0.1     <none>        443/TCP    57m
productpage   ClusterIP   172.25.0.61    <none>        9080/TCP   6m7s
ratings       ClusterIP   172.25.0.214   <none>        9080/TCP   6m7s
reviews       ClusterIP   172.25.0.228   <none>        9080/TCP   6m7s

root@meshlab:~# kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
details-v1-5498c86cf5-6jp4x       2/2     Running   0          6m10s
productpage-v1-65b75f6885-6nhqb   2/2     Running   0          6m10s
ratings-v1-b477cf6cf-5w27x        2/2     Running   0          6m10s
reviews-v1-79d546878f-xqk5k       2/2     Running   0          6m10s
reviews-v2-548c57f459-qhtgt       2/2     Running   0          6m10s
reviews-v3-6dd79655b9-jxx8t       2/2     Running   0          6m10s
```

同样可以修改 productpage 服务的类型为 NodePort 来访问它。

##### 3.2.4.2 （可选）配置 istio ingress gateway

此时，BookInfo 应用已经部署，但还不能被外界访问。 要开放访问，您需要创建 Istio 入站网关（Ingress Gateway）, 它会在网格边缘把一个路径映射到路由。

```bash
wget https://raw.githubusercontent.com/istio/istio/release-1.11/samples/bookinfo/networking/bookinfo-gateway.yaml
```

```yaml
# cat bookinfo-gateway.yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: bookinfo-gateway
spec:
  selector:
    istio: ingressgateway # use istio default controller
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "*"
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: bookinfo
spec:
  hosts:
  - "*"
  gateways:
  - bookinfo-gateway
  http:
  - match:
    - uri:
        exact: /productpage
    - uri:
        prefix: /static
    - uri:
        exact: /login
    - uri:
        exact: /logout
    - uri:
        prefix: /api/v1/products
    route:
    - destination:
        host: productpage
        port:
          number: 9080
```

```console
# kubectl apply -f bookinfo-gateway.yaml
gateway.networking.istio.io/bookinfo-gateway created
virtualservice.networking.istio.io/bookinfo created

# istioctl analyze
✔ No validation issues found when analyzing namespace: default.
```

```bash
# 将 istio ingress gateway 的 spec 下，添加
# externalIPs:
# - 47.243.226.54
# 这里 47.243.226.54 是外网 IP
kubectl edit svc istio-ingressgateway -n istio-system

# 配置环境变量
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
# export INGRESS_HOST=$(kubectl get po -l istio=ingressgateway -n istio-system -o jsonpath='{.items[0].status.hostIP}')
export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.externalIPs[0]}')
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT

echo "http://$GATEWAY_URL/productpage"
# http://47.243.142.173:32237/productpage
```

上面的 `GATEWAY_URL` 可能是内网地址，需要改成公网地址（Floating IP）才能被外部用户访问。

### 3.3 流量监控和治理

[返回目录](#课程目录)

```bash
wget https://github.com/istio/istio/releases/download/1.11.2/istio-1.11.2-linux-amd64.tar.gz

tar zxvf istio-1.11.2-linux-amd64.tar.gz
cd istio-1.11.2
kubectl apply -f samples/addons
kubectl rollout status deployment/kiali -n istio-system

# 如果在安装插件时出错，再运行一次命令。 有一些和时间相关的问题，再运行就能解决。
```

如果没有配置 kiali 到 istio-ingress-gateway（目前没有配置），那么 `kubectl edit svc kiali -n istio-system`，改成
nodeport，也可也访问 kiali 页面

接下来可以参考：<https://istio.io/v1.11/zh/docs/setup/getting-started/#%E5%90%8E%E7%BB%AD%E6%AD%A5%E9%AA%A4>
完成后续实验

#### 3.3.1 应用默认目标规则

```bash
kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml

# 等待几秒钟，以使目标规则生效
# 您可以使用以下命令查看目标规则

kubectl get destinationrules -o yaml
```

#### 3.3.2 配置请求路由

仅路由到一个版本，请应用为微服务设置默认版本的 Virtual Service。在这种情况下，Virtual Service 将所有流量路由到每个微服务的 v1 版本。

```bash
kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml

# 使用以下命令显示已定义的路由：
kubectl get virtualservices -o yaml

# 您还可以使用以下命令显示相应的 subset 定义：
kubectl get destinationrules -o yaml
```

您已将 Istio 配置为路由到 Bookinfo 微服务的 v1 版本，最重要的是 reviews 服务的版本 1。

现在，无论您刷新多少次，页面的评论部分都不会显示评级星标。这是因为您将 Istio 配置为将评论服务的所有流量路由到版本 reviews:v1，而此版本的服务不访问星级评分服务。

#### 3.3.3 基于用户身份的路由

接下来，您将更改路由配置，以便将来自特定用户的所有流量路由到特定服务版本。在这种情况下，来自名为 jason 的用户的所有流量将被路由到服务 reviews:v2。

请注意，Istio 对用户身份没有任何特殊的内置机制。事实上，productpage 服务在所有到 reviews 服务的 HTTP 请求中都增加了一个自定义的 end-user
请求头，从而达到了本例子的效果。

请记住，reviews:v2 是包含星级评分功能的版本。

```bash
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml

# 删除应用程序的 Virtual Service
# kubectl delete -f samples/bookinfo/networking/virtual-service-all-v1.yaml
```

#### 3.3.4 故障注入

<https://istio.io/v1.11/zh/docs/tasks/traffic-management/fault-injection/>

```bash
# 注入 7s 延迟
kubectl apply -f samples/bookinfo/networking/virtual-service-ratings-test-delay.yaml
```

#### 3.3.5 流量转移

<https://istio.io/v1.11/zh/docs/tasks/traffic-management/traffic-shifting/>

```bash
# 100% v1
kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml

# 50% v1 + 50% v3
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-50-v3.yaml

# 100% v3
kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-v3.yaml
```

#### 3.3.6 通过 Prometheus 查询度量指标

<https://istio.io/v1.11/zh/docs/tasks/observability/metrics/querying-metrics/>

```bash
# 通过 NodePort 暴露 prometheus
kubectl -n istio-system edit svc prometheus
```

#### 3.3.7 使用 Grafana 可视化指标

<https://istio.io/v1.11/zh/docs/tasks/observability/metrics/using-istio-dashboard/>

### 3.4 Istio 运维建议

1. 版本选择

   Istio 是一个迭代很快的开源项目。频繁的版本迭代会给企业带来一些困扰：是坚持使用目前已经测试过的版本，还是使用社区的最新版本？

   社区的最新版本的 Istio 的稳定性往往不尽如人意。出于安全性和稳定性的考虑，建议使用各 Vendor 厂商的 Istio，或者比最新版本延迟 2 个小版本，比如 1.13 到 1.11。

2. 备用环境

   针对相同的应用，在 K8S 环境中部署一套不被 Istio 管理的环境。比如文中的三层微服务，独立启动一套不被 Istio 管理的应用，使用原本的访问方式即可。

   这样做的好处是，每当进行 Istio 升级或者部分参数调整时都可以提前进行主从切换，让流量切换到没有被 Istio 管理的环境中，将 Istio 升级调整验证完毕后再将流量切换回来。

3. 评估范围

   由于 Istio 对微服务的管理是非代码侵入式的。因此通常情况下，业务服务需要进行微服务治理，需要被 Istio 纳管。而对于没有微服务治理要求的非业务容器，不必强行纳管在 Istio
   中。当非业务容器需要承载业务时，被 Istio 纳管也不需要修改源代码，重新在 K8S 上注入 Sidecar 部署即可。

4. 配置生效

   Istio 中有的配置策略能够较快生效，有的配置需要一段时间才能生效，如限流、熔断等。新创建策略的生效速度要高于替换性策略。因此在不影响业务的前提下，可以在应用新策略之前，先删除旧策略。

   此外，Istio 的配置生效，大多是针对微服务所在的 Namespace，但也有一些配置是针对 Istio 系统。因此，在配置应用时，要注意指定对应的 Namespace。

   Virtual Service 和 Destination Rules都是针对 Namespace 生效，因此配置应用时需要指定项目。

5. 功能健壮性参考

   从大量的测试效果看，健壮性较强的功能有基于目标端的蓝绿、灰度发布，基于源端的蓝绿、灰度发布，灰度上线，服务推广，延迟和重试，错误注入，mTLS，黑白名单。

   健壮性有待提升的功能有限流和熔断。

6. 入口流量方式选择

   在创建 Ingress 网关的时候，会自动创建相应的路由（K8S Ingress 或 Router）。Ingress 网关能够暴露的端口要多于 K8S
   Ingress。所以，我们可以根据需要选择通过哪条路径来访问应用。

   在 Istio 体系中的应用不使用 Ingress 也可以正常访问微服务。但是 PaaS 上运行的应用未必都是 Istio 体系下的，其他非微服务或者非 Istio 体系下的服务还是要通过
   Ingress 访问。此外，Istio 本身的监控系统和 Kiali 的界面都是通过 Ingress 访问的。

   相比 Spring Cloud，Istio 较好地实现了微服务的路由管理。但在实际生产中，仅有微服务的路由管理是不够的，还需要诸如不同微服务之间的业务系统集成管理、微服务的 API
   管理、微服务中的规则流程管理等。

### 3.5 基于 KubeSphere 的服务网格

KubeSphere 对 istio 进行了初步的产品化封装。

#### 3.5.1 预置条件

准备环境：

- 硬件：8Core / 32G / 60G
- 操作系统：CentOS 7.9

安装 [K8S 1.23.6](#322-安装-k8s-1236)

#### 3.5.2 安装 KubeSphere v3.2.1（带 istio）

首先安装默认存储，直接用 [Local Path Provider](kubernetes-best-practices.md#45-local-和动态分配)

```bash
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.22/deploy/local-path-storage.yaml

kubectl patch storageclass local-path -p '{"metadata": {"annotations": {"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

然后[安装 KubeSphere](kubernetes-best-practices.md#341-kubesphere) v3.2.1

```bash
wget https://github.com/kubesphere/ks-installer/releases/download/v3.2.1/kubesphere-installer.yaml

# 替换 ks-installer 镜像，因为 nginx ingress controller 有 bug
sed -i 's/image: kubesphere/image: caas4/g' kubesphere-installer.yaml

# 同上，这两个镜像更新一下，也是因为上述 bug
docker pull caas4/ks-apiserver:v3.2.1
docker pull caas4/ks-controller-manager:v3.2.1
docker tag caas4/ks-apiserver:v3.2.1 kubesphere/ks-apiesrver:v3.2.1
docker tag caas4/ks-controller-manager:v3.2.1 kubesphere/ks-controller-manager:v3.2.1

kubectl apply -f kubesphere-installer.yaml

wget https://github.com/kubesphere/ks-installer/releases/download/v3.2.1/cluster-configuration.yaml

cp cluster-configuration.yaml cluster-configuration.yaml.bak
vi cluster-configuration.yaml
```

```diff
# diff cluster-configuration.yaml cluster-configuration.yaml.bak
78c78
<     enabled: true             # Enable or disable the KubeSphere DevOps System.
---
>     enabled: false             # Enable or disable the KubeSphere DevOps System.
148c148
<     enabled: true     # Base component (pilot). Enable or disable KubeSphere Service Mesh (Istio-based).
---
>     enabled: false     # Base component (pilot). Enable or disable KubeSphere Service Mesh (Istio-based).
```

```bash
kubectl apply -f cluster-configuration.yaml

# 看安装记录
kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l 'app in (ks-install, ks-installer)' -o jsonpath='{.items[0].metadata.name}') -f
```

### 3.6 灰度发布

[返回目录](#课程目录)

#### 3.6.1 部署测试用例

参考 [Github](https://github.com/99cloud/micro-service-demo) 或
[Gitee](https://gitee.com/dev-99cloud/micro-service-demo)，可以将 demo 部署到标准 K8S 环境。

然后，将 demo 部署到 KubeSphere

#### 3.6.2 金丝雀发布

在金丝雀发布中，您可以引入服务的新版本，并向其发送一小部分流量来进行测试。同时，旧版本负责处理其余的流量。如果一切顺利，您就可以逐渐增加向新版本发送的流量，同时逐步停用旧版本。如果出现任何问题，可以更改流量比例来回滚至先前版本。
该方法能够高效地测试服务性能和可靠性，有助于在实际环境中发现潜在问题，同时不影响系统整体稳定性。

1. 创建金丝雀发布任务，选择 reviews 服务，新建 v2 版本，以及使用新的 v2 版本镜像。可以根据版本间的流量比例或者请求头进行流量的分配
2. 访问服务，查看流量图
3. 如果一切运行顺利，则可以将所有流量引入新版本。在灰度发布中点击金丝雀发布任务，在出现的对话框中，点击 reviews v2 的三个点，选择接管所有流量。这代表 100% 的流量将会被发送到新版本
   (v2)。
4. 再次访问 Bookinfo，多刷新几次浏览器，您会发现页面只会显示 reviews v2 的结果（即带有黑色星标的评级）。下线该金丝雀任务，则 v1 版本会被自动删除

##### 3.6.2.1 按比例

1. 创建灰度发布，选择金丝雀发布
2. 修改 process 服务配置，加上 grayscale 选项
3. 使用按比例分配流量，新老版本各百分之 50
4. 访问图片服务，发现部分图片变成黑白图片
5. 在灰度中使用新版本接管所有流量
6. 访问图片服务，发现所有图片变成黑白图片

##### 3.6.2.2 按请求标签

1. 创建灰度发布，选择金丝雀发布
2. 修改 process 服务配置，加上 watermark 参数指定水印文本，例如 --watermark 图片测试
3. 使用按请求参数分配流量，设置 Cookie 匹配 userGroup=canary
4. 访问图片服务，所有图片均无水印
5. 在图片服务页面设置用户组为 canary 比刷新页面，发现所有图片均有水印文字
6. 在灰度中使用新版本接管所有流量
7. 访问图片服务任意用户组访问的图片均有水印

#### 3.6.3 蓝绿发布

蓝绿发布提供零宕机部署，即在保留旧版本的同时部署新版本。在任何时候，只有其中一个版本处于活跃状态，接收所有流量，另一个版本保持空闲状态。如果运行出现问题，您可以快速回滚到旧版本。

1. 在灰度策略选项卡下，点击蓝绿部署右侧的发布任务。
2. 待您实现蓝绿部署并且结果满足您的预期，您可以点击任务下线来移除 v1 版本，从而下线任务。操作跟金丝雀部署一致

#### 3.6.4 流量镜像

流量镜像 (Traffic Mirroring)，也称为流量影子 (Traffic
Shadowing)，是一种强大的、无风险的测试应用版本的方法，它将实时流量的副本发送给被镜像的服务。采用这种方法，您可以搭建一个与原环境类似的环境以进行验收测试，从而提前发现问题。由于镜像流量存在于主服务关键请求路径带外，终端用户在测试全过程不会受到影响。

1. 创建流量镜像，步骤跟前两个一致。
2. 可以点击任务下线移除流量镜像任务。此操作不会影响当前的应用版本。

### 3.7 服务治理

[返回目录](#课程目录)

#### 3.7.1 熔断

1. 修改 process 服务配置，修改 watermark 参数设置水印为 $POD 将 process 服务的 Pod 名作为水印
2. 访问图片服务，图片上有不同的 POD 名水印
3. 将 process 服务副本数设置为 2 ，设置熔断策略
4. 使用 curl <domain>/process/_statusCode 触发熔断，将一个 pod 隔离
5. 访问图片服务，所有图片上的水印都为同一 POD
6. 使用 curl <domain>/process/_statusCode 触发熔断，将所有 pod 隔离
7. 访问图片服务，图片无法正常加载
8. 删除熔断熔断，访问图片服务，图片正常展示
