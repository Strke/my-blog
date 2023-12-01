## Docker网络理论

### 1、Network Namespace

Docker 网络的底层原理是 Linux 的 Network Namespace，所以对于 Linux NetworkNamespace 的理解对 Docker 网络底层原理的理解非常重要。

Network Namespace 是 Linux 内核提供的用于实现网络虚拟化的重要功能，它能创建多个隔离的网络空间，每个独立的网络空间内的防火墙、网卡、路由表、邻居表、协议栈都是独立的。不管是虚拟机还是容器,当运行在独立的命名空间时,就像是一台单独的主机一样。

##### 1.1、创建两个命名空间

```bash
ip netns add ns1
ip netns add ns2
```

##### 1.2、在命名空间中执行命令

在ns1命名空间中执行命令```ip a```

```bash
ip netns exec ns1 ip a
```

##### 1.3、创建网络接口veth pair

使用虚拟设备接口技术可以让两个命名空间联通

下面的命令用于创建一对网络接口veth-ns1和veth-ns2

```bash
ip link add veth-ns1 type veth peer name veth-ns2
```

##### 1.4、分配ip给网络接口

给veth-ns2分配192.168.1.1/24的ip地址

```bash
ip netns exec ns1 ip addr add 192.168.1.1/24 dev veth-ns1
ip netns exec ns1 ip addr add 192.168.1.2/24 dev veth-ns2
```

##### 1.5、up设备

```bash
ip netns exec ns1 ip link set dev veth-ns1 up
ip netns exec ns2 ip link set dev veth-ns2 up
```

## Docker网络架构

Docker网络架构主要由CNM、Libnetwork、Driver构成

### 1、CNM

CNM，Container Network Model，容器网络模型，其是一种网络连接的**解决方案**，是一种设计规范、设计标准，其规定了 Docker 网络的基础组成要素。

CNM 中定义了三个基本要素: 沙盒 Sandbox，终端 Endpoint 与网络 Network。

* 沙盒:一个独立的网络栈，其中包括以太网接口、端口号、路由表、DNS 配置等。LinuxNetwork Namespace 是沙盒的标准实现。
* 终端: 虚拟网络接口，主要负责创建连接，即将沙盒连接到网络上。一个终端只能接入某一个网络。
* 网络: 802.1d 网桥的软件实现，是需要交互的终端的集合。



### 2、Libnetwork

CNM 是设计规范，而 Libnetwork 是开源的、由 Go 语言编写的、跨平台的 CNM 的标准实现。

Libnetwork 除了实现了 CNM 的三个组件，还实现了本地服务发现、容器负载均衡，以及网络控制层与管理层功能。

### 3、Driver

每种不同的网络类型都有对应的不同的底层 Driver，这些 Driver 负责在主机上真正实现需要的网络功能，例如创建 veth pair 设备等。

不过，无论哪种网络类型，其工作方式都是类似的。通过调用 Docker 引擎的 API 发出请求，然后由 Libnetwork 做出框架性的处理，然后将请求转发给相应的 Driver。

![](C:\Users\lin\Desktop\笔记--Docker\pic\微信图片_20231130213705.png)

## bridge网络

bridge 网络，也称为单机桥接网络，是 Docker 默认的网络模式。该网络模式只能存在于单个 Docker 主机上，其只能用于连接所在 Docker 主机上的容器。

### 1、docker0网桥

在 Linux 主机上，Docker 的 bridge 网络由 Bridge 驱动创建，其在创建时会创建一个默认的网桥 docker0。容器与网桥间是通过 veth pair 技术实现的连接，网桥与外网间是通过“网络地址转换 NAT 技术”实现的连接，即将通信的数据包中的内网地址转换为外网地址。

![](C:\Users\lin\Desktop\笔记--Docker\pic\微信图片_20231130215518.png)

##### 创建网桥（网络）

```bash
docker network create -d bridge
```

##### 容器互ping

* 默认的bridge网络不能用别名来表示容器的网络，自定义的网络bridge2能用别名来表示容器的网络
* 不同的bridge网络不能直接ping

![](C:\Users\lin\Desktop\笔记--Docker\pic\微信图片_20231130221118.png)



##### 定向连接

定向连接下，只能从bb4连接到bb1，不能从bb1连接到bb4

```bash
docker run --name bb4 -link bb1 -it box
```



## none网络

none网络，容器是一个独立的Network Namespace，但没有网络接口和IP

##### 创建none网络

使用--network none来指定创建的容器没有网络功能

```bash
docker run -d --name bb4 --network none box
```

## host网络

host网络，是与宿主机host共用一个Network Namespace。该网络类型的容器没有独立的网络空间，没有独立IP

##### 创建host网络

```BASH
docker run -d -name bb5 --network host box
```

host模型下，容器端口直接暴露给宿主机，不需要做网络映射





