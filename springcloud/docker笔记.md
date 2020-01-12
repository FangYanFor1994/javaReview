一 、docker简介

#### 1.1docker能解决什么问题

```java
1.难题：
	软件开发中最麻烦的事之一，就是环境配置。软件从开发到上线，一般都要经过开发、测试、上线。而每个人的计算机环境配置可能都不相同，谁能保证自已开发的软件，能在每一台机器上跑起来？
每台机器必须保证：操作系统的设置，各种依赖和组件的安装都正确，软件才能运行起来。
比如，开发与部署一个 Java 应用，计算机必须安装 JDK，并配置环境变量，还必须有各种依赖。在windows上进行开发，到Linux上进行部署时这些环境又得重装，这样很就麻烦。而且系统如果需要集群，那每一台机器都得重新配置环境。
开发人员经常说："它在我的机器可以正常运行"，言下之意就是，其他机器很可能跑不了。环境配置如此麻烦，换一台机器，就要重来一次，费力费时。很多人想到，能不能从根本上解决问题，软件可以带环境安装？也就是说，安装的时候，把原始环境一模一样地复制过来。

2.解决：
	开发人员可以使用  Docker 来解决 "它在我的机器可以正常运行" 的问题，它会将运行程序的相关配置打包（打包成一个镜像），然后直接搬移到新的机器上运行。
```

#### 1.2什么是虚拟机技术

```java
虚拟机（virtual machine）就是带环境安装的一种解决方案。
它可以在一种操作系统里面运行另一种操作系统，比如 VMware workstation 虚拟化产品提供了虚拟的硬件，在Windows 系统里面运行 Linux 系统。安装在虚拟机（如Linux）上的应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统(如Windows)来说，虚拟机(如Linux)就是一个普通文件，不需要了就删掉，对
其他部分毫无影响。
```

![1578829968321](assets/1578829968321.png)

```java
缺点：虽然可以通过虚拟机还原软件需要的配置环境。但是虚拟机有几个缺点：
1.资源占用多
每个虚拟机会独占一部分内存和硬盘空间。哪怕虚拟机里面的应用程序，真正使用的内存只有 1MB，虚拟机依然需要几百 MB 的内存才能运行。
2.冗余步骤多
虚拟机是完整的操作系统，一些系统级别的操作步骤，往往无法跳过，比如用户登录。
3.启动慢
启动硬件上的操作系统需要多久，启动虚拟机就需要多久。可能要等几分钟，虚拟机才能真正运行。
```

#### 1.3什么是容器

![1578830220244](assets/1578830220244.png)

##### 1.3.1容器技术

由于虚拟机存在以上那些缺点，Linux发展出了另外一种虚拟化技术：Linux容器（Linux Containers,缩写为LXC）。

```java
容器与虚拟机有所不同，虚拟机通过虚拟软件中间层将一台或者多台独立的虚拟机器运行在物理硬件之上。而容器则是直接运行在操作系统内核之上，是进程级别的，并对进程进行了隔离，而不是模拟一个完整的操作系统。因此，容器虚拟化也被称为“操作系统级虚拟化”，容器技术可以将软件需要的环境配置都打包到一个隔离的容器中。让多个独立的容器高效且轻量的运行在同一台宿主机上。而Docker就是为了实现这一切而生的。
```

##### 1.3.2容器与虚拟机比较

1）本质上的区别：

![1578830469570](assets/1578830469570.png)

2）使用上的区别：

容器：体积小，启动快（秒级），资源占用少（只占用需要的资源），好评如潮。

![1578830544038](assets/1578830544038.png)

**虚拟机已死，容器才是未来！！！**

#### 1.4分析docker容器架构

![1578830605975](assets/1578830605975.png)

##### 1.4.1docker客户端和服务端

```java
docker是一个C/S架构程序。docker客户端只需要向docker服务器或者守护进程发出请求，服务器或者守护进程将完成所有工作并返回结果。docker提供了一个命令行工具和一整套RESTful API。你可以在同一台宿主机上运行docker守护进程和客户端，也可以从本地的docker客户端连接到运行在另一台宿主机上的远程docker守护进程。
```

![1578830831020](assets/1578830831020.png)

##### 1.4.2docker镜像（Image）

镜像（Image）是docker中的一个模板。通过docker镜像来创建docker容器，一个镜像可以创建出多个容器。镜像是由一系列指令一步一步构建出来的。

镜像与容器的关系类似于java中类与对象的关系。镜像体积很小非常“便携”，易于分享、存储和更新。![1578831018812](assets/1578831018812.png)

##### 1.4.3docker容器（Container）

```
容器（container）是基于镜像创建的运行实例，一个容器中可以运行一个或多个应用程序。
docker可以帮助你构建和部署容器，你只需要把自己的应用程序或者服务打包放进容器即可。
我们可以认为镜像是docker生命周期中的构建后者打包阶段，而容器则是启动或者执行阶段。
可以理解容器中有包含：一个精简版的Linux环境+要运行的应用程序。
```

##### 1.4.4docker仓库（repository）

```java
1.仓库（Repository）是集中存放镜像文件的场所。
2.有时候会把仓库（Repository）和仓库注册服务器（Registry）混为一谈，但并不严格区分。实际上，仓库注册服务器上往往存放着多个仓库，每个仓库中又包含了多个镜像，每个镜像有不同的标签（tag）。
3.仓库分为公有仓库（Public）和私有仓库（Private）两种。
  1）Docker公司运营的公共仓库叫做 Docker Hub （https://hub.docker.com/），存放了数量庞大的镜像供用户下载。用户可以在Docker Hub注册账号，分享并保存自己的镜像。（说明：在Docker Hub下载镜像巨慢）
  2）国内的公有仓库包括阿里云 、网易云 等，可以提供大陆用户更稳定快速的访问。
4.当用户创建了自己的镜像之后就可以使用 push 命令将它上传到公有或者私有仓库，这样下次在另外一台机器上使用这个镜像时候，只需要从仓库上 pull 下来就可以了。
```

docker仓库的概念和git类似，注册服务器可以理解为GitHub这样的托管服务器。

![1578831524598](assets/1578831524598.png)

### 二、VMware和Centos7安装

安装详解请见目录 "相关软件操作/centos.md"

### 三、docker安装卸载与启停

#### 3.1查看当前系统的内核版本

查看当前内核版本是否高于3.10

![1578834671088](assets/1578834671088.png)

#### 3.2安装docker服务

使用镜像仓库进行安装，采用yum命令在线安装（即电脑需要联网）

root用户运行以下命令：

1. 卸载旧版本：（如果安装过旧版本的话）

   Docker 的早期版本称为  docker 或  docker-engine 。如果安装了这些版本，请卸载它们及关联的依赖资源。

```properties
yum remove docker docker-common docker-selinux docker-engine
```

![1578834965511](assets/1578834965511.png)

2. 安装所需的软件包

```properties
yum-utils 提供了  yum-config-manager 实用程序，并且  devicemapper 存储驱动需要 device-mapper-persistent-data 和  lvm2 。
# yum install -y yum-utils device-mapper-persistent-data lvm2
```

![1578835360716](assets/1578835360716.png)

3. 设置docker的镜像仓库

```properties
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

上面可能会报错（原因是国内访问不到docker官方镜像的缘故）

![1578835944849](assets/1578835944849.png)

解决:使用以下方式：阿里源访问

```properties
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

![1578836036880](assets/1578836036880.png)

4. 安装最新版本docker CE

```properties
yum install docker-ce
```

![1578836352162](assets/1578836352162.png)

* 安装中出现下面提示，输入y后回车

![1578836385426](assets/1578836385426.png)

* 如用的docker国外网下载比较慢，当出现下面提示，输入y后回车

![1578836439989](assets/1578836439989.png)

5. 安装完成

![1578836459554](assets/1578836459554.png)

**如果上面安装不成功，则按3.3节卸载再按上面重新安装**

#### 3.3卸载docker服务

1. 卸载docker软件包

```properties
[root@mengxuegu /]# yum remove docker-ce
```

2.删除镜像、容器等 

```properties
[root@mengxuegu /]# rm -rf /var/lib/docker
```

#### 3.4启动与停止docker服务

* systemctl命令是系统服务管理器指令，它是service和chkconfig两个命令组合。

  ```properties
  1.启动docker： systemctl start docker
  2.停止docker： systemctl stop docker
  3.重启docker： systemctl restart docker
  4.查看docker状态： systemctl status docker
  5.开机自动启动docker： systemctl enable docker
  ```

#### 3.5docker版本查看

查看当前安装的docker版本

```properties
[root@mengxuegu /]# docker version
```

![1578839359930](assets/1578839359930.png)

#### 3.6docker帮助命令

* 查看docker帮助命令： docker --help

![1578839429991](assets/1578839429991.png)

* 查看docker概要信息： docker info

![1578839463330](assets/1578839463330.png)