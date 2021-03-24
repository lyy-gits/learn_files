## Docker概述

### 1.**docker为什么出现？**

一款产品：开发--上线  两套环境 应用环境，应用配置

开发 .... 运维  问题：我在我的电脑上可以运行！版本更新导致服务不可用！对于运维来说		考验十分大

环境配置十分麻烦，每一个机器都要部署环境（集群redis、ES、Hadoop。。） 费时费力

发布一个项目 （jar+（redis  mysql  jdk  ES））项目能不能带上环境安装打包

之前在服务器配置一个应用的环境  Redis  MySQL  jdk  配置麻烦不能跨平台

windows  最后发布linux

传统：开发jar  运维来做

现在：开发打包部署上线，一套流程做完

java---apk----发布（应用商店）---张三使用apk----安装即可用

java---jar（环境） ----打包项目带上环境（镜像）-----（Docker仓库：商店）----下载发布的镜像---直接运行即可

Docker给以上的问题，提出了解决方案

![img](https://docimg5.docs.qq.com/image/TX_3_Gm5hyCwbbh0ew0Gpw?w=200&h=58)

Docker的思想就来自于集装箱

JRE---多个应用（端口冲突）----原来都是交叉的

隔离：Docker核心思想  打包装箱  每个箱子是互相隔离的

水果  生化武器  （隔离）

Docker通过隔离机制，可以将服务器利用到极致

本质：所有的技术都是因为出现了一些问题，我们需要去解决，才去学习

### 2.**Docker的历史**

2010年，几个搞IT的年轻人，在美国成立了一家公司‘dotCloud’

做一些pass的云计算服务     虚拟机有关的容器技术

他们将自己的技术 （ 容器化技术） 命名就是Docker

Docker刚刚诞生的时候，没有引起行业的注意  dotCloud活不下去

开源

开放源代码

2013年，Docker开源

Docker越来越多的人发现了docker的优点！火了  Docker每个月都会更新一个版本

2014年4月9日，Docker1.0发布

#### **Docker为什么这么火，十分的轻巧**

在容器技术出来之前  （VM）使用的都是虚拟机技术

虚拟机：在windows中装一个VMware，通过这个软件我们可以虚拟出来一台或者多台电脑，但是比较笨重

虚拟机也是属于虚拟化技术，Docker容器技术，也是一种虚拟化技术

eg：

vm，linux centos原生镜像（一个电脑）  隔离，需要开启多个虚拟机   几个G

docker，隔离，镜像（最核心的环境4m+jdk+mysql）十分的小巧，运行镜像就可以了   几个M  KB   秒级启动

到现在，所有开发人员必须要会Docker

### 3.**聊聊Docker**

Docker是基于GO语言开发的  开源项目

官网地址：https://www.docker.com/

![img](https://docimg8.docs.qq.com/image/hbkFHLJyjGV4MFMq572ibA?w=1202&h=519)

文档地址：https://docs.docker.com/   Docker的文档超级详细

仓库地址：https://hub.docker.com/   git push  git pull

### 4.**Docker能做什么**

#### (1)**虚拟机技术**

![img](https://docimg3.docs.qq.com/image/OiSwOkP_3nrTPIrt6ZsR4w?w=633&h=504)

虚拟机技术缺点：

1. 资源占用十分多
2. 冗余步骤多
3. 启动慢

#### (2)**容器化技术**

**容器化技术不是模拟的一个完整的操作系统**

![img](https://docimg1.docs.qq.com/image/7hAxUMuhXswwgv0JfTmoIw?w=703&h=611)

**比较Docker和虚拟机技术的不同：**

传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件

容器内的应用直接运行在宿主机的内核，容器是没有自己的内核的，也没有虚拟硬件，所以就轻便了

每个容器间是互相隔离的，每个容器内都有一个属于自己的文件系统，互不影响

### 5.Docker的优势

Docker 相比于传统虚拟化方式具有更多的优势：

- docker 启动快速属于秒级别。虚拟机通常需要几分钟去启动。
- docker 需要的资源更少， docker 在操作系统级别进行虚拟化， docker 容器和内核交互，几乎没有性能损耗，性能优于通过 Hypervisor 层与内核层的虚拟化。
- docker 更轻量， docker 的架构可以共用一个内核与共享应用程序库，所占内存极小。同样的硬件环境， Docker 运行的镜像数远多于虚拟机数量，对系统的利用率非常高。
- 与虚拟机相比， docker 隔离性更弱， docker 属于进程之间的隔离，虚拟机可实现系统级别隔离。
- 安全性： docker 的安全性也更弱。 Docker 的租户 root 和宿主机 root 等同，一旦容器内的用户从普通用户权限提升为 root 权限，它就直接具备了宿主机的 root 权限，进而可进行无限制的操作。虚拟机租户 root 权限和宿主机的 root 虚拟机权限是分离的，并且虚拟机利用如 Intel 的 VT-d 和 VT-x 的 ring-1 硬件隔离技术，这种隔离技术可以防止虚拟机突破和彼此交互，而容器至今还没有任何形式的硬件隔离，这使得容器容易受到攻击。
- 可管理性： docker 的集中化管理工具还不算成熟。各种虚拟化技术都有成熟的管理工具，例如 VMware vCenter 提供完备的虚拟机管理能力。
- 高可用和可恢复性： docker 对业务的高可用支持是通过快速重新部署实现的。虚拟化具备负载均衡，高可用，容错，迁移和数据保护等经过生产实践检验的成熟保障机制， VMware 可承诺虚拟机 99.999% 高可用，保证业务连续性。
- 快速创建、删除：虚拟化创建是分钟级别的， Docker 容器创建是秒级别的， Docker 的快速迭代性，决定了无论是开发、测试、部署都可以节约大量时间。
- 交付、部署：虚拟机可以通过镜像实现环境交付的一致性，但镜像分发无法体系化。 Docker 在 Dockerfile 中记录了容器构建过程，可在集群中实现快速分发和快速部署。

### 6.**DevOps（开发、运维）**

**应用更快速的交付和部署**

传统：一堆帮助文档，安装程序

Docker：打包镜像发布测试，一键运行

**更便捷的升级和扩缩容**

使用了Docker之后，我们部署应用就和搭积木一样

项目打包为一个镜像，扩展服务器A

**更简单的系统运维**

在容器化之后，我们的开发，测试环境都是高度一致的

**更高效的计算资源利用**

Docker是内核级别的虚拟化，可以在物理机上可以运行很多的容器实例，服务器的性质可以被压榨到极致

## Docker安装

### **Docker的基本组成**

![img](https://docimg4.docs.qq.com/image/xiQgBZ0Z2_gvwjRUDHm6dQ?w=764&h=359)

**镜像（image）：**

docker镜像就好比是一个模板，可以通过这个模板来创建容器服务，tomcat镜像---run---tomcat01  容器（提供服务器），通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）

**容器（container）：**

Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建的

启动，停止，删除，基本命令！

目前就可以把这个容器理解为一个简易的linux系统

**仓库（Registry）：**

仓库就是存放镜像的地方

仓库分为公有仓库和私有仓库

Docker Hub（默认是国外的）

阿里云、腾讯云...都有容器服务器（配置镜像加速）

### **安装Docker**

#### 1.**环境准备**

```shell
1. 需要会一点点的Linux的基础
2. CentOS7
3. 连接远程服务器
```

#### 2.**环境查看**

```shell
#系统内核是3.10以上的
[root@VM-0-19-centos ~]# uname -r
3.10.107-1-tlinux2_kvm_guest-0052
#系统版本
[root@VM-0-19-centos ~]# cat /etc/os-release 
NAME="Tencent tlinux"
VERSION="2.2 (Final)"
ID="tlinux"
ID_LIKE="rhel fedora centos"
VERSION_ID="2.2"
PRETTY_NAME="Tencent tlinux 2.2 (Final)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:tlinux:linux:2"
HOME_URL="http://tlinux.oa.com/"
BUG_REPORT_URL="http://tapd.oa.com/tlinux/bugtrace/bugreports/my_view/"
```

#### 3.**安装**

`Docker` 分为 CE 和 EE 两大版本。CE 即社区版（免费，支持周期 7 个月）， EE 即企业版，强调安全，付费使用，支持周期 24 个月。

帮助文档：https://docs.docker.com/engine/install/centos/

```shell
#1.卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
#2.直接安装docker
yum install docker-io -y
#3.设置加速镜像源
vim /etc/docker/daemon.json
{
"registry-mirrors": [
  "https://mirror.ccs.tencentyun.com"
]
}
#更新yum软件包索引
#就是把服务器的包信息下载到本地电脑缓存起来，makecache 建立一个缓存。以后用 install 时就在缓存中搜索，提高了速度。配合yum -C search xxx使用。不用上网检索就能查找软件信息。我们在更新 yum 源或者配置 yum 源之后，通常都会使用 yum makecache 更新 yum 缓存。 yum clean all 需要定期清理缓存。
yum makecache fast

#4.启动docker
systemctl start docker
#5.使用docker version是否安装成功
#6.hello-world
docker run hello-world  （不存在自动pull）
#7.查看下载的hello-world镜像
docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
docker.io/hello-world   latest              bf756fb1ae65        11 months ago       13.3 kB
```

#### 4.卸载docker

```shell
#1.卸载依赖
yum remove  docker
#2.删除资源
rm -rf /var/lib/docker/

#/var/lib/docker/  docker的默认工作路径
```

#### 5.底层原理

##### 回顾HelloWorld流程

![img](http://rscl.site/assets/img/2-4.c55cac73.png)

##### docker run流程图

![img](http://rscl.site/assets/img/2-5.4eaf634b.png)

##### Docker是怎么工作的

![img](http://rscl.site/assets/img/2-6.f6fd8b26.png)

Docker 是一个 `Client-Server` 结构的系统，Docker 的守护进程运行在主机上。通过 Socket 从客户端访问！

`Docker-Server` 接收到 `Docker-Client` 的指令，就会执行这个命令！

Docker 客户端是 Docker 用户与 Docker 交互的主要方式。当您使用 docker 命令行运行命令时， Docker 客户端将这些命令发送给服务器端，服务端将执行这些命令。 docker 命令使用 docker API 。 Docker 客户端可以与多个服务端进行通信。

##### 为什么Docker比VM快

- 1、docker 有着比虚拟机更少的抽象层。由于 docker 不需要 Hypervisor 实现硬件资源虚拟化，运行在 docker 容器上的程序直接使用的都是实际物理机的硬件资源。因此在 CPU、内存利用率上 docker 将会在效率上有明显优势。

- 2、docker 利用的是宿主机的内核，而不需要 Guest OS。

  ![img](data:image/png;base64,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)

所以说，当新建一个容器时，docker 不需要和虚拟机一样重新加载一个操作系统内核。仍而避免引导、加载操作系统内核返个比较费时费资源的过程，当新建一个虚拟机时，虚拟机软件需要加载 GuestOS，返个新建过程是分钟级别的。而 docker 由于直接利用宿主机的操作系统，则省略了这个复杂的过程，因此新建一个 docker 容器只需要几秒钟。

> GuestOS： VM（虚拟机）里的的系统（OS）;

> HostOS：物理机里的系统（OS）；

#### 6.Docker组件是如何协作运行容器的

现在我们再通过 `hello-world` 这个例子来体会一下 Docker 各个组件是如何协作的。

容器启动过程如下：

- `Docker` 客户端执行 `docker run` 命令
- `Docker daemon` 发现本地没有 `hello-world` 镜像
- `daemon` 从 `Docker Hub` 下载镜像
- 下载完成，镜像 `hello-world` 被保存到本地
- `Docker daemon` 启动容器

具体过程可以看如下这幅演示图：

![img](http://rscl.site/assets/img/2-27.8218a08f.jpg)

我们可以通过 `docker images` 可以查看到 `hello-world` 已经下载到本地

![img](http://rscl.site/assets/img/2-28.b80676a8.jpg)

我们可以通过 `docker ps` 显示正在运行的容器，我们可以看到， `hello-world` 在输出提示信息以后就会停止运行，容器自动终止，所以我们在查看的时候没有发现有容器在运行。

![img](http://rscl.site/assets/img/2-29.ae2f15d5.png)

我们把 `Docker` 容器的工作流程剖析的十分清楚了，我们大体可以知道 `Docker` 组件协作运行容器可以分为以下几个过程：

- `Docker` 客户端执行 `docker run` 命令
- `Docker daemon` 发现本地没有我们需要的镜像
- `daemon` 从 `Docker Hub` 下载镜像
- 下载完成后，镜像被保存到本地
- `Docker daemon` 启动容器

## Docker常用命令

### 1.帮助命令

```sh
docker version     #显示docker的版本信息。
docker info        #显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help #帮助命令
```

帮助文档的地址：https://docs.docker.com/engine/reference/commandline/build/

### 2. 镜像命令

```sh
docker images   #查看所有本地主机上的镜像 可以使用docker image ls代替

docker search   #搜索镜像

docker pull     #下载镜像 docker image pull

docker rmi      #删除镜像 docker image rm
```

`docker images` 查看所有本地的主机上的镜像

```sh
docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
mysql                 5.7                 e73346bdf465        24 hours ago        448MB

# 解释
#REPOSITORY      # 镜像的仓库源
#TAG             # 镜像的标签
#IMAGE ID        # 镜像的id
#CREATED         # 镜像的创建时间
#SIZE            # 镜像的大小
# 可选项
Options:
  -a, --all             Show all images (default hides intermediate images) #列出所有镜像
  -q, --quiet           Only show numeric IDs # 只显示镜像的id

docker images -aq ＃显示所有镜像的id
e73346bdf465
d03312117bb0
```

`docker search` 搜索镜像

```sh
docker search mysql
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   9500                [OK]
mariadb                           MariaDB is a community-developed fork of MyS…   3444                [OK]
```

```sh
Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
# --filter=STARS=3000 #搜索出来的镜像就是STARS大于3000的
docker search mysql --filter=STARS=3000
NAME                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql               MySQL is a widely used, open-source relation…   9500                [OK]
mariadb             MariaDB is a community-developed fork of MyS…   3444                [OK]
```

`docker pull` 下载镜像

```sh
# 下载镜像 docker pull 镜像名[:tag]
docker pull tomcat:8
8: Pulling from library/tomcat #如果不写tag，默认就是latest
90fe46dd8199: Already exists   #分层下载： docker image 的核心 联合文件系统
35a4f1977689: Already exists
bbc37f14aded: Already exists
74e27dc593d4: Already exists
93a01fbfad7f: Already exists
1478df405869: Pull complete
64f0dd11682b: Pull complete
68ff4e050d11: Pull complete
f576086003cf: Pull complete
3b72593ce10e: Pull complete
Digest: sha256:0c6234e7ec9d10ab32c06423ab829b32e3183ba5bf2620ee66de866df640a027  # 签名 防伪
Status: Downloaded newer image for tomcat:8
docker.io/library/tomcat:8 #真实地址

#等价于
docker pull tomcat:8
docker pull docker.io/library/tomcat:8
```

`docker rmi` 删除镜像

```sh
docker rmi -f 镜像id                       #删除指定的镜像
docker rmi -f 镜像id 镜像id 镜像id 镜像id    #删除多个镜像
docker rmi -f $(docker images -aq)        #删除全部的镜像
```

### 3.容器命令

> 说明：我们有了镜像才可以创建容器，Linux，下载 centos 镜像来学习

```sh
docker pull centos
```

新建容器并启动

```sh
docker run [可选参数] image | docker container run [可选参数] image
#参数说明
--name="Name"    容器名字 tomcat01 tomcat02 用来区分容器
-d               后台方式运行
-it              使用交互方式运行，进入容器查看内容
-p               指定容器的端口 -p 8080(宿主机):8080(容器)
      -p ip:主机端口:容器端口
      -p 主机端口:容器端口(常用)
      -p 容器端口
      容器端口
-P(大写)          随机指定端口
```

```sh
# 测试、启动并进入容器
[root@rscl ~]# docker run -it centos /bin/bash
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
8a29a15cefae: Already exists
Digest: sha256:fe8d824220415eed5477b63addf40fb06c3b049404242b31982106ac204f6700
Status: Downloaded newer image for centos:latest

[root@ef0df438ed8c /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@ef0df438ed8c /]# exit #从容器退回主机
exit
[root@rscl ~]# ls /
bin  boot  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

列出所有运行的容器

```sh
#docker ps命令
  #列出当前正在运行的容器
  -a, --all             显示所有的容器，包括当前运行的和停止的容器
  -n, --last int        显示最近创建的容器
  -q, --quiet           只显示容器的编号

docker ps

docker ps -a

docker ps -aq
```

退出容器

```sh
exit         #容器直接退出
Ctrl + P + Q #容器不停止退出
```

删除容器

```sh
docker rm 容器id                  #删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm -f
docker rm -f $(docker ps -aq)    #删除所有的容器
docker ps -a -q|xargs docker rm  #删除所有的容器
```

启动和停止容器的操作

```sh
docker start 容器id     #启动容器
docker restart 容器id   #重启容器
docker stop 容器id      #停止当前正在运行的容器
docker kill 容器id      #强制停止当前容器
```

### 4.常用其他命令

后台启动命令

```sh
# 命令 docker run -d 镜像名
docker run -d centos
a8f922c255859622ac45ce3a535b7a0e8253329be4756ed6e32265d2dd2fac6c
docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
# 问题docker ps的时候，发现centos 停止了
# 常见的坑，docker 容器使用后台运行，就必须要有要一个前台进程，docker发现没有应用，就会自动停止
# nginx，容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了
```

查看日志

```sh
docker logs --help
Options:
      --details        Show extra details provided to logs
*  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
*      --tail string    Number of lines to show from the end of the logs (default "all")
*  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
```

```sh
#显示日志
-f, --follow         #跟踪实时日志
-t, --timestamps     #显示时间戳
--tail  n            #仅列出最新N条容器日志

docker logs f -t --tail n 容器id #查看n行日志
```

```sh
docker logs -t --tail 10 3475812637aa  #发现这个容器没有日志，自己编写一段shell脚本，模拟日志
docker run -d centos /bin/sh -c "while true;do echo kuangshen;sleep 1;done"
```

查看容器中进程信息 ps

```sh
# 命令 docker top 容器id
```

查看镜像的元数据

```sh
# 命令
docker inspect 容器id

#测试
[root@rscl ~]# docker inspect 55321bcae33d
```

进入当前正在运行的容器

```sh
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 命令
docker exec -it 容器id bashshell

#测试
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
55321bcae33d        centos              "/bin/sh -c 'while t…"   10 minutes ago      Up 10 minutes                           bold_bell

docker exec -it 55321bcae33d /bin/bash
```

```sh
# 方式二
docker attach 容器id
#测试
docker attach 55321bcae33d
正在执行当前的代码...
(这里会死循环，Ctrl + P + Q退出)

区别
#docker exec   #进入当前容器后开启一个新的终端，可以在里面操作。（常用）
#docker attach # 进入容器正在执行的终端
```

从容器内拷贝文件到主机上

```sh
#查看当前主机目录
[root@rscl home]# ls

#进入docker容器内部
[root@rscl home]# docker ps -a
[root@rscl home]# docker attach  763e9bfccc47
[root@763e9bfccc47 /]# cd /home
#在容器内新建一个文件
[root@763e9bfccc47 /home]# touch test.java
[root@763e9bfccc47 /home]# exit


# 将文件拷贝到主机上
# docker cp 容器 id:容器内路径 主机目的路径
[root@rscl home]# docker cp 763e9bfccc47:/home/test.java  /home
[root@rscl home]# ls   #可以看见test.java存在
```

> 拷贝是一个手动过程，未来我们使用数据卷技术可以实现自动同步 `/home` `/home`

### 5.常用命令小结

![img](http://rscl.site/assets/img/3-1.7c71c7c8.png)

```sh
attach      Attach local standard input, output, and error streams to a running container
#当前shell下 attach连接指定运行的镜像
build       Build an image from a Dockerfile      # 通过Dockerfile定制镜像
commit      Create a new image from a container's changes #提交当前容器为新的镜像
cp          Copy files/folders between a container and the local filesystem #从容器中拷贝指定文件或者目录到宿主机中
create      Create a new container                #创建一个新的容器，同run，但不启动容器
diff        Inspect changes to files or directories on a container's filesystem #查看docker容器的变化
events      Get real time events from the server  # 从docker服务获取容器实时事件
exec        Run a command in a running container  # 在运行中的容器上运行命令
export      Export a container's filesystem as a tar archive #导出容器文件系统作为一个tar归档文件[对应import]
history     Show the history of an image # 展示一个镜像形成历史
images      List images                  #列出系统当前的镜像
import      Import the contents from a tarball to create a filesystem image #从tar包中导入内容创建一个文件系统镜像
info        Display system-wide information # 显示全系统信息
inspect     Return low-level information on Docker objects #查看容器详细信息
kill        Kill one or more running containers # kill指定docker容器
load        Load an image from a tar archive or STDIN #从一个tar包或标准输入中加载一个镜像[对应 save]
login       Log in to a Docker registry    # 注册或登录一个 docker 源服务器
logout      Log out from a Docker registry # 从当前 registry 退出
logs        Fetch the logs of a container  # 输出当前容器日志信息
pause       Pause all processes within one or more containers # 暂停容器
port        List port mappings or a specific mapping for the container # 查看映射端口对应的容器内部源端口
ps          List containers                # 列出容器列表
pull        Pull an image or a repository from a registry # 从docker镜像源服务器拉取指定镜像或者库镜像
push        Push an image or a repository to a registry   # 推送指定镜像或者库镜像至docker源服务器
rename      Rename a container
restart     Restart one or more containers   # 重启运行的容器
rm          Remove one or more containers    # 移除一个或者多个容器
rmi         Remove one or more images        # 移除一个或者多个镜像
run         Run a command in a new container # 创建一个新的容器并运行一个命令
save        Save one or more images to a tar archive (streamed to STDOUT by default) # 保存一个镜像为一个tar包[对应 load]
search      Search the Docker Hub for images     # 在docker hub中搜索镜像
start       Start one or more stopped containers # 启动容器
stop        Stop one or more running containers  # 停止容器
tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE # 给源中镜像打标签
top         Display the running processes of a container          # 查看容器中运行的进程信息
unpause     Unpause all processes within one or more containers   # 取消暂停容器
version     Show the Docker version information                   # 查看 docker 版本号
wait        Block until one or more containers stop, then print their exit codes # 截取容器停止时的退出状态值
```

## 练习

### 练习 1：Docker 安装 Nginx

- 1. 搜索镜像 search 建议大家去 docker 搜索，可以看到帮助文档
-    2.拉取镜像 pull
-    3.运行测试

```sh
docker pull nginx

# -d 后台运行
# --name 给容器命名
# -p 宿主机端口：容器内部端口
docker run -d --name nginx01 -p 3344:80 nginx
docker ps
curl localhost:3344   #测试
```

> 思考问题：我们每次改动 nginx 配置文件，都需要进入容器内部？十分的麻烦，要是可以在容器外部提供一个映射路径，达到在容器外部修改文件，容器内部就可以自动修改？ -v 数据卷

### 练习 2：docker 来装一个 tomcat

```sh
# 官方的使用
docker run -it --rm tomcat:9.0
# 我们之前的启动都是后台，停止了容器之后，容器还是可以查到， docker run -it --rm image 一般是用来测试，用完就删除
--rm       Automatically remove the container when it exits

#下载再启动
docker pull tomcat
#启动运行
docker run -d -p 8080:8080 --name tomcat01 tomcat
#测试访问有没有问题
curl localhost:8080

#进入容器
[root@rscl home]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
db09851cf82e        tomcat              "catalina.sh run"   28 seconds ago      Up 27 seconds       0.0.0.0:8080->8080/tcp   tomcat01
[root@rscl home]# docker exec -it db09851cf82e /bin/bash
root@5abe6704a5cf:/usr/local/tomcat#

# 发现问题：
# 1、linux命令少了。
# 2.没有webapps，默认是最小的镜像，所有不必要的都剔除掉。保证最小可运行环境。
```

> 思考问题：我们以后要部署项目，如果每次都要进入容器是不是十分麻烦？要是可以在容器外部提供一个映射路径，webapps，我们在外部放置项目，就自动同步到内部就好了！

### 练习 3：部署 es+kibana

- es 暴露的端口很多！
- es 十分的耗内存
- es 的数据一般需要放置到安全目录！挂载

```sh
# --net somenetwork ? 网络配置
# 启动elasticsearch
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2
# 启动了 Linux 就卡住了
# es是十分耗内存的
docker stats # 查看docker容器资源消耗情况
```

![img](data:image/png;base64,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)

```sh
#赶紧关闭
docker rm -f d73ad2f22dd3

#添加内存的限制，修改配置文件 -e 环境配置修改
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
```

![img](data:image/png;base64,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)

测试一下 es 是否成功启动

```sh
[root@rscl home]# curl localhost:9200
{
  "name" : "d73ad2f22dd3",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "atFKgANxS8CzgIyCB8PGxA",
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

### 练习4：使用 kibana 连接 es？思考网络如何才能连接

![img](http://rscl.site/assets/img/4-4.927a977b.png)

### 可视化

portainer

```sh
docker run -d -p 8080:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

Rancher

**什么是 portainer？**

Docker 图形化界面管理工具！提供一个后台面板供我们操作！

```sh
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

测试访问：外网 8088 端口 `http://ip:8088`

首次登陆需要注册用户，给 admin 用户设置密码：

![img](http://rscl.site/assets/img/4-5.dd9f093c.png)

单机版这里选择 local 即可，选择完毕，点击 Connect 即可连接到本地 docker：

![img](http://rscl.site/assets/img/4-6.d7f537e8.png)

进入之后的面板

![img](http://rscl.site/assets/img/4-7.5e5e8c49.png)

## Docker镜像讲解

### 镜像是什么?

镜像是一种轻量级、可执行的独立软件，用来打包软件运行环境和基于运行环境开发的软件，他包含运行某个软件所需的所有内容，包括代码、运行时库、环境变量和配置文件。

**如何得到镜像**

- 从远程仓库下载
- 拷贝
- 自己制作一个镜像 DockerFile

### Docker 镜像加载原理

UnionFs（联合文件系统）：Union 文件系统（UnionFs）是一种分层、轻量级并且高性能的文件系统，他支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下（ unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

> UnionFs（联合文件系统）

docker 的镜像实际上由一层一层的文件系统组成，这种层级的文件系统 UnionFS。

boots(boot file system）主要包含 bootloader 和 Kernel, bootloader 主要是引导加 kernel, Linux 刚启动时会加 bootfs 文件系统，在 Docker 镜像的最底层是 boots。这一层与我们典型的 Linux/Unix 系统是一样的，包含 boot 加載器和内核。当 boot 加载完成之后整个内核就都在内存中了，此时内存的使用权已由 bootfs 转交给内核，此时系统也会卸载 bootfs。

rootfs（root file system),在 bootfs 之上。包含的就是典型 Linux 系统中的/dev,/proc,/bin,/etc 等标准目录和文件。 rootfs 就是各种不同的操作系统发行版，比如 Ubuntu, Centos 等等。

![img](http://rscl.site/assets/img/5-1.7184d381.png)

**平时我们安装进虚拟机的 CentOS 都是好几个 G，为什么 Docker 这里才 200M？**

![img](data:image/png;base64,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)

对于个精简的 OS, rootfs 可以很小，只需要包合最基本的命令，工具和程序库就可以了，因为底层直接用 Host 的 kernel，自己只需要提供 rootfs 就可以了。由此可见对于不同的 Linux 发行版， boots 基本是一致的， rootfs 会有差別，因此不同的发行版可以公用 bootfs.

虚拟机是分钟级别，容器是秒级！

### 分层理解

> 分层的镜像

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层一层的在下载

![image-20201218141537072](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218141537072.png)

思考：为什么Docker镜像要采用这种分层的结构呢？

最大的好处，莫过于是资源的共享了。比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份Base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享

查看镜像分层的方式可以通过docker image inspect命令

```shell
[root@VM-0-19-centos ~]# docker image inspect redis
{
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:87c8a1d8f54f3aa4e05569e8919397b65056aa71cdf48b7f061432c98475eee9",
                "sha256:25075874ce886bd3adb3b75298622e6297c3f893e169f18703019e4abc8f13f0",
                "sha256:caafc8119413c94f1e4b888128e2f337505fb57e217931a9b3a2cd7968340a9e",
                "sha256:e5d940a579ec4a80b6ec8571cb0fecf640dba14ccfd6de352977fd379a254053",
                "sha256:2a1c28c532d20c3b8af8634d72a4d276a67ce5acb6d186ac937c13bd6493c972",
                "sha256:1540b8226044ed5ce19cc0fec7fbfb36a00bb15f4e882d6affbd147a48249574"
            ]
        }
    }
]
```

理解：

所有的Docker镜像都起始于一个基础镜像层，当进行修改或增加新的内容时，就会在当前镜像层之上，创建新的镜像层。

举一个简单的例子，例如基于Ubuntu Linux 16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加Python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创建第三个镜像层。

该镜像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）

![image-20201218142455945](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218142455945.png)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要，下图中举了一个简单的例子，每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件

![image-20201218142733896](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218142733896.png)

上图中的镜像层跟之前图中的略有区别，主要目的是便于展示文件

下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版本

![image-20201218142918368](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218142918368.png)

![image-20201218143811480](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218143811480.png)

这种情况下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个镜像层添加到镜像当中。

Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统。

Linux上可用的存储引擎有AUFS、Overlay2、Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于Linux中对应的文件系统或者块设备技术，并且每种存储引擎都有其独有的性能特点。

Docker在Windows上仅支持windowsfilter一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和COW

下图展示了与系统显示相同的三层镜像，所有镜像层堆叠并合并，对外提供统一的视图

![image-20201218144811970](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218144811970.png)

> 特点

Docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部

这一层就是我们通常说的容器层，容器之下的都叫镜像层



![image-20201218150036602](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218150036602.png)

如何提交自己的镜像

### commit镜像

```shell
docker commit  提交容器成为一个新的副本
#命令和git原理类似
docker commit -m="提交的描述信息" -a="作者" 容器id  目标镜像名,[TAG]
```

测试

```shell
#1.启动默认的tomcat

#2.发现这个默认的tomcat是没有webapps应用，镜像的原因，官方的镜像默认webapps下是没有文件的

#3.拷贝webapps.dist下的文件到webapps下

#4.将操作过的容器通过commit提交为一个镜像，以后使用修改过的镜像即可。
[root@VM-0-19-centos ~]# docker commit -a='lyy' -m='add webapps app' ee tomcat02:1.0
```

![image-20201218151650688](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218151650688.png)

```shell
如果想要保存当前容器的状态，就可以通过commit来提交，获得一个镜像
相当于学习VM时快照的作用
```

## 容器数据卷

### 什么是容器数据卷

docker的理念回顾

将应用和环境打包成一个镜像

如果数据都在容器中，那么删除容器，数据就会丢失  (需求：数据持久化)

MySQL，容器删除    （需求：MySQL数据可以存储在本地）

容器之间可以有一个数据共享的技术。Docker容器中产生的数据，同步到本地

这既是卷技术，目录的挂载，将容器内的目录，挂载到Linux上面

![image-20201218153821010](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218153821010.png)

**总结：容器的持久化和同步操作！容器间也是可以数据共享**

### 使用数据卷

> 方式一：直接使用命令来挂载  -v

```shell
 docker run -it -v 主机目录:容器内目录
 #测试
 [root@VM-0-19-centos /home]# docker run -it -v /home/test:/home centos
 
 #启动后可以通过docker inspect 容器id  查看挂载情况  （双向绑定）
```

![image-20201218154724120](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218154724120.png)

测试文件的同步

![image-20201218154953810](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218154953810.png)

测试

1.停止容器

2.宿主机上修改文件

3.启动容器

4.容器内的数据依旧是同步的

![image-20201218155729730](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218155729730.png)

作用：以后修改只需要在本地修改即可，容器内会自动同步

### 实战：安装MySQL

思考：MySQL的数据持久化问题

```shell
#获取镜像
[root@VM-0-19-centos /home]# docker pull mysql:5.7
#运行容器，需要做数据挂载  安装启动mysql，需要配置密码
#官方测试  docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag  

#启动
-d  后台运行
-p  端口映射
-v  数据卷挂载
-e  环境配置
--name  容器名字
[root@VM-0-19-centos /home]# docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7


#启动成功之后，本地使用sqlyog来测试一下
#sqlyog  连接到服务器的3310  ---->3310和容器3306映射，可以连接上

#在本地测试创建数据库，查看映射的路径是否OK
```

![image-20201218172156949](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218172156949.png)

查看宿主机和容器内是否同步新创建的数据库，同步成功

![image-20201218172541962](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218172541962.png)

假设将容器删除，发现挂载到本地的数据卷依旧没有丢失，这就实现了容器数据持久化功能

![image-20201218172927625](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218172927625.png)

![image-20201218172937656](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218172937656.png)

### 具名和匿名挂载

```shell
#匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx

#查看多有的volume的情况
[root@VM-0-19-centos ~]# docker volume ls
DRIVER              VOLUME NAME
local               586fb107c1f608356dc2fe071b02a14ae48be9759701d66e8ce1e5c14d020404
#以上就是匿名挂载，-v只写了容器内的路径，没有写容器外的路径

#具名挂载
[root@VM-0-19-centos ~]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx 
21ca7a9a8691c265d2f064c7cf9f44101673442a50c96e36e60128f83ed7f562
[root@VM-0-19-centos ~]# docker volume ls
DRIVER              VOLUME NAME
local               586fb107c1f608356dc2fe071b02a14ae48be9759701d66e8ce1e5c14d020404
local               8adb7d0791ba412d80e293ac11e9c549f14c0dab2ce8cc1c873b0250e4da877a
local               juming-nginx
#通过 -v 卷名：容器内路径
#查看卷
```

![image-20201218184346025](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218184346025.png)

所有的docker容器内的卷，没有指定目录的情况下都是在/var/lib/docker/volumes/xxx/_data

通过具名挂载可以方便的找到卷，大多数情况使用的是“具名挂载”

```shell
#如何确定是具名挂载还是匿名挂载，还是指定路径挂载
-v 容器内路径   #匿名
-v 卷名：容器内路径	#具名挂载
-v /宿主机路径：容器内路径  #指定路径挂载
```

拓展

```shell
#通过 -v 容器内路径，ro rw 改变读写权限
ro  只读
rw	读写
#一旦设置了容器权限，容器对挂载出来的内容就有限定了
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:rw nginx

#ro  只能通过宿主机来操作，容器内部是没有权限操作的
```

### 初识Dockerfile

Dockerfile就是用来构建docker镜像的构建文件（命令脚本）

通过脚本生成镜像，镜像是一层一层的，脚本是一个一个的命令，每个命令都是一层

```shell
#创建一个dockerfile文件，名字可以随机，建议dockerfile
#文件中的内容  指令（大写）  参数
FROM centos

VOLUME ["volume01","valume02"]   #匿名挂载

CMD echo "...end..."
CMD /bin/bash
#这里每个命令，就是镜像的一层
```

![image-20201218190756146](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218190756146.png)

```shell
#启动自己创建的容器
```

![image-20201218191316594](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218191316594.png)

这个卷和宿主机一定有一个同步的目录

![image-20201218191720168](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218191720168.png)

查看卷挂载路径

```shell
[root@VM-0-19-centos ~]# docker inspect e
```

![image-20201218191819413](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218191819413.png)

测试刚才的文件是否同步

这种方式使用频率非常高，会构建自己的镜像

假设构建镜像时没有挂载卷，要手动挂载  -v 卷名 ：容器内路径

> 容器内新建的文件

![image-20201218191850418](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218191850418.png)

> 查看宿主机是否同步

![image-20201218192017371](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218192017371.png)

### 数据卷容器

多个mysql同步数据

![image-20201218192807484](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218192807484.png)

```shell
#启动3个容器，通过自己创建的镜像启动
```

![image-20201218193105046](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218193105046.png)

启动docker02执行挂载操作，在docker01创建文件后，查看docker02是否同步

![image-20201218193519578](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218193519578.png)

![image-20201218193535420](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218193535420.png)

![image-20201218194221907](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218194221907.png)

```shell
#测试，删除docker01查看docker02和docker03是否还可以访问这个文件
#测试结果依旧可以访问这个文件
```

多个mysql实现数据共享

```shell
[root@VM-0-19-centos /home]# docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
[root@VM-0-19-centos /home]# docker run -d -p 3310:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volume-from mysql01 mysql:5.7
#可以实现两个容器数据同步
```

结论：

容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止

但是一旦持久化到了本地，这个时候，本地的数据是不会删除的

## DockerFile

### Dockerfile介绍

dockerfile是用来构建docker镜像的文件

构建步骤：

1、编写一个dockerfile文件

2、docker build 构建为一个镜像

3、docker run 运行镜像

4、docker push 发布镜像（DockerHub、镜像仓库）

官方做法

![image-20201218195845853](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218195845853.png)

![image-20201218195941139](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218195941139.png)

很多官方镜像都是基础包，很多功能没有，通常会搭建自己的镜像

### DockerFile构建

**基础知识：**

1.每个保留关键字（指令）都是必须是大写字母

2.执行从上到下顺序执行

3.#表示注释

4.每一个指令都会创建提交一个新的镜像层并提交

![image-20201218200439003](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201218200439003.png)

dockerfile是面向开发的，我们以后要发布项目，制作镜像就需要编写dockerfile文件

步骤：开发、部署、运维

Docker镜像逐渐成为企业交付的标准

DockerFile：构建文件，定义了一切的步骤，源代码

DockerImages：通过DockerFile构建生成的镜像，最终发布和运行的产品，原来是jar war

Docker容器：容器就是镜像运行起来提供服务

### Docker指令

```shell
FROM 		#基础镜像，一切从这里构建  centos
MAINTAINER	#镜像是谁写的，姓名+邮箱
RUN			#镜像构建的时候需要运行的命令
ADD			#步骤，tomcat镜像，这个tomcat的压缩包，添加内容
WORKDIR		#镜像的工作目录   
VOLUME		#挂载的目录
EXPOSE		#保留端口配置
CMD			#指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT	#指定这个容器启动的时候要运行的命令，可以追加命令
INBUILD		#当构建一个被继承DockerFile这个时候就会运行ONBUILD的指令，触发指令
COPY		#类似ADD，将文件拷贝到镜像中
ENV			#构建设置环境变量
```

### 实战测试

Docker Hub中99%镜像都是从这个基础镜像过来的 FROM scratch，然后配置需要的软件和配置来进行的构建

https://github.com/CentOS/sig-cloud-instance-images/blob/b2d195220e1c5b181427c3172829c23ab9cd27eb/docker/Dockerfile

![image-20201221104139260](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221104139260.png)

> 创建一个自己的centos

```shell
#1.编写dockerfile的文件
[root@VM-0-19-centos /home/dockerfile]# cat mydockerfile-centos 
FROM centos
MAINTAINER lyy<lyy_log@163.com>
ENV MYPATH /usr/local
WORKDIR $MYPATH 
RUN yum -y install vim
RUN yum -y install net-tools
EXPOSE 80
CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
#2.通过这个文件构建镜像
#命令docker build -f dockerfile文件路径  -t 镜像名：[tag]
Successfully built a969b90976a7
#3.测试运行

```

对比：原生的centos

![image-20201221110924866](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221110924866.png)

增加之后的镜像

![image-20201221111022999](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221111022999.png)

列出本地镜像的变更历史

![image-20201221113313498](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221113313498.png)

拿到一个镜像，可以研究一下是怎么构建的

> CMD和ENTRYPOINT区别

```shell
CMD			#指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT	#指定这个容器启动的时候要运行的命令，可以追加命令
```

测试cmd

```shell
#编写dockerfile文件
[root@VM-0-19-centos /home/dockerfile]# vim dockerfile-cmd-test
FROM centos
CMD ["ls","-a"]
#构建镜像
[root@VM-0-19-centos /home/dockerfile]# docker build -f dockerfile-cmd-test -t cmdtest .
#run运行，ls -a命令生效
[root@VM-0-19-centos /home/dockerfile]# docker run -it cmdtest
.   .dockerenv	dev  home  lib64       media  opt   root  sbin	sys  usr
..  bin		etc  lib   lost+found  mnt    proc  run   srv	tmp  var
#想追加一个命令 -l  ls -al
[root@VM-0-19-centos /home/dockerfile]# docker run -it cmdtest -l
/usr/bin/docker-current: Error response from daemon: oci runtime error: container_linux.go:235: starting container process caused "exec: \"-l\": executable file not found in $PATH".
#cmd的清理下  -l替换了 CMD["ls","-a"]命令  -l不是命令所以报错
```

测试ENTRYPOINT

```shell
[root@VM-0-19-centos /home/dockerfile]# vim dockerfile-cmd-entrypoint
FROM centos
ENTRYPOINT ["ls","-a"]
[root@VM-0-19-centos /home/dockerfile]# docker build -f dockerfile-cmd-entrypoint -t entrypoint-test .
Sending build context to Docker daemon 4.096 kB
Step 1/2 : FROM centos
 ---> 300e315adb2f
Step 2/2 : ENTRYPOINT ls -a
 ---> Running in 715a7af5e0e8
 ---> 433f0cd8dfd2
Removing intermediate container 715a7af5e0e8
Successfully built 433f0cd8dfd2
[root@VM-0-19-centos /home/dockerfile]# docker run 43
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
#追加命令是直接拼接在ENTPYPOINT之后的
[root@VM-0-19-centos /home/dockerfile]# docker run 43 -l
total 56
drwxr-xr-x   1 root root 4096 Dec 21 04:02 .
drwxr-xr-x   1 root root 4096 Dec 21 04:02 ..
-rwxr-xr-x   1 root root    0 Dec 21 04:02 .dockerenv
lrwxrwxrwx   1 root root    7 Nov  3 15:22 bin -> usr/bin
drwxr-xr-x   5 root root  340 Dec 21 04:02 dev
drwxr-xr-x   1 root root 4096 Dec 21 04:02 etc
drwxr-xr-x   2 root root 4096 Nov  3 15:22 home
lrwxrwxrwx   1 root root    7 Nov  3 15:22 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Nov  3 15:22 lib64 -> usr/lib64
drwx------   2 root root 4096 Dec  4 17:37 lost+found
drwxr-xr-x   2 root root 4096 Nov  3 15:22 media
drwxr-xr-x   2 root root 4096 Nov  3 15:22 mnt
drwxr-xr-x   2 root root 4096 Nov  3 15:22 opt
dr-xr-xr-x 152 root root    0 Dec 21 04:02 proc
dr-xr-x---   2 root root 4096 Dec  4 17:37 root
drwxr-xr-x   1 root root 4096 Dec 21 04:02 run
lrwxrwxrwx   1 root root    8 Nov  3 15:22 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Nov  3 15:22 srv
dr-xr-xr-x  12 root root    0 Dec 21 04:02 sys
drwxrwxrwt   7 root root 4096 Dec  4 17:37 tmp
drwxr-xr-x  12 root root 4096 Dec  4 17:37 usr
drwxr-xr-x  20 root root 4096 Dec  4 17:37 var
```

Dockerfile中很多命令都十分的相似，我们需要了解他们的区别，最好的学习就是对比然后测试效果

### 实战：Tomcat镜像

1.准备镜像文件 tomcat压缩包，jdk的压缩包

2.编写dockerfile文件，官方命名Dockerfile，build会自动寻找这个文件，就不需要-f指定了

```shell
FROM centos
MAINTAINER lyy<lyy_log@163.com>
COPY readme.txt /usr/local/readme.txt
ADD jdk-8u131-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.22.tar.gz /usr/local
RUN yum -y install vim
ENV MYPATH /usr/local
WORKDIR $MYPATH
ENV JAVA_HOME /usr/local/jdk1.8.0_131
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.22
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.22
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
EXPOSE 8080
CMD /usr/local/apache-tomcat-9.0.22/bin/startup.sh && -F /usr/local//usr/local/apache-tomcat-9.0.22/bin/logs/catalina.out
```

3.构建镜像

```shell
#通过docker build -t diytomcat .
```

4.启动镜像

5.访问测试

6.发布项目（由于做了卷挂载,我们直接在本地编写项目就可以发布了）

```shell
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.4" 
    xmlns="http://java.sun.com/xml/ns/j2ee" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
        http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
</web-app>
```

```shell
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>hello,lyy(runoob.com)</title>
</head>
<body>
<p>
   今天的日期是: <%= (new java.util.Date()).toLocaleString()%>
</p>
</body> 
</html> 
```

项目部署成功，可以直接访问

http://159.75.20.7:9090/test/

### 发布自己的镜像

> DockerHub

1.地址https://hub.docker.com/注册自己的账号

2.确定这个账号可以登录

3.在服务器上提交自己的镜像

```shell
[root@VM-0-19-centos ~/tomcat/tomcatlog]# docker login --help

Usage:	docker login [OPTIONS] [SERVER]

Log in to a Docker registry

Options:
      --help              Print usage
  -p, --password string   Password
  -u, --username string   Username
  [root@VM-0-19-centos ~]# docker login -u lyyoli
Password: 
Login Succeeded

```

4.登录完毕后就可以提交镜像了，docker push

```shell
#push自己的镜像到服务器上
[root@VM-0-19-centos ~]# docker push lyy/diytomcat:1.0
The push refers to a repository [docker.io/lyy/diytomcat]
2925961b2287: Preparing 
ff868a2dd349: Preparing 
10cc743807dd: Preparing 
1fbf9364d72a: Preparing 
2653d992f4ef: Preparing 
denied: requested access to the resource is denied   #拒绝

#push镜像问题
[root@VM-0-19-centos ~]# docker push lyyy/diytomcat:1.0
The push refers to a repository [docker.io/lyyy/diytomcat]
An image does not exist locally with the tag: docker.io/lyyy/diytomcat

#解决：增加一个tag（这里要注意一个问题,给自己镜像命名的时候格式应该是: docker注册用户名/镜像名,比如docker用户名为 test123,那么我的镜像tag就为 test123/whalesay,不然是push不上去的）
[root@VM-0-19-centos ~]# docker tag diytomcat:latest lyy/diytomcat:1.0

#docker push上去即可，自己发布的镜像带上版本号
[root@VM-0-19-centos ~]# docker push lyyoli/diytomcat:1.0
The push refers to a repository [docker.io/lyyoli/diytomcat]
2925961b2287: Pushing 7.084 MB/58 MB
ff868a2dd349: Pushing 4.009 MB/15.41 MB
10cc743807dd: Pushing 5.488 MB/376.1 MB
1fbf9364d72a: Retrying in 1 second 
2653d992f4ef: Pushing 6.028 MB/209.3 MB
```

![image-20201221162145109](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221162145109.png)

提交的时候也是按照镜像的层级来进行提交的

> 发布到腾讯云镜像服务

腾讯云容器镜像参考腾讯云官方文档地址

### 小结

![image-20201221164105329](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221164105329.png)

```shell
#保存镜像到本地
[root@VM-0-19-centos ~]# docker save redis -o redis.tar
#导入镜像
[root@VM-0-19-centos ~]# docker load -i redis.tar
```

## Docker网络

### 理解Docker0网络

清空所有环境

> 测试

![image-20201221165450321](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221165450321.png)

三个网络

```shell
#问题，docker是如何处理容器网络访问的？
```

![image-20201221165634650](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221165634650.png)

```shell
[root@VM-0-19-centos ~]# docker run -d -P --name tomcat01 tomcat

#查看容器的内部网络地址   ip addr   发现容器启动的时候会得到一个eth0 ip地址，是docker分配的
[root@VM-0-19-centos ~]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
136: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:acff:fe11:2/64 scope link 
       valid_lft forever preferred_lft forever
       
#思考：linux是否可以ping通容器内部
[root@VM-0-19-centos ~]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.048 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.028 ms

#linux可以ping通docker容器内部
```

> 原理

1.每启动一个docker容器，docker就会给docker容器分配一个ip，只要安装了docker，就会有一个网卡docker0

桥接模式，使用的技术是veth pair技术

启动容器后再次测试ip addr 

![image-20201221175415861](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221175415861.png)

2.再启动一个容器测试，发现又多了一对网卡

![image-20201221190216505](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221190216505.png)

```shell
#我们发现这个容器的网卡，都是一对一对出现的
#veth pair就是一对的虚拟设备接口，他们都是成对出现的，一段连着协议，一段彼此相连
#正因为有这个特性，veth pair充当一个桥梁，连接各种虚拟网络设备的
#Openstack，Docker容器之间的连接，OVS的连接，都是使用veth pair技术
```

3.测试tomcat01是否和tomcat02可以ping通

```shell
[root@VM-0-19-centos ~]# docker exec -it tomcat02 ping 172.17.0.2
#结论：容器和容器之间是可以互相ping通的
```

绘制网络图

![image-20201221191839076](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221191839076.png)

结论：tomcat01和tomcat02是公用的一个路由器，docker0

所有的容器不指定网络的情况下，都是docker0路由的，docker会给我们的容器分配一个默认的可用ip

> 小结

Docker使用的是Linux的桥接，宿主机中是一个Docker容器的网桥 docker0

![image-20201221193032038](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221193032038.png)

Docker中的所有的网络接口都是虚拟的，虚拟的转发效率高（内网传递文件）

只要容器删除，对应的一对网桥也随之删除

![image-20201221194339002](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221194339002.png)

### --link

> 思考一个场景，编写了一个微服务，database url=ip：项目重启，数据库ip换掉了，希望可以通过名字来进行访问容器

```shell
[root@VM-0-19-centos ~]# docker exec -it tomcat02 ping tomcat01
ping: tomcat01: Name or service not known

#如何解决呢？

#通过--link可以解决网络连通问题（--link就是在/etc/host文件中加了一行）
[root@VM-0-19-centos ~]# docker run -d -P --name tomcat03 --link tomcat02 tomcat
[root@VM-0-19-centos ~]# docker exec -it tomcat03 ping tomcat02
PING tomcat02 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcat02 (172.17.0.3): icmp_seq=1 ttl=64 time=0.065 ms
64 bytes from tomcat02 (172.17.0.3): icmp_seq=2 ttl=64 time=0.046 ms

#反向可以ping通吗？  
[root@VM-0-19-centos ~]# docker exec -it tomcat02 ping tomcat03
ping: tomcat03: Name or service not known
```

探究：inspect

![image-20201221194442832](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221194442832.png)

tomcat03在本地配置了docker02的配置

```shell
#方式一
[root@VM-0-19-centos ~]# docker inspect tomcat03 | grep tomcat02 -n
60:                "/tomcat02:/tomcat03/tomcat02"

#方式二
[root@VM-0-19-centos ~]# docker exec -it tomcat03 cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.3	tomcat02 53f7559b3d3a
172.17.0.4	08f025c8b3b6

```

![image-20201221195038753](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221195038753.png)

本质探究：--link就是在hosts配置中增加了一个172.17.0.3  tomcat02 53f7559b3d3a

现在docker已经不建议使用--link

自定义网络，不使用docker0

docker0问题：不支持容器名连接访问

### 自定义网络

容器互联

> 查看所有的docker网络

![image-20201221195431208](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221195431208.png)

**网络模式**

bridge：桥接 docker（默认，自己创建也是用bridge模式）

none：不配置网络

host：主机模式（和宿主机共享网络）

container：容器内网络连通（用得少，局限很大）

**测试**

```shell
#我们直接启动的命令 --net bridge 而这个就是我们的docker0
docker run -d -P --name tomcat01 tomcat
docker run -d -P --name tomcat01 --net bridge tomcat

#docker0特点，默认，域名不能访问  --link可以打通连接

#自定义网络
#--driver  桥接bridge
#--subnet  子网  192.168.0.0/16  192.168.0.2~192.168.255.255
#--gateway 网关
[root@VM-0-19-centos ~]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet

[root@VM-0-19-centos ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
2fb8a57dd4ba        bridge              bridge              local
a1e2cfe13d9d        host                host                local
834fc9892382        mynet               bridge              local
cd5298e57574        none                null                local

```

自定义网络创建成功

![image-20201221200338692](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221200338692.png)

通过mynet网络启动容器

```shell
[root@VM-0-19-centos ~]# docker run -d -P --name tomcat-net-02 --net mynet tomcat
```

![image-20201221200502819](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201221200502819.png)

```shell
#再次测试ping连接
[root@VM-0-19-centos ~]# docker exec -it tomcat-net-01 ping 192.168.0.3
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
64 bytes from 192.168.0.3: icmp_seq=1 ttl=64 time=0.073 ms
64 bytes from 192.168.0.3: icmp_seq=2 ttl=64 time=0.047 ms

#现在不使用--link也可以ping名字了
[root@VM-0-19-centos ~]# docker exec -it tomcat-net-01 ping tomcat-net-02
PING tomcat-net-02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.047 ms
64 bytes from tomcat-net-02.mynet (192.168.0.3): icmp_seq=2 ttl=64 time=0.046 ms
```

自定义网络docker都已经帮我们维护好了对应的关系，推荐使用自定义网络

好处：

redis  不同集群使用不同网络，保证集群是安全和健康的

mysql  不同集群使用不同网络，保证集群是安全和健康的

### 网络连通

![image-20201222092050954](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201222092050954.png)

![image-20201222091803085](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201222091803085.png)

![image-20201222091819190](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201222091819190.png)

```shell
#测试打通  tomcat01---->mynet
[root@VM-0-19-centos ~]# docker network connect mynet tomcat01

#连通之后就是将tomcat01放到了mynet网络下
[root@VM-0-19-centos ~]# docker network inspect mynet

#一个容器两个ip地址
#服务器：公网ip  内网ip

```

![image-20201222092402351](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201222092402351.png)

```shell
#tomcat01连通ok
[root@VM-0-19-centos ~]# docker exec -it tomcat01 ping tomcat-net-01
PING tomcat-net-01 (192.168.0.2) 56(84) bytes of data.
64 bytes from tomcat-net-01.mynet (192.168.0.2): icmp_seq=1 ttl=64 time=0.071 ms
64 bytes from tomcat-net-01.mynet (192.168.0.2): icmp_seq=2 ttl=64 time=0.049 ms
#tomcat02是依旧打不通的
[root@VM-0-19-centos ~]# docker exec -it tomcat02 ping tomcat-net-01
ping: tomcat-net-01: Name or service not known
```

结论：假设要跨网络操作别人，就需要使用docker network connect 连通

### 实战：部署Redis集群

![image-20201222092956191](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201222092956191.png)

创建一个自定义网络

```shell
[root@VM-0-19-centos ~]# docker network create redis --subnet 172.38.0.0/16

#查看网络
[root@VM-0-19-centos ~]# docker network inspect redis
```

shell脚本启动redis

```shell
for port in $(seq 1 6); \
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
port 6379 
bind 0.0.0.0
cluster-enabled yes 
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done

 docker run -p 6371:6379 -p 16371:16379 --name redis-1 \
    -v /mydata/redis/node-1/data:/data1 \
    -v /mydata/redis/node-1/conf/redis.conf:/etc/redis/redis.conf \
    -d --net redis --ip 172.38.0.11 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
    
 docker run -p 6376:6379 -p 16376:16379 --name redis-6 \
    -v /mydata/redis/node-6/data:/data1 \
    -v /mydata/redis/node-6/conf/redis.conf:/etc/redis/redis.conf \
    -d --net redis --ip 172.38.0.16 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
    
#进入redis
[root@VM-0-19-centos ~]# docker exec -it redis-1 /bin/sh    （redis里没有/bin/bash）

#创建集群 （--cluster-replicas  指定一个master下有几个slave）
/data # redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172
.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1 
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.38.0.15:6379 to 172.38.0.11:6379
Adding replica 172.38.0.16:6379 to 172.38.0.12:6379
Adding replica 172.38.0.14:6379 to 172.38.0.13:6379
M: e9c575c60bb88d03c0525670dedbb3affbe7cfb8 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
M: e1ef42ed7b4b79c020d3d93c3ac21bab4cde87c0 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
M: 5ec1a712ec14da115781560e0a61930ae917ffa0 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
S: eeee17f327b899f16ea8c4aa7b76ae50d9b69e11 172.38.0.14:6379
   replicates 5ec1a712ec14da115781560e0a61930ae917ffa0
S: d1c329e59c30f0775d3f3f4e5d201bf1f559d3df 172.38.0.15:6379
   replicates e9c575c60bb88d03c0525670dedbb3affbe7cfb8
S: fee781c857afe62338b71dc34c6a1ea89c71175e 172.38.0.16:6379
   replicates e1ef42ed7b4b79c020d3d93c3ac21bab4cde87c0
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
...
>>> Performing Cluster Check (using node 172.38.0.11:6379)
M: e9c575c60bb88d03c0525670dedbb3affbe7cfb8 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: fee781c857afe62338b71dc34c6a1ea89c71175e 172.38.0.16:6379
   slots: (0 slots) slave
   replicates e1ef42ed7b4b79c020d3d93c3ac21bab4cde87c0
M: e1ef42ed7b4b79c020d3d93c3ac21bab4cde87c0 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
M: 5ec1a712ec14da115781560e0a61930ae917ffa0 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: eeee17f327b899f16ea8c4aa7b76ae50d9b69e11 172.38.0.14:6379
   slots: (0 slots) slave
   replicates 5ec1a712ec14da115781560e0a61930ae917ffa0
S: d1c329e59c30f0775d3f3f4e5d201bf1f559d3df 172.38.0.15:6379
   slots: (0 slots) slave
   replicates e9c575c60bb88d03c0525670dedbb3affbe7cfb8
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.


```

```shell
#进入集群
/data # redis-cli -c
#查看集群信息
127.0.0.1:6379> cluster info
127.0.0.1:6379> cluster nodes
#测试存储数据,显示存储在了13上
127.0.0.1:6379> set a b
-> Redirected to slot [15495] located at 172.38.0.13:6379
OK
#停掉13对应的docker
[root@VM-0-19-centos ~]# docker stop redis-3
#在集群获取a   14顶替了13
127.0.0.1:6379> get a
-> Redirected to slot [15495] located at 172.38.0.14:6379
"b"
#查看集群情况
172.38.0.14:6379> cluster nodes
```

![image-20201222095900870](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20201222095900870.png)

使用docker之后，所有技术都会慢慢变得简单起来

### SpringBoot微服务打包Docker镜像

1.构架springboot项目

2.打包应用

3.编写dockerfile

4.构建镜像

5.发布运行