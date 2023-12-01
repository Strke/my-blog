+++

date=2023-11-21T21:23:08-04:00
description="Esmeralda"
featured_image="/images/esmeralda.jpg"
tags=[]
title="Docker容器"
categories="docker"

+++





## 容器基础

### 1、容器启动流程

![](C:\Users\lin\Desktop\笔记--Docker\pic\微信图片_20231121190107.png)

通过 docker run 命令可以启动运行一个容器。该命令在执行时首先会在本地查找指定的镜像，如果找到了，则直接启动，否则会到镜像中心查找。如果镜像中心存在该镜像，则会下载到本地并启动，如果镜像中心也没有，则直接报错。

如果再与多架构镜像原理相整合，则就形成了完整的容器启动流程。

### 2、容器运行本质

Docker 容器存在的意义就是为了运行容器中的应用，对外提供服务，所以启动容器的目的就是启动运行该容器中的应用。容器中的应用运行完毕后，容器就会自动终止。所以如果不想让容器启动后立即终止运行，则就需要使容器应用不能立即结束。通常采用的方式有两种，使应用处于与用户交互的状态或等待状态。

## 容器运行命令

### 1、交互模式

交互模式运行ubuntu

```dockerfile
docker run --name myubuntu -it ubuntu:latest /bin/bash
```

交互模式运行tomcat

```dockerfile
docker run --name mytomcat8 -it tomcat:8.5.32 /bin/bash
```

### 2、守护进程模式

守护进程模式运行tomcat

```dockerfile
docker run --name mytom6 -dp 8082:8080 tomcat:8.5.49
```

### 3、删除容器

* 退出并停止容器 ：exit
* 退出不停止容器Ctrl+P+Q

### 4、exec

```dockerfile
docker exec -w root -it mytom /bin/bash
```

exec在exit退出容器时不会关闭容器

### 5、attach

必须要连接到正在运行的容器

```dockerfile
docker attach mytom2
```

附加标准输入、标准输出和错误流到正在运行的容器



### 6、ps

显示容器

### 7、容器启停

* start
* restart
* stop
* kill

### 8、删除容器

强制删除

```dockerfile
docker rm -f xxxx
```

### 9、把容器打包为镜像

```dockerfile
docker commit -a "xx <xxx@qq.com>" -m "xxx" mycentos
```

### 10、导入导出容器

* 导出容器export

将容器tom导出到/root目录的tomcat8.tar文件中

```dockerfile
docker export -o tomcat8.tar tom
```

* 加载容器import

```dockerfile
docker import tomcat8.tar <repository>:<tag>
```

* 与save/load命令的对比

  ​	export 与 save

  * export 作用于容器，save 作用于镜像，但它们导出的结果都为 tar 文件
  * export 一次只能对一个容器进行导出，save 一次可以对多个镜像进行导出
  * export 只是对当前容器的文件系统快照进行导出，其会丢弃原镜像的所有历史记录与元数据信息，save 则是保存了原镜像的完整记录

  import与load

  * import 导入的是容器包，load 加载的是镜像包，但最终都会恢复为镜像
  * import 恢复为的镜像只包含当前镜像一层,load 恢复的镜像与原镜像的分层是完全相同的
  * import 恢复的镜像就是新构建的镜像，与原镜像的 magelD 不同: load 恢复的镜像与原镜像是同一个镜像，即ImagelD 相同
  * import 可以为导入的镜像指定\<repository\>与\<tag\>,load 加载的镜像不能指定\<repository\>与\<tag\>，与原镜像的相同,

* 与docker commit 对比

相同点: docker export + docker import 会将一个容器变为一个镜像，docker commit 也可以将一个容器变一个镜像。“
不同点: docker export + docker import 恢复的镜像仅包含原容器生成的一层分层,docker commit 生成的镜像中包含容器的原镜像的所有分层信息。













