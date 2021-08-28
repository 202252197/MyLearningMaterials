


## Docker简介
---
Docker是基于Go语言开发的！开源项目！
Docker的官网：https://www.docker.com
Docker的文档地址：https://docs.docker.com   (Docker的文档是超级详细的)
Docker的仓库地址：https://hub.docker.com
Docker的下载：（进入Docker文档页面就有下载的入口）
![[Pasted image 20210822154800.png]]

### Docker解决了什么问题
这是一个典型的应用场景，Docker Image 中包含了程序需要的所有的运行时依赖，比如Java的程序，肯定要Image中包含JDK；比如Python的程序，肯定要在Image中包含对应版本的Python解释器。程序在我这跑得好好的，去你那就不行了，显然是环境问题。Docker把整个运行时环境打包放到image中，所以搞定了环境依赖问题！

比如你发布一个项目（JAR包+（环境Redis，Mysql，JDK，ES））,项目需要带上环境安装打包！此时Docker就可以帮你解决。相当于Docker帮你将JAR包和环境打包到一起部署上线，一套流程做完！

### Docker和虚拟机的不同
传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件

Docker守护进程可以直接与主操作系统进行通信，为各个Docker容器分配资源；它还可以将容器与主操作系统隔离，并将各个容器互相隔离。虚拟机启动需要数分钟，而Docker容器可以在数毫秒内启动。由于没有臃肿的从操作系统，Docker可以节省大量的磁盘空间以及其他系统资源。

说了这么多Docker的优势，大家也没有必要完全否定虚拟机技术，因为两者有不同的使用场景。虚拟机更擅长于彻底隔离整个运行环境。例如，云服务提供商通常采用虚拟机技术隔离不同的用户。而Docker通常用于隔离不同的应用，例如前端，后端以及数据库。

每个Docker容器间是互相隔离，每个Docker容器内都有一个属于自己的文件系统，互不影响。

**理解虚拟机**
使用**虚拟机**运行多个相互隔离的应用时，如下图:
![[Pasted image 20210823212850.png]]
**从下到上理解上图:**

-   **基础设施(Infrastructure)**。它可以是你的**个人电脑**，数据中心的**服务器**，或者是**云主机**。
-   **虚拟机管理系统(Hypervisor)**。利用Hypervisor，可以在**主操作系统**之上运行多个不同的**从操作系统**。类型1的Hypervisor有支持MacOS的**HyperKit**，支持Windows的**Hyper-V、Xen**以及**KVM**。类型2的Hypervisor有VirtualBox和VMWare workstation。
-   **客户机操作系统(Guest Operating System)**。假设你需要运行3个相互隔离的应用，则需要使用Hypervisor启动3个**客户机操作系统**，也就是3个**虚拟机**。这些虚拟机都非常大，也许有700MB，这就意味着它们将占用2.1GB的磁盘空间。更糟糕的是，它们还会消耗很多CPU和内存。
-   **各种依赖。**每一个**客户机操作系统**都需要安装许多依赖。如果你的应用需要连接PostgreSQL的话，则需要安装**libpq-dev**；如果你使用Ruby的话，应该需要安装gems；如果使用其他编程语言，比如Python或者Node.js，都会需要安装对应的依赖库。
-   **应用**。安装依赖之后，就可以在各个**客户机操作系统**分别运行应用了，这样各个应用就是相互隔离的。

**理解Docker容器**
使用**Docker容器**运行多个相互隔离的应用时，如下图:
![[Pasted image 20210823213151.png]]
不难发现，相比于**虚拟机**，**Docker**要简洁很多。因为我们不需要运行一个臃肿的**客户机操作系统**了。

**从下到上理解上图:**

-   **基础设施(Infrastructure)**。
-   **主操作系统(Host Operating System)**。所有主流的Linux发行版都可以运行Docker。对于MacOS和Windows，也有一些办法”运行”Docker。
-   **Docker守护进程(Docker Daemon)**。Docker守护进程取代了Hypervisor，它是运行在操作系统之上的后台进程，负责管理Docker容器。
-   **各种依赖**。对于Docker，应用的所有依赖都打包在**Docker镜像**中，**Docker容器**是基于**Docker镜像**创建的。
-   **应用**。应用的源代码与它的依赖都打包在**Docker镜像**中，不同的应用需要不同的**Docker镜像**。不同的应用运行在不同的**Docker容器**中，它们是相互隔离的。
**对比虚拟机与Docker**

**Docker守护进程**可以直接与**主操作系统**进行通信，为各个**Docker容器**分配资源；它还可以将容器与**主操作系统**隔离，并将各个容器互相隔离。**虚拟机**启动需要数分钟，而**Docker容器**可以在数毫秒内启动。由于没有臃肿的**从操作系统**，Docker可以节省大量的磁盘空间以及其他系统资源。

说了这么多Docker的优势，大家也没有必要完全否定**虚拟机**技术，因为两者有不同的使用场景。**虚拟机**更擅长于彻底隔离整个运行环境。例如，云服务提供商通常采用虚拟机技术隔离不同的用户。而**Docker**通常用于隔离不同的应用，例如**前端**，**后端**以及**数据库**。

## Docker的基本组成
---

![[Pasted image 20210822160557.png]]

-   **Client：客户端**
-   **DOCKER_HOST：Docker的服务**
-   **Registry：远程仓库**
-   **docker build：构建一个容器**
-   **docker pull：拉取一个容器**
-   **docker run：运行一个容器**
-   **Docker daemon：Docker的守护进程**
-   **Images：镜像（可以理解为Class类）（镜像就好比是一个模板，可以通过模板创建容器实例或者说容器服务，一个镜像可以创建多个实例）**
-   **Containers：多个容器（可以理解为类实例出来的对象）（可以对容器进行启动，停止，删除等基本命令）**
-   **Registry：远程仓库（存放镜像的地方，仓库分为公有仓库和私有仓库）（Docker Hub是国外的共有仓库地址，可以配置镜像加速使用国内阿里云的仓库）**

## 安装Docker
---
+ 首先使用 **uname -r** 命令查看一下内核发行版本号（**我的是3.10.0-1160.e17.x86_64**）
+ 然后使用cat /etc/os-release命令查看一下系统的版本（**我的是Centos7**）
+ 安装之前请先执行下面命令卸载掉旧的docker

``````
sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
``````
+ 安装yum的扩展包
```
sudo yum install -y yum-utils
```
+ 添加软件源信息
```
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo   #默认是设置国外的软件源地址
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo   #推荐使用阿里云国内的软件源地址
```
+ 更新yum软件包索引
```
sudo yum makecache fast
```
+ 安装docker相关的    docker-ce是社区版本  ee是企业版本
```
sudo yum install docker-ce docker-ce-cli containerd.io
```
+ 启动docker
```
sudo systemctl start docker
```
+ 查看docker是否安装成功
```
docker version
```
+ 运行docker hello world程序
```
sudo docker run hello-world
```
+ 查看一下下载的这个hello-world镜像
```
docker images
```
## 卸载Docker
---
+ 卸载依赖
```
sudo yum remove docker-ce docker-ce-cli containerd.io
```
+ 删除资源
```
 sudo rm -rf /var/lib/docker
 sudo rm -rf /var/lib/containerd
```
## 配置阿里云镜像加速器
---
+ 首先打开阿里云，搜索容器镜像服务，打开如下

![[Pasted image 20210822165657.png]]
+ 配置镜像加速器
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://xg92zt19.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
## Run的流程和Docker的原理
---
### Run  image（镜像）的流程
![[Pasted image 20210822170832.png]]
+ 运行一个docker镜像文件的时候，首先Docker会在本机寻找镜像
+ 判断本机是否有这个镜像文件
+ 如果有就直接运行这个镜像文件
+ 如果没有就去Docker Hub上去下载
+ Docker Hub是否可以找的到
+ 找不到返回错误，找不到镜像
+ 找到了的话就去下载这个镜像到本地，然后使用这个镜像进行运行

### Docker底层原理
Docker 是一个 Client-Server结构的系统，Docker的守护进程运行在主机上，通过Socket从客户端访问 ！

Docker Server接收到Docker-Client的 指令,就会执行这个命令！
![[Pasted image 20210822175438.png]]

**Docker为什么比VM快？**
+ Docker有着比虚拟机更少的抽象层
+ Docker利用的是宿主机的内核,vm需要是Guest OS。

![[Pasted image 20210822181258.png]]
所以说,新建一个容器的时候,docker不需要像虚拟机一样重新加载一个操作系统内核，避免引导。虚拟机是加载Guest OS，分钟级别的，而Docker是利用宿主机的操作系统，省略了这个复杂的过程，秒级！
![[Pasted image 20210822181318.png]]
## Docker的常用命令
---
```
docker version       #显示docker的版本信息
docker info          #显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help    #万能命令
```
### Docker的镜像命令
#### docker images 
**查看本地主机所有的镜像** 
```
[root@bogon ~]# docker images 
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   5 months ago   13.3kB

#解释
REPOSITORY	镜像的仓库源
TAG			镜像的标签
IMAGE ID	镜像的id
CREATED		镜像的创建时间
SIZE		镜像的大小

#可选项
	-a,--all	#列出所有镜像
    -q,--quiet	#只显示镜像的id
```
#### docker search
**搜索镜像**
```
[root@bogon ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   11263     [OK]       
mariadb                           MariaDB Server is a high performing open sou…   4278      [OK]       
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   836                  [OK]

#可选项，通过搜索来过滤
--filter=STARS=3000		#搜索出来的镜像就是STARS大于3000的

[root@bogon ~]# docker search mysql --filter=STARS=3000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   11263     [OK]       
mariadb   MariaDB Server is a high performing open sou…   4278      [OK] 

[root@bogon ~]# docker search mysql --filter=STARS=5000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   11263     [OK] 
```
#### docker pull
**下载镜像**
```
# 下载镜像  docker pull 镜像名[:tag]
[root@bogon ~]# docker pull mysql
Using default tag: latest	#如果不写tag,默认就是latest
latest: Pulling from library/mysql
33847f680f63: Pull complete  #分层下载,docker image的核心	联合文件系统
5cb67864e624: Pull complete 
1a2b594783f5: Pull complete 
b30e406dd925: Pull complete 
48901e306e4c: Pull complete 
603d2b7147fd: Pull complete 
802aa684c1c4: Pull complete 
715d3c143a06: Pull complete 
6978e1b7a511: Pull complete 
f0d78b0ac1be: Pull complete 
35a94d251ed1: Pull complete 
36f75719b1a9: Pull complete 
Digest: sha256:8b928a5117cf5c2238c7a09cd28c2e801ac98f91c3f8203a8938ae51f14700fd		#签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest	#真实地址
 
#等价于它
docker pull mysql
docker pull docker.io/library/mysql:latest

#指定版本下载
[root@bogon ~]# docker pull mysql:5.7
5.7: Pulling from library/mysql
33847f680f63: Already exists 
5cb67864e624: Already exists 
1a2b594783f5: Already exists 
b30e406dd925: Already exists 
48901e306e4c: Already exists 
603d2b7147fd: Already exists 
802aa684c1c4: Already exists 
5b5a19178915: Pull complete 
f9ce7411c6e4: Pull complete 
f51f6977d9b2: Pull complete 
aeb6b16ce012: Pull complete 
Digest: sha256:be70d18aedc37927293e7947c8de41ae6490ecd4c79df1db40d1b5b5af7d9596
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```
![[Pasted image 20210822194021.png]]
#### docker rmi
**删除镜像**
```
#删除指定的镜像
[root@bogon ~]# docker rmi -f 8cf625070931  #删除mysql5.7的镜像文件
Untagged: mysql:5.7
Untagged: mysql@sha256:be70d18aedc37927293e7947c8de41ae6490ecd4c79df1db40d1b5b5af7d9596
Deleted: sha256:8cf6250709314f2fcd2669e8643f5d3bdebfe715bddb63990c8c96e5d261d6fc
Deleted: sha256:452fe6896278c26338d547f8d1092011d923785247c46629b374d3477fe28c84
Deleted: sha256:bd40bf60af5d06e6b93eaf5a648393d97f70998faa3bfa1b85af55b5a270cb35
Deleted: sha256:c43e9e7d1e833650e0ed54be969d6410efa4e7fa6e27a236a44a2b97e412ee93
Deleted: sha256:70f18560bbf492ddb2eadbc511c58c4d01e51e8f5af237e3dbb319632f16335b

[root@bogon ~]# docker images	#可以看到已经删除mysql5.7了
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
mysql         latest    c60d96bd2b77   3 weeks ago    514MB
hello-world   latest    d1165f221234   5 months ago   13.3kB

#删除多个镜像

docker rmi -f 镜像ID 镜像ID 镜像ID 镜像ID

#删除全部镜像
[root@bogon ~]# docker rmi -f $(docker images -aq)   #  $()可以传入参数，将docker images -aq 查询所有image的镜像id
Untagged: mysql:latest
Untagged: mysql@sha256:8b928a5117cf5c2238c7a09cd28c2e801ac98f91c3f8203a8938ae51f14700fd
Deleted: sha256:c60d96bd2b771a8e3cae776e02e55ae914a6641139d963defeb3c93388f61707
Deleted: sha256:5c8c91273faab368a6d659156f2569fa9f40b0e0139222fdf9eef073df4b3797
Deleted: sha256:33d8196a776f42a16f10395b66f10f91443b1fb194bca2a9b8dfb0deff5babb8
Deleted: sha256:3ec63323025213e3cabf17ac7933506dc5520ec49226a9764418f77ea60d35c8
Deleted: sha256:1f129b005b51b049ac84ed0775b82096d480b7d9308a9a137697f37346562266
Deleted: sha256:80ed209bd0434faa1ce31fbaab8508124dddf8f6502c5736ee4b8e46697a8477
Deleted: sha256:e53f0d35c77064014a5c1c1332d84d5f421a58418ca9c208bc470691c0e483e3
Deleted: sha256:75209fb28131d5537e73406ff0f6f508f3eb1f4d86c43d1d16df76fd28b9cc35
Deleted: sha256:34a01bee1a62a01034ffc3da48a3cb45716a0cf2e264c26663e02288e81c7ec2
Deleted: sha256:9f8bca37a56017fd3462d4fc329b0b20f97c2dd4c15e55a8e6ad1c023ab5552b
Deleted: sha256:c8a6e3f9a2412c28cd8c48e2c7bed5e7fbaa0ab6649add2dbe8641cb29b967f6
Deleted: sha256:0a26eacdbd862e75d064d817e8a5bcf5e060c2680c10f77ffa52757c0b8c3328
Deleted: sha256:814bff7343242acfd20a2c841e041dd57c50f0cf844d4abd2329f78b992197f4
Untagged: hello-world:latest
Untagged: hello-world@sha256:0fe98d7debd9049c50b597ef1f85b7c1e8cc81f59c8d623fcb2250e8bec85b38
Deleted: sha256:d1165f2212346b2bab48cb01c1e39ee8ad1be46b87873d9ca7a4e434980a7726

[root@bogon ~]# docker images	#可以看到已经删除全部镜像了
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```
### Docker的容器命令
**说明：有了镜像才可以创建容器，下载一个centos镜像来测试**
```
[root@bogon ~]# docker pull centos		#下载centos镜像
Using default tag: latest
latest: Pulling from library/centos
7a0437f04f83: Pull complete 
Digest: sha256:5528e8b1b1719d34604c87e11dcd1c0a20bedf46e83b5632cdeac91b8c04efc1
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest


[root@bogon ~]# docker images		#下载完成
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
centos       latest    300e315adb2f   8 months ago   209MB
```
#### docker run image
**新建容器并启动**
```
docker run [可选参数]	image

#参数说明
--name="Name"	容器名称 tomcat01 tomcat02,用来区分容器
-d				后台方式运行
-it				使用交互方式运行，进入容器查看内容
-p				指定容器的端口 -p 8080:8080
	#-p有四种使用方式
	-p  ip:主机端口:容器端口
	-p	主机端口:容器端口
	-p	容器端口
	容器端口

-p	随机指定端口

#测试，启动并进入容器
[root@bogon ~]# docker run -it centos /bin/bash

#如果出现WARNING: IPv4 forwarding is disabled. Networking will not work.
解决办法：
vi /etc/sysctl.conf
net.ipv4.ip_forward=1  #添加这段代码

然后重启虚拟机

#查看是否修改成功 （备注：返回1，就是成功）
[root@docker-node2 ~]# sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 1

#从容器中退回到主机
[root@c8eb77459317 /]# exit 
exit

[root@lvshihao /]# ls    #[root@bogon ~]变成[root@lvshihao /]是因为我修改了一下hostname(也就是主机名)
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```
#### docker ps
**列出所有的运行的容器**
```
#docker ps 命令
        	#列出当前正在运行的容器
	-a		#列出当前正在运行的容器，包括历史运行过的容器
	-n=?	#显示最近创建的容器 ?代表显示最近创建的前几个容器
	-q		#只显示容器的编号

[root@lvshihao /]# docker ps	#列出当前正在运行的容器
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

[root@lvshihao /]# docker ps -a	#列出当前正在运行的容器，包括历史运行过的容器
CONTAINER ID   IMAGE          COMMAND       CREATED             STATUS                         PORTS     NAMES
c8eb77459317   centos         "/bin/bash"   18 minutes ago      Exited (0) 15 minutes ago                pedantic_goldberg
3fd14ec500c4   centos         "/bin/bash"   29 minutes ago      Exited (1) 21 minutes ago                fervent_borg
9186fe713fdd   d1165f221234   "/hello"      About an hour ago   Exited (0) About an hour ago             intelligent_bouman
```
#### 退出容器命令
```
exit			#直接容器停止并退出
Ctrl + P + Q 	#容器不停止退出
```
#### docker rm
**删除容器**
```
docker rm 容器id	#删除指定容器，不能删除正在运行的容器，如果要强制删除rm -f
docker rm -f $(docker ps -aq)	#删除所有容器
docker ps -a -q|xargs docker rm -f	#删除所有容器
```
#### 启动和停止和重启容器的命令
```
docker start 容器id	#启动容器
docker restart 容器id	#重启容器
odcker stop	容器id	#停止当前正在运行的的容器
docker kill	容器id	#强制停止当前容器
```
**注：”如果发现你的Linux系统终端里的root@localhost变成root@bogon的主机名“** 

解决办法：
```
[root@bogon ~]# hostname localhost
[root@bogon ~]# su
[root@localhost ~]# 
```
### 常用其他命令
#### 后台启动容器
```
#命令 docker run -d 镜像名！
[root@lvshihao /]# docker run -d centos
757173133e8e73985f024dc7e0506f4ac773ce6ba1fbbce18584eda80dbc7603

[root@lvshihao /]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS                     PORTS     NAMES
757173133e8e   centos    "/bin/bash"   9 seconds ago   Exited (0) 8 seconds ago             optimistic_banach

#问题docker ps,返现centos已经停止了

#常见的坑,docker容器使用后台运行,就必须要有一个前台进程，docker发现没有应用，就会停止
```
#### 查看容器日志
```
docker logs -f -t --tail 显示的条数 容器ID      #发现centos没有日志

#自己编写一段shell脚本,让centos在后台一直每一秒执行着输出lvshihao的操作，这样centos就不会立刻挂掉
docker run -d centos /bin/bash -c "while true;do echo lvshihao;sleep 1;done"

[root@lvshihao /]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS     NAMES
3ab34a21e1cc   centos    "/bin/bash -c 'while…"   About a minute ago   Up About a minute             sharp_yonath

#显示日志
-tf	#显示日志
--tail number #要显示日志条数

[root@lvshihao /]# docker logs -tf --tail 3 3ab34a21e1cc
2021-08-14T01:52:57.239363667Z lvshihao
2021-08-14T01:52:58.243463010Z lvshihao
2021-08-14T01:52:59.249260645Z lvshihao
```
#### 查看容器中进程信息
```
#命令 docker top 容器id
[root@lvshihao /]# docker top 3ab34a21e1cc
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                70857               70839               0                   09:49               ?                   00:00:00            /bin/bash -c while true;do echo lvshihao;sleep 1;done
root                76932               70857               0                   09:56               ?                   00:00:00            /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
```
#### 查看镜像的元素据
```
#命令
docker inspect 容器id

#测试
[root@lvshihao /]# docker inspect 3ab34a21e1cc
[
    {
        "Id": "3ab34a21e1cce570adf734bb9cea5b1df197bbafee806dd137cdf839c9f5af32",
        "Created": "2021-08-14T01:49:57.957598671Z",
        "Path": "/bin/bash",
        "Args": [
            "-c",
            "while true;do echo lvshihao;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 70857,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-08-14T01:49:58.36755625Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
        "ResolvConfPath": "/var/lib/docker/containers/3ab34a21e1cce570adf734bb9cea5b1df197bbafee806dd137cdf839c9f5af32/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/3ab34a21e1cce570adf734bb9cea5b1df197bbafee806dd137cdf839c9f5af32/hostname",
        "HostsPath": "/var/lib/docker/containers/3ab34a21e1cce570adf734bb9cea5b1df197bbafee806dd137cdf839c9f5af32/hosts",
        "LogPath": "/var/lib/docker/containers/3ab34a21e1cce570adf734bb9cea5b1df197bbafee806dd137cdf839c9f5af32/3ab34a21e1cce570adf734bb9cea5b1df197bbafee806dd137cdf839c9f5af32-json.log",
        "Name": "/sharp_yonath",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/fd530145a93bd0d4db9acd20db467c650621cc30e5f605bdeb7e282ee380cc14-init/diff:/var/lib/docker/overlay2/3bdf7ab69d622c811093a3a63304f2742982927912dc4299e5178f5318e5f5e8/diff",
                "MergedDir": "/var/lib/docker/overlay2/fd530145a93bd0d4db9acd20db467c650621cc30e5f605bdeb7e282ee380cc14/merged",
                "UpperDir": "/var/lib/docker/overlay2/fd530145a93bd0d4db9acd20db467c650621cc30e5f605bdeb7e282ee380cc14/diff",
                "WorkDir": "/var/lib/docker/overlay2/fd530145a93bd0d4db9acd20db467c650621cc30e5f605bdeb7e282ee380cc14/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "3ab34a21e1cc",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash",
                "-c",
                "while true;do echo lvshihao;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20201204",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "7af8de99d2031e7131411d3fee641b0c0d0b2ecc240603f83a786bedf877894d",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/7af8de99d203",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "aa1b52291a367be48b38a3ab5b7615f7a415e88f757e4f82b8bfa096d0a28019",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "04da5e0f80d775440a2afa1f93fd7a2ddd829ee3c504c9d9e75305865509a305",
                    "EndpointID": "aa1b52291a367be48b38a3ab5b7615f7a415e88f757e4f82b8bfa096d0a28019",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```
#### 进入当前正在运行的容器
```
#我们容器通常都是使用后台方式运行的,需要进入容器，修改一些配置



#方式一
docker exec -it 容器id bashShell


[root@bogon ~]# docker ps 
CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS         PORTS     NAMES
3ab34a21e1cc   centos    "/bin/bash -c 'while…"   10 hours ago   Up 5 seconds             sharp_yonath

[root@bogon ~]# docker exec -it 3ab34a21e1cc /bin/bash

[root@3ab34a21e1cc /]# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  1 11:41 ?        00:00:00 /bin/bash -c while true;do echo lvshihao;sleep 1;done
root         40      0  0 11:41 pts/0    00:00:00 /bin/bash
root         62      1  0 11:42 ?        00:00:00 /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
root         63     40  2 11:42 pts/0    00:00:00 ps -ef


#方式二
docker attach 容器id

#测试
[root@lvshihao ~]# docker ps 
CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS         PORTS     NAMES
3ab34a21e1cc   centos    "/bin/bash -c 'while…"   10 hours ago   Up 6 minutes             sharp_yonath

[root@lvshihao ~]# docker attach 3ab34a21e1cc
lvshihao
lvshihao
正在执行当前的代码...


#docker exec	进入容器后开启一个新的终端，可以在里面操作（常用）
#docker attach	进入容器正在执行的终端，不会启动新的进程！
```
#### 从容器内拷贝文件到主机上
```
docker cp 容器id:容器内路径	目的主机路径

[root@lvshihao ~]# docker ps 
CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS          PORTS     NAMES
3ab34a21e1cc   centos    "/bin/bash -c 'while…"   10 hours ago   Up 16 minutes             sharp_yonath

#进入正在运行的容器
[root@lvshihao ~]# docker exec -it 3ab34a21e1cc /bin/bash

#在home目录创建一个lvshihao66.txt
[root@3ab34a21e1cc /]# cd /home/
[root@3ab34a21e1cc home]# touch lvshihao66.txt

#退回到主机上
[root@lvshihao ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS          PORTS     NAMES
3ab34a21e1cc   centos    "/bin/bash -c 'while…"   10 hours ago   Up 17 minutes             sharp_yonath

#将容器的文件copy到主机上面
[root@lvshihao ~]# docker cp 3ab34a21:/home/lvshihao66.txt /home

#在主机上面查看是否copy成功
[root@lvshihao ~]# cd /home/

[root@lvshihao home]# ls
lvshihao  lvshihao66.txt
```
## 部署Nginx
---
```
#1.搜索镜像 search 建议大家去docker hub搜索，可以看到帮助文档
#2.下载镜像	pull
#3.运行测试
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    08b152afcfae   3 weeks ago    133MB
centos       latest    300e315adb2f   8 months ago   209MB

# -d 后台运行
# --name 给容器命名
# -p 宿主机端口，容器内部端口

[root@localhost ~]# docker run -d --name nginx01 -p 3344:80 nginx
98fe864057ca7582ada2fc6b0b6f0fa3c40416481fb6ad7663931916315e84be

[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS         PORTS                                   NAMES
98fe864057ca   nginx     "/docker-entrypoint.…"   13 seconds ago   Up 6 seconds   0.0.0.0:3344->80/tcp, :::3344->80/tcp   nginx01

[root@localhost ~]# curl localhost:3344

#进入容器
[root@localhost ~]# docker  exec -it nginx01 /bin/bash

root@98fe864057ca:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx

root@98fe864057ca:/# cd /etc/nginx/

root@98fe864057ca:/etc/nginx# ls
conf.d  fastcgi_params  mime.types  modules  nginx.conf  scgi_params  uwsgi_params
```
## 部署Tomcat
---
```
#官方的使用
docker run -it --rm tomcat:9.0
 
#我们之前的启动都是后台，停止了容器之后，容器还是可以查到  docker run -it --rm，一般用来测试，用完就删除

#下载再启动
docker pull tomcat

#启动运行
docker run -d -p 3355:8080 --name tomcat01 tomcat

#测试访问没有问题

#进入容器
[root@localhost ~]# docker exec -it tomcat01 /bin/bash

#发现问题：
#  1.linux命令少了，2.webapps没有文件，阿里云镜像的原因，默认是最小的镜像，所有不必要的都剔除掉
#  保证最小可运行环境
```
## 部署ElasticSearch
---
```
# es 暴露的端口很多！
# es 十分的耗内存
# es 的数据需要放置到安全目录！可以使用docker的挂载
# --net somenetwork 是网络配置
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:tag

#启动 elaticsearch
docker run -d --name elasticsearch  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2

#测试一下es是否成功
[root@localhost ~]# curl localhost:9200
{
  "name" : "566b9cc477be",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "IAy-Zq6LTqa2dhC8LLCBSA",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

#查看cpu的状态
[root@localhost ~]# docker stats 
CONTAINER ID   NAME                 CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O        PIDS
e5dc2070b9ac   elasticsearch        0.28%     469.4MiB / 972.3MiB   48.28%    1.18kB / 942B     3GB / 277MB      43
```
可以看到我的虚拟机是1G内存，这个Elastic Search占用了我469.4M，相当于占用了我48.28%的内存，我们可以对ES增加内存的限制，修改配置文件。
我们可以通过-e参数进行环境配置的修改，先关闭掉elastic search
```
#关闭掉刚刚启动的ElasticSearch
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                                                                  NAMES
566b9cc477be   elasticsearch:7.6.2   "/usr/local/bin/dock…"   37 minutes ago   Up 36 minutes   0.0.0.0:9200->9200/tcp, :::9200->9200/tcp, 0.0.0.0:9300->9300/tcp, :::9300->9300/tcp   elasticsearch

[root@localhost ~]# docker stop 566b9cc477be

#重新运行一个ElasticSearch实例
docker run -d --name elasticsearch02  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2

#启动成功之后查看是否能够正常访问
[root@localhost ~]# curl localhost:9200
{
  "name" : "9ca8bdf59d48",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "xgzM52tdTRyF-xqCzuPdBg",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```
然后再看一下内存占用的情况**docker stats**
```
CONTAINER ID   NAME                 CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O        PIDS
614b5775be06   elasticsearch02      18.58%    375.2MiB / 972.3MiB   38.58%    1.18kB / 942B     303MB / 1.82MB   42
```
可以看到此时占用的的内存就少了很多，依然正常访问成功
## Portainer可视化面板安装
---
Portainer是Docker图形化界面管理工具！提供一个后台面板供我们操作！
```
# portainer安装

 docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
 ```
 访问测试:http://ip:8088
 ![[Pasted image 20210822214038.png]]
 输入你要设置的密码和确认密码
 ![[Pasted image 20210822214101.png]]
 然后选择Local本地的，点击Connect，然后进入docker面板
 ![[Pasted image 20210822214121.png]]
 可视化面板几乎不会用，测试玩玩即可
## Docker镜像讲解
---
### 镜像是什么
镜像是一种轻量级，可执行的独立软件包，用来打包软件运行环境和基本运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码，运行时，库，环境变量和配置文件。
### Docker镜像加载原理
**UnionFS(联合文件系统)**

UnionFS(联合文件系统)：Union文件系统（UnionFS）是一种分层，轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下，Union文件系统是Docker镜像的基础，镜像可以通过分层来进行继承，基于基础镜像，可以制作各种具体应用镜像。

特性：与此同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

**Docker镜像加载原理**

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system)主要包含bootloader和kernel,bootloader主要是引导加载kernel,Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs，这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核，当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs(root file system)，在bootfs之上，包含的就是典型Linux系统中的/dev，/proc，/bin，/etc 等标准目录和文件，rootfs就是各种不同的操作系统发行版，比如Centos，Ubuntu等等。

![[Pasted image 20210822214650.png]]
平时我们安装进虚拟机的CentOS都是好几个G，为什么Docker这里才200M？
对于一个精简的OS，rootfs可以很小，只需要包含最基本的命令、工具和程序库就可以了，因为底层直接使用Host的Kernel，自己只需要提供rootfs就可以了，由此可见对于不同的Linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs。

**分层理解**
分层的镜像,我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层一层的在下载！
![[Pasted image 20210822214808.png]]
理解：所有的Docker镜像都起始于一个基础镜像层，当进行修改或增加新的内容时，就会在当前镜像层之上，创建新的镜像层，举一个简单的例子，假如基于Ubuntu Linux 16.04 创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加Python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创建第三个镜像层。

该镜像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。
![[Pasted image 20210822214858.png]]
在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要，下图中举了一个简单的例子，每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件。
![[Pasted image 20210822215005.png]]
上图中的镜像层跟之前图中的咯有区别，主要目的是便于展示文件

下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层的文件7是文件5的一个更新版本。
![[Pasted image 20210822215033.png]]
这种情况下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中。

Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统。

Linux上可以用的存储引擎有AUFS，Overlay2，Device Mapper，Btrfs以及ZFS。顾名思义，每种存储引擎都基于Linux中对应的

文件系统或者块设备技术，并且每种存储引擎都有其独有的性能特点。

Docker在Windows上仅支持windowsfilter一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW[1]

下图展示了与系统显示相同的三层镜像，所有镜像层堆叠并合并，对外提供统一的试图。
![[Pasted image 20210822215153.png]]
Docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部！
这一层就是我们通常说的容器层，容器之下的都叫镜像层！

## Commit镜像
---
```
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[TAG]
```
实战测试
```
# 1.启动一个默认的Tomcat

# 2.发现这个默认的tomcta 是没有webapps应用，镜像的原因，官方的镜像默认webapps下面是没有文件的

# 进入Tomcat容器 cp -r webapps.dist/* webapps
# 3.将容器中的webapps.dist文件夹下面的文件copy到webapps下

# 4.将我们操作过的容器通过commit提交为一个镜像！我们以后就是用我们修改过的镜像即可，这就是我们自己修改的镜像
```
![[Pasted image 20210823214620.png]]
如果你想要保存当前容器的状态，就可以通过commit来提交，获得一个镜像类似于VM的快照
## 容器数据卷的使用
---
docker的理念回想：

将应用和环境打包成一个镜像！数据？如果数据都在容器中，那么我们容器删除，数据就会丢失！需求：数据可以持久化
Mysql，容器删了，删库跑路！需求：Mysql数据可以存储在本地！
容器之间可以有一个数据共享的技术！Docker容器中产生的数据，同步到本地！
这就是卷技术！目录的挂载，将我们容器内的目录，挂载到Linux上面！
### 使用数据卷
```
直接使用命令来挂载 -v
docker run -it -v 主机目录:容器目录

# 测试
docker run -it -v /home/ceshi:/home centos /bin/bash
#启动起来时候我们可以通过 docker inspect 容器id
```
![[Pasted image 20210823215251.png]]
测试文件的同步

可以看到在容器中的/home目录中创建lvshihao.txt文件后，在主机上的/home/ceshi问价夹中也同步了lvshihao.txt文件

然后咱们把容器关掉，在主机上创建一个hello.txt文件，然后启动容器，可以看到容器的/home目录中也同步了主机创建的hello.txt文件

好处：我们以后修改只需要在本地修改即可，容器内会自动同步！
### 实战：Mysql同步数据
思考：MySQL的数据持久化的问题！
```
# 获取镜像
docker pull mysql:5.7

# 运行容器 需要做数据挂载 # 安装启动mysql ，需要配置密码的，这是要注意点！
# 官方测试。docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=mysql密码 -d mysql:tag

# 启动我们的
-d 后台运行
-p 端口映射
-v 卷挂载
-e 环境配置
--name 容器名称

docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

# 启动成功之后，我们在本地使用 navicat 来测试一下
# 连接服务器的3310端口

# 在本地测试创建一个数据库，查看一下我们映射的路径是否OK！

# 即使将容器删除，主机的数据也是不会丢失的！这就实现容器数据持久化功能！
```
### 具名和匿名挂载
```
# 匿名挂载  -v 容器内路径！
docker run -d -P --name nginx02 -v /ect/nginx nginx

# 查看所有的volume的情况
[root@localhost /]#docker volume ls
local     a55bcb5b5f89c6a5a983a9dcf64ee6565a764539b63c62392dd3cfe500f096ee
# 这里发现，这种就是匿名挂载，我们在 -v 只写了容器内的路径，没有写容器外的路径！

# 具名挂载 通过 -v 卷名:容器内路径
docker run -d -P --name nginx03 -v juming-nginx:/etc/nginx nginx

# 查看所有的volume的情况
[root@localhost /]# docker volume ls
DRIVER    VOLUME NAME
local     2263b103536e825ae90a197b365f0f1424a89134e20ab384201c203f1b27705d
local     a55bcb5b5f89c6a5a983a9dcf64ee6565a764539b63c62392dd3cfe500f096ee
local     juming-nginx

# 查看一下这个卷 通过docker volume inspect 卷名
```
![[Pasted image 20210823215812.png]]
所有的docker容器内的卷，没有指定目录的情况下都是在 **/var/lib/docker/volumes/xxxx/_data**

我们通过具名挂载可以方便的找到我们的一个卷，大多数情况使用的**具名挂载**

```
# 如何确定是具名挂载还是匿名挂载，还是指定路径挂载！
-v 容器内路径	#匿名挂载
-v 卷名:容器内路径	#具名挂载
-v /宿主机路径:容器内路径	#指定路径挂载

拓展:

# 通过 -v 容器内路径，ro  rw 改变读写权限
ro readonly #只读
rw readwrite #只读可写

# 一旦这个设置了容器权限，容器对我们挂载出来的内容就有限定了!
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:rw nginx

#ro 只要看到ro就说明这个路径只能通过宿主机来操作，容器内部是无法操作的！
```
## 数据卷之DockerFile
---
Dockerfile 就是用来构建docker镜像的构建文件！

通过这个脚本可以生成镜像，镜像是一层一层的，脚本一个个的命令，每个命令都是一层！
```
# 创建一个dockerfile文件，名字可以随机 建议 Dockerfile
# 文件中的内容 指令（大写）参数

FROM centos
VOLUME ["volume01","volume02]
CMD echo "----end----"
CMD /bin/bash

# 这里的每个命令，就是镜像的一层！
然后使用: docker build -f dockerfile文件 -t 构建的docker镜像名:tag dockerfile所在的目录位置

举个例子: docker build -f dockerfile1 -t lvshihao/centos:1.0 .
```
![[Pasted image 20210823220712.png]]
**启动自己写的容器**
![[Pasted image 20210823221023.png]]
**查看一下卷挂载的路径**
docker inspect 容器id 查看卷的挂载路径
![[Pasted image 20210823221049.png]]
测试一下在容器中创建文件是否能够同步出去！  
这种方式我们未来使用的十分多，因为我们通常会构建自己的镜像！
假设构建镜像时候没有挂载，需要手动镜像挂载 -v 卷名:容器内路径！

## 数据卷容器
---
```
# 使用咱们制作的docker镜像创建出3个容器

#--volumes-from 容器间传递共享
docker run -it --name centos01  lvshihao/centos:1.0
docker run -it --name centos02 --volumes-from centos01 lvshihao/centos:1.0
docker run -it --name centos03 --volumes-from centos01 lvshihao/centos:1.0

#测试，可以删除centos01，查看一下cnetos02和centos03是否还可以访问这个文件
#测试依旧可以访问
```
多个mysql实现数据共享
```
# 通过 -v将卷挂载到宿主机上
docker run -d -p 3310:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
# 通过 --volumes-from 将容器数据卷挂载到mysql01上实现容器之间数据同步
docker run -d -p 3310:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volumes-from mysql:5.7
```
容器之间配置信息的传递，数据卷的生命周期一直持续到没有容器使用它为止。

## DockerFile
---
### DockerFile介绍
dockerfile 是用来构建docker镜像的文件！命令参数脚本！
构建步骤：
+ 编写一个dockerfile文件
+ docker build 构建成为一个镜像
+ docker run 运行镜像
+ docker push 发布镜像（DockerHub、阿里云镜像仓库！）

![[Pasted image 20210824221311.png]]

![[Pasted image 20210824221701.png]]
很多官方镜像都是基础包，很多功能没有，我们通常会自己搭建自己的镜像！官方既然可以制作镜像，那我们也可以！
### DockerFile构建过程
**基础知识：**
+ 每个保留关键字（指令）都是必须是大写字母
+ 执行从上到下顺序执行
+ #表示注释
+ 每一个指令都会创建提交一个新的镜像层，并提交！


![[Pasted image 20210824223106.png]]
dockerfile是面向开发的，我们以后要发布项目，做镜像，就要编写dockerfile文件，这个文件十分简单！
Docker镜像逐渐成为企业交付的标注，必须要掌握！
DockerFile：构建文件，定义了一切的步骤，源代码
DockerImages：通过DockerFile构建生成的镜像，最终发布和运行的产品！
Docker容器：容器就是镜像运行起来提供服务
