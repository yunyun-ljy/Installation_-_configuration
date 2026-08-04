# centos安装

&nbsp;Docker 目前支持 CentOS 及以后的版本 系统的要求跟 Ubuntu 情况类似， 64 位操作 系统，内核版本至少为 3.10

首先，为了方便添加软件源，以及支持 devicemapper 存储类型，安装如下软件包：

## 最小化安装整体命令:

\`\`\`shell

### 关闭防火墙

systemctl stop firewalld

systemctl disable firewalld.service

### 关闭selinux

vi /etc/selinux/config

修改SELINUX=disabled

#1.拷贝一份新的阿里云的 下载源 到 /etc/yum.repos.d/下

先导入CentOS-Base.repo

mv /root/CentOS-Base.repo /etc/yum.repos.d/

yes覆盖

#2.清空原下载池

sudo yum clean all

#3. 加载新源

sudo yum makecache

添加阿里yum源

若运行脚本则跳过

yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

### 安装工具包

yum install -y epel-release

### 安装基础软件

yum install -y net-tools rsync vim wget ntp

#安装gcc

yum -y install gcc gcc-c++ libstdc++-devel

yum install -y yum-utils device-mapper-persistent-data lvm2

yum clean all

yum makecache

若运行脚本则跳过

#  安装docker

若运行脚本则跳过

&nbsp;yum -y install docker-ce

bash get-docker.sh --mirror Aliyun

&nbsp;

&nbsp;#4.启动docker

systemctl enable --now docker

无报错则安装成功

&nbsp;

&nbsp;#5.镜像加速

sudo mkdir -p /etc/docker

vim /etc/docker/daemon.json

写入以下内容

{

"registry-mirrors": \[

"https://dockerhub.icu",

"https://docker.registry.cyou",

"https://docker-cf.registry.cyou",

"https://dockercf.jsdelivr.fyi",

"https://docker.jsdelivr.fyi",

"https://dockertest.jsdelivr.fyi",

"https://mirror.aliyuncs.com",

"https://dockerproxy.com",

"https://mirror.baidubce.com",

"https://docker.m.daocloud.io",

"https://docker.nju.edu.cn",

"https://docker.mirrors.sjtug.sjtu.edu.cn",

"https://docker.mirrors.ustc.edu.cn",

"https://mirror.iscas.ac.cn",

"https://docker.rainbond.cc"

\]

}

systemctl daemon-reload

systemctl restart docker

systemctl enable docker

systemctl status docker

注：如果docker启动失败可以尝试把文件/etc/docker/daemon.json换成换以下内容

{

"registry-mirrors": \[

"https://mirror.baidubce.com",

"https://docker.mirrors.ustc.edu.cn",

"https://docker.nju.edu.cn",

"https://mirror.iscas.ac.cn"

\]

}