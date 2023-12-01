+++

date=2023-11-23T16:08:08-04:00
description="Esmeralda"
featured_image="/images/esmeralda.jpg"
tags=[]
title="Dockerfile"
categories="docker"

+++

## Dockerfile

Dockerfile 是用于构建 Docker 镜像的脚本文件，由一系列指令构成。通过 docker build命令构建镜像时，Dockerfile 中的指令会由上到下依次执行，每条指令都将会构建出一个镜像。这就是镜像的分层。因此，指令越多，层次就越多，创建的镜像就越多，效率就越低，所以在定义 Dockerfile 时，能在一个指令完成的动作就不要分为两条。

## 指令

* 大小写不敏感，惯例时全大写
* 指令后至少携带一个参数
* #号开头为注释

#### CMD

```dockerfile
CMD ["EXECUTABLE", "PARAM1", "PARAM2",...]
# 执行EXECUTABLE可执行文件，后面的参数为运行参数
CMD command param1 param2,...
# command是 shell命令
CMD ["PARAM1", "PARAM2",...]
# 提供给ENTERYPOINT的默认参数
```

Dockerfile 中的[command]或[“EXECUTABLE”]如果是通过 CMD 指定的，则该镜像的启动命令 docker run 中是不能添加参数[ARG]的。因为 Dockerfile 中的 CMD 是可以被命令中的[COMMAND]替代的。如果命令中的 IMAGE 后仍有内容，此时对于 docker daemon 来说，其首先认为是替代用的[COMMAND]，如果有两个或两个以上的内容，后面的内容才会认为是[ARG]。所以，添加的-y 会报错，因为没有-y 这样的[COMMAND]。

#### ENTRYPOINT

```dockerfile
ENTRYPOINT ["EXECUTABLE", "PARAM1", "PARAM2",...]
# 调用EXECUTABLE应用程序，后面是两个参数
ENTRYPOINT command param1 param2,...
# command是shell命令
```

Dockerfile 中的[command]或[“EXECUTABLE”]如果是通过 ENTRYPOINT 指定的，则该镜像的启动命令 docker run 中是可以添加参数[ARG]的。因为 Dockerfile 中的 ENTRYPOINT 是不能被命令中的[COMMAND]替代的。如果命令中的IMAGE 后仍有内容，此时对于 docker daemon来说，其只能是[ARG]。

#### CMD与ENTRYPOINT总结

不过，docker daemon 对于 ENTRYPOINT 指定的[command]与[“EXECUTABLE”]的处理方式是不同的。如果是[command]指定的 shell，daemon 会直接运行，而不会与 docker run 中的[ARG]进行拼接后运行:如果是“EXECUTABLE”指定的命令，daemon 则会先与docker run中的[ARG]进行拼接，然后再运行拼接后的结果。

无论是CMD还是ENTRYPOINT，使用['EXECUTABLE']方式的通用性会更强

#### ADD

```dockerfile
ADD <src> <dest>
ADD ["<src>", "<dest>"]
```

 该指令将复制当前宿主机中指定文件 src 到容器中的指定目录 dest 中。src 可以是宿主机中的绝对路径，也可以时相对路径。但相对路径是相对于 docker build 命令所指定的路径的。src 指定的文件可以是一个压缩文件，压缩文件复制到容器后会自动解压为目录;src 也可以是一个 URL，此时的 ADD 指令相当于 wget 命令; src 最好不要是目录，其会将该目录中所有内容复制到容器的指定目录中。dest 是一个绝对路径，其最后面的路径必须要加上斜杠，否则系统会将最后的目录名称当做是文件名的。

#### copy 

功能与ADD相同，但是src不能是URL，且当src为压缩文件时，复制到容器后不会自动解压。

## Dockerfile文件内容

```dockerfile
FROM centos:7   # 基础镜像信息
MAINTAINER lin lin@qq.com   # 维护者信息
WORKDIR /usr   # 容器打开后默认进入的目录
WORKDIR local   # 后续的WORKDIR会基于之前的路径

RUN yum install -y wget vim net-tools   # 
CMD /bin/bash
```

## 悬虚镜像

悬虚镜像是指既没有 Repository 又没有 Tag 的镜像。当新建了一个镜像后，为该镜像指定了一个已经存在的 TAG，那么原来的镜像就会变为悬虚镜像。
为了演示悬虚镜像的生成过程，这里先修改前面定义的 Dockerfile，然后再生成镜像，且生成的新的镜像与前面构建的镜像的名称与 Tag 均相同。

### 删除悬虚镜像

* docker rmi \<imageID\>   指定imageID指定删除
* docker image prune    一次性删除本地所有悬虚镜像



## build cache

例子：

```dockerfile
FROM centos:7
LABEL auth = "Tom"
COPY hello.log /var/log/
RUN yum -y install vim
CMD /bin/bash
```

### 镜像生成过程

* FROM centos:7

FROM 指令为最终构建出的镜像设定了一个基础镜像。该语句不会产生新的镜像层，它是使用指定的镜像作为基础镜像层的。

* LABEL auth="Tom"

LABEL指令也会生成一个新的镜像层，该镜像层中记录了json文件内容的修改变化，没有文件系统的变化。

如果该指令就是最后一条指令,那么此时形成的镜像的文件系统其实就是原来 FROM 后指定镜像的文件系统，只是 json 文件发生了变化。但由于 ison 文件内容发生了变化，所以产生了新的镜像层。

* COPY hello.log /var/log/

COPY 指令会将宿主机中的指定文件复制到容器中的指定目录，所以会改变该镜像层文件系统大小,并生成新的镜像层文件系统内容，从而导致json文件中的镜像ID发生变化，产生了新的镜像层。

* RUN yum -y install vim

RUN 指令本身并不会改变镜像层文件系统大小，但由于其 RUN 的命令是 yum install,而该命令运行的结果是下载并安装一个工具，所以导致 RUN 命令最终也改变了镜像层文件系统大小，所以也就生成了新的镜像层文件系统内容。所以json 文件中的镜像 ID 也就发生了变化，产生了新的镜像层。

* CMD /bin/bash

对于 CMD 或 ENTRYPOINT 指令，其是不会改变镜像层文件系统大小的，因为其不会在docker build 过程中执行。所以该条指令没有改变镜像层文件系统大小。

但对于 CMD 或 ENTRYPOINT 指令，由于其是将来容器启动后要执行的命令，所以会将该条指令写入到 json 文件中，会引发 json 文件的变化。所以json 文件中的镜像 ID 也就发生了变化，产生了新的镜像层。

### docker build cache机制

Docker Daemnon 通过 Dockerfile 构建镜像时，当发现即将新构建出的镜像(层)与本地已存在的某镜像(层)重复时，默认会复用已存在镜像(层)而不是重新构建新的镜像(层)，这种机制称为 docker build cache 机制。该机制不仅加快了镜像的构建过程，同时也大量节省了Docker 宿主机的空间。

docker build cache 并不是占用内存的 cache，而是一种对磁盘中相应镜像层的检索、复用机制。所以，无论是关闭 Docker 引擎，还是重启 Docker 宿主机，只要该镜像(层)存在于本地，那么就会复用。

### build cache 失效

产生一下三种变化后，从发生变化的指令层开始所有的镜像层cache全部失效。即从该指令行开始的镜像层将构建出新的镜像层，而不再使用 buildcache，即使后面的指令并未发生变化。因为镜像关系本质上是一种树状关系，只要其上层节点变了，那么该发生变化节点的所有下层节点也就全部变化了

* Dockerfile文件发生变化
* ADD或COPY指令内容发生变化
* RUN指令外部依赖变化
* 指定不使用build cache

有些时候为了确保在镜像构建过程中使用到新的数据，在镜像构建 docker build 时，通过--no-cache 选项指定不使用 build cache。

### 清理悬虚build cache

指无法使用的build  cache，一般是悬虚镜像产生的build cache。

通过docker system prune 清除





















