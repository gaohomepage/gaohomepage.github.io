---
title: Singularity Intro
date: 2023-12-02 23:53:06
tags:
    - Singularity
    - Tech
---
# Singularity：在无sudo权限的情况下运行docker

docker简化了环境配置，但是在服务器集群上，由于缺乏sudo权限或者docker未安装，无法直接使用docker，和实验室的小伙伴交流得知singularity可以起到类似的功能

### Reference

1. [HPC使用singularity](https://www.xiexianbin.cn/hpc/singularity/index.html)
2. [singularity日常使用](http://www.xtaohub.com/Container-Tech/Singularity-in-nutshell.html)
3. [singularity介绍](https://www.xiexianbin.cn/hpc/singularity/index.html)

### Intro

Singularity中包含如下重要概念

1. 容器：指的是包含软件和依赖的镜像，不同于docker区分容器和镜像（类似进程和程序），singularity中的容器既可以是静态的也可以是动态的
2. SIF：只读的Singularity镜像
3. Sandbox：可写的容器存在形式，在文件系统中是一个目录，用于开发和创建自身的容器
4. 定义文件：.def文件，用于生成沙箱和镜像

### 在HPC集群中安装Singularity

主要参考 [无root权限安装singularity](https://www.shumlab.com/tech/try-singularity/)，注意config时上--without-suid参数

## 踩坑

### Unsquash找不到

需要修改`installpath/etc/singularity/`目录下的`singularity.conf`文件中mksquashfs path为编译的squashfs目录

### SetUID权限

Linux文件的权限包含r(可读)w(可写)x(可执行)，还有s权限，这适用于可执行文件

> s权限允许当用户拥有对文件的可执行权限时，执行文件时成为文件的拥有者，执行结束则切换回原来的身份
>
> 临时获取root权限以操作对应的文件

这个问题解决需要`cat /proc/sys/user/max_user_namespaces`，如果为0则系统不支持

## Singularity使用

### 创建容器

1. 从外部源下载，利用`singularity pull/build`从外部容器仓库下载到本地为SIF文件
   1. URI://从容器库构建
   2. docker://从Docker Hub构建
   3. shub://从Singularity Hub构建
2. 创建可写的沙箱目录
   1. Singularity镜像是只读的，需要写入可以使用--sandbox参数

> 可读/可写并非针对绑定目录而言，而是对于docker系统的根目录而言，使用sandbox创建镜像时如果添加--writeable则容器中的所有目录都是可写的，想要创造可写容器只能对sandbox创建容器时添加--writeable选项

#### 创建沙箱目录

```shell
singularity build --sandbox sandboxpath imagepath
```

#### 进入Shell环境

```shell
singularity shell imagepath/sandboxpath
```

#### 从定义文件生成镜像

```shell
singularity build build imagepath deffilepath
```

### 映射

`--bind`选项允许我们将目录挂载到容器内，挂载的目录一定是可写的

```shell
singularity shell -B sourcedir:mountdir imagepath
```

