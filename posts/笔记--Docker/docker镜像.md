## 镜像定位

对于任何镜像，都可通过<repository>:<tag>进行唯一定位。其中<tag>一般称为镜像的版本号。<tag>中有一个比较特殊的版本--latest。如果不指定，默认<tag>即为 latest。



#### 自动化镜像

![微信图片_20231120101446](C:\Users\lin\Desktop\笔记--Docker\pic\微信图片_20231120101446.png)

AUTOMATED 表示当前镜像是否是“自动化镜像”什么是自动化镜像?就是使用 DockerHub 连接一个包含 Dockerfile 文件(专门构建镜像用的文件)的 GitHub 仓库或 Bitbucket 仓库的源码托管平台，然后 Docker Hub 就会自动根据 Dockerfile 内容构建镜像。这种构建出的镜像会被标记为AUTOMATED,这种构建镜像的方式称为 Trusted Build(受信构建)。只要 Dockerfile文件内容发生变化，那么 Docker Hub 就会构建出新的镜像。

## 镜像操作

#### 1、docker pull

#### 2、docker images

#### 3、 docker search

​	通过docker search可以从docker hub上查看指定名称的镜像

#### 4、docker rmi

​	删除本地镜像	

* -f，--force  强制移除打开了容器的镜像

* --no-prune

#### 5、导入导出镜像

```dock
docker save -o my.tar tomacat:8.5.32 zookeeper：3.7
docker save tomacat:8.5.32 zookeeper：3.7 > zt.tar

docker load -i my.tar
```



## 镜像分层

Docker 镜像由一些松合的**只读**镜像层组成，Docker Daemon 负责堆叠这些镜像层，并将它们关联为一个统一的整体，即对外表现出的是一个独立的对象。

通过 docker pull 命令拉取指定的镜像时,每个 Pull complete 结尾的行就代表下载完毕了一个镜像层。

不过，分层结构的最大的好处是，在不同镜像间实现资源共享，即不同镜像对相同下层镜像的复用。对于 docker pull 命令,其在拉取之前会先获取到其要拉取镜像的所有 imagelD,然后在本地查找是否存在这些分层。如果存在，则不再进行拉取，而是共享本地的该分层大大节点的存储空间与网络带宽，提升了拉取效率。

#### 1、镜像层构成

每个镜像层由两部分构成：镜像文件系统与镜像Json文件。

* 镜像文件系统（FS)构成

​	(1)基础镜像层

​	所有镜像的最下层都具有一个可以看得到的基础镜像层 Base lmage,基础镜像层的文件系统称为根文件系统 rootfs。而 rootfs 则是建立在 Linux 系统中“看不到的”引导文件系统bootfs 之上。

​	(2)拓展镜像层

​	在基础镜像层之上的镜像层称为扩展镜像层。顾名思义,其是对基础镜像层功能的扩展。在 Dockerfile 中，每条指令都是用于完成某项特定功能的，而每条指令都会生成一个扩展镜像层。

![微信图片_20231120110925](C:\Users\lin\Desktop\笔记--Docker\pic\微信图片_20231120110925.png)

(3)容器层

​	一旦镜像运行了起来就形成了容器，而容器就是一个运行中的 Linux 系统，其也是具有文件系统的。容器的这个文件系统是在 docker 镜像最外层之上增加了一个可读写的容器层对文件的任何更改都只存在于容器层。因此任何对容器的操作都不会影响到镜像本身。

​	容器层如果需要修改某个文件，系统会从容器层开始向下一层层的查找该文件，直到找到为止。任何对于文件的操作都会记录在容器层。例如，要修改某文件，容器层会首先把在镜像层找到的文件 copy 到容器层，然后再进行修改。删除文件也只会将存在于容器层中的文件副本删除。

​	可以看出,Docker 容器就是一个叠加后的文件系统,而这个容器层称为 Union File System,联合文件系统。

##### 拓展、LinuxOS启动过程

​	Linux的bootfs文件系统由两部分构成：bootloader和kernel

![](C:\Users\lin\Desktop\笔记--Docker\pic\微信图片_20231120121826.png)

## 镜像摘要

一串长度64位的16进制字符串，是镜像内容的哈希值，是内容散列。

Docker 默认采用的 Hash 算法是 SHA256，即 Hah 值是一个长度为 256 位的二进制值Docker 使用 16 进制表示，即变为了长度为 64 位的字符串。

主要是用来区分相同\<repository\>:\<tag\>的不同镜像

## 多架构镜像

多架构镜像，是某<repository>中的某<tag>镜像针对不同操作系统/系统架构的不同镜像实现。即多架构镜像中包含的镜像的<repository>:<tag>都是相同的，但它们针对的操作系统/系统架构是不同的。

在 Docker Hub 中，镜像的多架构信息保存在 Manifest 文件中。在拉取镜像时，Docker会随着 pull 命令将当前 Docker 系统的 OS 与架构信息一并提交给 Docker Hub。Docker Hub 首先会根据镜像的<repository>:<tag>查找是否存在 Manifest。如果不存在，则直接查找并返回<repository>:<tag>镜像即可: 如果存在，则会在 Manifest 中查找是否存在指定系统/架构的镜像。如果存在该系统/架构，则根据 Manifest 中记录的地址找到该镜像的位置。
