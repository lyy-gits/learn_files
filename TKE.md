# TKE容器创建Nginx

## 创建集群

### 1.创建集群

+ 填写集群名
+ 确认所在地域
+ 确认pod数量
+ 操作系统发行版本

![image-20210202160614095](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202160614095.png)

+ 创建容器网络
  每个pod存放结单63个计算方式:
  - 如果cidr值是20（网络位），32-20 = 12（主机位）， 2^ 12 =4096  （可容纳主机）
  - 4096 - 32 (集群内Server数量上限) = 4064
  - 每个pod上限4096 / 64 = 63 (63.5)

### 2.购买服务器

确认好Master节点和购买服务器的计费模式

![image-20210202161239400](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202161239400.png)

### 3.其他设置

![image-20210202161351052](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202161351052.png)

### 4.确认配置

按业务所需选择安装的组件

![image-20210202161516264](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202161516264.png)

### 5.成功创建

![image-20210202162821003](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202162821003.png)

## 拉取Nginx容器

在集群上拉取Nginx容器

### 1.创建Nginx镜像

![image-20210202163150883](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202163150883.png)

### 2.拉取Nginx镜像

![image-20210202163326025](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202163326025.png)

此处选择的是Docker Hub镜像

![image-20210202163353354](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202163353354.png)

### 3.映射端口

+ 服务访问方式
+ 端口映射

![image-20210202163511905](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202163511905.png)

### 4.service中查找访问路径

![image-20210202164358437](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202164358437.png)

访问出口地址

![image-20210202164437351](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202164437351.png)

# 手工搭建helloworld服务

## 创建镜像仓库

### 1.创建镜像仓库

+ 新建命名空间
+ 新建镜像仓库
+ 重置密码

新建命名空间

![image-20210202170159720](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202170159720.png)

新建镜像仓库

![image-20210202170254705](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202170254705.png)

私有镜像仓库

![image-20210202170321023](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202170321023.png)

重置密码

![image-20210202170408286](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202170408286.png)

### 2.制作镜像

+ 编写应用程序

```java
mkdir hellonode
cd hellonode
vim server.js
```

写入测试代码

```java
var http = require('http');
var handleRequest = function(request, response) {
console.log('Received request for URL: ' + request.url);
response.writeHead(200);
response.end('Hello World!');
};
var www = http.createServer(handleRequest);
www.listen(80);
```

本机测试

```java
node server.js
curl 127.0.0.1:80
```

+ 构建镜像

通过docker构建镜像

```java
cd /hellonode
vim Dockerfile
```

编写DockerFile

```java
FROM node:4.4
EXPOSE 80
COPY server.js .
CMD node server.js
```

构建镜像

```java
docker build -t hello-node:v1 .
```

查看构建结果

```java
docker images
```

![image-20210202174418555](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202174418555.png)

上传镜像到腾讯云镜像仓库，先打标签

```java
# imageid:323c2d698688
docker tag 323c2d698688 ccr.ccs.tencentyun.com/lyytest/helloworld:v1
#登录仓库
docker login --username=100014009655 ccr.ccs.tencentyun.com/lyytest/myhub
#上传镜像
 docker push ccr.ccs.tencentyun.com/lyytest/helloworld:v1
```

上传成功

![image-20210202195257051](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202195257051.png)

控制台查看

![image-20210202195535696](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202195535696.png)

## 通过镜像创建hello world服务

![image-20210202195820866](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202195820866.png)

![image-20210203092058800](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210203092058800.png)

选择新建的个人镜像

![image-20210202195905295](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202195905295.png)

映射容器与负载均衡的端口

![image-20210202195932636](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210202195932636.png)

构建成功的运行状态

![image-20210203092232846](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210203092232846.png)

测试访问

![image-20210203092338201](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210203092338201.png)

![image-20210203092359537](C:\Users\v_lyyoli\AppData\Roaming\Typora\typora-user-images\image-20210203092359537.png)