### Docker引擎架构

Docker 引擎是用来运行和管理容器的核心软件，其现代架构由四部分主要组件构成:Docker Client，Dockerd、Containerd 与 Runc。

* Docker Client

Docker 客户端，Docker 引擎提供的 CLI工具，用于用户向 Docker 提交命令请求。

* Dockerd

Dockerd，即 Docker Daemon。在现代 Dockerd 中的主要包含的功能有镜像构建、镜像管理、RESTAPI、核心网络及编排等。其通过gRPC与 Containerd 进行通信。

* Containerd

Containerd，即 Container Daemon，该项目的主要功能是管理容器的生命周期。不过其本身并不会去创建容器，而是调用 Runc 来完成容器的创建。

* Runc

Runc 只有一个作用一创建容器，其本质是一个独立的容器运行时 CLI工具。其在 fork出一个容器子进程后会启动该容器进程。在容器进程启动完毕后，Runc 会自动退出。

* Shim

Shim 是实现“Daemonless Container”不可或缺的工具，使容器与 Docker Daemon 解耦,使得 DockerDaemon 的维护与升级不会影响到运行中的容器。
每次创建容器时,Containerd 会先 fork 出 Shim 进程,再由 Shim 进程 fork 出 Runc 进程当 Runc自动退出之前，会先将新容器进程的父进程指定为相应的 shim 进程。”除了作为容器的父进程外，shim 进程还具有两个重要功能:

​	1、保持所有 STDIN 与STDOUT 流的开启状态，从而使得当 Docker Daemon 重启时，容器不会因为 Pipe 的关闭而终止。

​	 2、将容器的退出状态反馈给 Docker Daemon。

![4](C:\Users\lin\Desktop\笔记--Docker\pic\4.png)



