

# Ubuntu 安装 docker

**1.更新软件源**

```
sudo apt-get update
```

**3.安装docker仓库软件包依赖**

```
sudo apt-get install ca-certificates curl gnupg lsb-release
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

**4.在系统中添加Docker的官方密钥**

```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

**5.添加docker源**

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```

**6.再次更新源列表**

```
sudo apt-get update
```

**7.查看可以安装的docker版本并安装docker**

```
sudo apt-cache policy docker-ce
sudo apt install docker-ce
```

**8.使用命令查看是否安装成功以及安装的docker版本**

```
docker -v
```

**9.启动 docker服务并设置开机自动启动docker**

```
sudo systemctl start docker//win 子系统 使用 sudo service docker start
sudo systemctl enable docker
```

# xshell连接windows子系统linux

**修改ssh配置文件** 

1. cd /etc/ssh  ll查询目录
2. cat sshd_config
3. vim sshd_config

```
#修改端口
Port 2222 
#打开本地监听
ListenAddress 0.0.0.0  
#允许密码登陆
PasswordAuthentication yes
#允许root登录
PermitRootLogin yes
```

**重启ssh**

```
sudo service ssh restart
```

**Error(15) 解决 sshd: no hostkeys available -- exiting.**

1. ssh-keygen -A 
2. /etc/init.d/ssh start

**通过远程ip连接wsl中的服务（局域网）**

1. 在wsl子系统中，使用以下命令，获取wsl的ip

   ```
   ip addr | grep eth0
   ```

1. 在windows中，用管理员方式打开powershell，输入命令，这里我的wsl的ip为172.30.64.232，要启动服务的端口为2345，这里因为8080端口限制比较多，所以换了一个普通的端口，因此命令如下：

```
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=2345 connectaddress=172.30.64.232 connectport=2345
```

# CentOS7 安装Docker

**1.安装需要的软件包， yum-util 提供yum-config-manager功能，另两个是devicemapper驱动依赖**

```javascript
yum install -y yum-utils device-mapper-persistent-data lvm2
```

**2.设置 yum 源**

设置一个yum源，下面两个都可用

```javascript
yum-config-manager --add-repo http://download.docker.com/linux/centos/docker-ce.repo（中央仓库）

yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo（阿里仓库）
```

**3.选择docker版本并安装 （1）查看可用版本有哪些**

```javascript
yum list docker-ce --showduplicates | sort -r
```

**4.直接安装最新docker版本**

```
yum -y install docker-ce docker-ce-cli containerd.io
查看版本
docker -v
```
