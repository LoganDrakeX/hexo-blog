---
title: "gitblit从Windows迁移到Centos7"
date: 2021-6-11
categories: 配置记录
tags:
 - git
 - 配置

---

### 标准开头

记录一下将gitblit仓库从Windows迁移到Centos7的过程
<!--more-->

### 第一步：备份原仓库

将Windows下gitblit整个安装目录（包含下面文件的目录）打压缩包，防止丢失数据。

![](http://image.xiaocaiji.site/blog/QQ%E6%88%AA%E5%9B%BE20210611162526.png)

### 第二步：在Centos7上安装gitblit

#### 下载安装包

方法一：在Windows环境中下载好Centos7的gitblit安装包，[地址传送](https://github.com/gitblit/gitblit/releases/)，下载gitblit-1.9.1.tar.gz，并将压缩包传到Centos7上

方法二：直接在Centos7上执行命令，下载安装包

```sh
wget https://github.com/gitblit/gitblit/releases/download/v1.9.1/gitblit-1.9.1.tar.gz
```

#### 安装，安装位置 /opt/gitblit

```sh
tar -zxvf gitblit-1.9.1.tar.gz -C /opt/
cd /opt/
mv gitblit-1.9.1 gitblit
```

### 第三步：移植

用备份文件中data/defaults.properties替换掉/opt/gitblit/data/defaults.properties

用备份文件中data/users.conf替换掉/opt/gitblit/data/users.conf

用备份文件中data/git目录替换掉/opt/gitblit/data/git目录，此目录为仓库目录，名称不一定是git，在data/defaults.properties配置文件中查看git.repositoriesFolder 配置项

### 第四步：测试

在/opt/gitblit目录下运行

```sh
./gitblit.sh 
```

在本机访问[localhost:8080](localhost:8080)，若要使用局域网其他机器访问，需要执行命令`systemctl stop firewalld`关闭防火墙，使用`本机局域网IP:8080`访问

![](http://image.xiaocaiji.site/blog/QQ%E6%88%AA%E5%9B%BE20210611173320.png)

### 第五步：设置开机自启动

编辑文件service-centos.sh，修改端口GITBLIT_HTTP_PORT=8080，执行下面命令

```sh
./install-service-centos.sh
systemctl start gitblit.service  #启动服务
systemctl status gitblit.service  #查看服务状态
systemctl enable gitblit.service  #开机自启动
```



