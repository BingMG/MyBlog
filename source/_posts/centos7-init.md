---
title: Centos7 - 初始化服务器
categories:
- [Diary, PlayStation]
- [Diary, Games]
- [Life]
tags:
- PS3
- Games
photos:
- /2021/02/24/centos7-init/example.jpg
- /2021/02/24/centos7-init/example.jpg
- /2021/02/24/centos7-init/example.jpg

---

@(610 - Linux)



### 修改时间
```bash
# 查看当前时间
date "+%Y-%m-%d %H:%M:%S"

# 手动设置时间
date -s "20210101 00:00:00" 
```
<!-- more -->
### 更改密码
```bash
passwd
```
### 更改ssh端口号
确认当前ssh端口号
```bash
netstat -nlpt | grep ssh
```
更改ssh配置文件，并重启ssh服务
```bash
vi /etc/ssh/sshd_config

# 重启ssh服务
systemctl restart sshd.service
```
### yum更换国内源
安装或更新wget，防止命令未安装
```bash
yum install wget -y
```
备份默认yum源，并下载阿里源，更新缓存后进行系统更新操作
```bash
# 备份默认yum源
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

# 下载并替换yum源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

# 更新yum缓存
yum makecache

# 更新系统
yum update -y
```
### 安装Glances
Glances是一款简单大气的命令行性能监控软件，top工具替代品
```bash
yum install -y glances
```
### 安装Docker

清除可能存在的Docker安装包或遗留包
```bash
# 清除可能存在的Docker安装包或遗留包
sudo yum remove docker \
                docker-client \
                docker-client-latest \
                docker-common \
                docker-latest \
                docker-latest-logrotate \
                docker-logrotate \
                docker-engine
```

```bash
sudo yum install -y yum-utils \
                    device-mapper-persistent-data \
                    lvm2
```

执行安装
```bash 
# 添加yum的Docker源
sudo yum-config-manager \
     --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 执行Docker安装
sudo yum install -y docker-ce \
                    docker-ce-cli \
                    containerd.io

# 启动Docker服务
sudo systemctl start docker

# 运行测试镜像
sudo docker run hello-world

# 移除测试镜像

```

### Java安装
```bash
# 检查本机是否安装了openjdk
rpm -qa|grep openjdk -i

# 创建文件夹
mkdir /usr/java
mkdir /home/software

# 上传文件并解压
cd /home/software
tar -zxvf jdk-8u191-linux-x64.tar.gz
mv jdk1.8/ /usr/java/

# 添加环境变量
vim /etc/profile
export JAVA_HOME=/usr/java/jdk1.8
export CLASSPATH=.:%JAVA_HOME%/lib/dt.jar:%JAVA_HOME%/lib/tools.jar  
export PATH=$PATH:$JAVA_HOME/bin

# 刷新环境变量
source /etc/profile
```

### Nvm安装
```bash 
# 获取install.sh文件并执行
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash

# 刷新环境变量
source ~/.bash_profile

# 检查是否正确安装
nvm --version

# 查看nvm安装列表进行node.js安装
nvm list-remote
nvm install v12.20.1
nvm install v14.15.4

# 切换node.js版本并设置默认版本
nvm list
nvm use v12.20.1
nvm use v14.15.4
nvm alias default v14.15.4

#检查node.js是否正确安装
node -v
npm -v
```

```bash
# 安装yarn
npm install yarn -g

# 安装cgr换源工具
npm install -g cgr
# 测试仓库速度
cgr test
# 国内使用淘宝源
cgr use taobao
```

### 清除History记录
```bash
# 清除历史记录
history -c 
```