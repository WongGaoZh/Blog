# linux安装软件

> 安装路径规范: 
>/opt目录用来安装附加软件包，是用户级的程序目录，
>
>/usr/local：用户级的程序目录
>
>所以像jdk, maven , 等软件安装到/usr/local, 推荐优先使用解压版


> mirror
>apache基金仓库:  https://mirrors.cnnic.cn/apache/
>
>华为仓库:  https://mirrors.huaweicloud.com/home

## window 配置 远程linux 免密登入

```shell
 # ssh路径地址
 ~/.
 
# 通过scp命令,把window的id_rsa.pub上传到linux服务器的ssh路径下
scp id_rsa.pub root@xxx:/root/.ssh/

# 在linux服务器上
cd ~/. 
cat id_ras.pub >>  authorized_keys 
```


## linux安装jdk

- 服务包

- 安装过程
```$shell
tar -zxvf jdk-8uxxx-linux-x64.tar.gz
# 为了待会配置环境变量的时候方便，将文件夹名字改为jdk
mv jdk1.8.0_281/ jdk

vim /etc/profile

# jdk
export JAVA_HOME=/usr/local/jdk
export CLASSPATH=$:CLASSPATH:$JAVA_HOME/lib/
export PATH=$PATH:$JAVA_HOME/bin
# 使环境变量生效
source /etc/profile

```

## linux安装maven

```

# 解压

# 配置环境变量
vim /etc/profile

# maven
export M2_HOME=/usr/local/apache-maven-3.3.9
export PATH=$PATH:$M2_HOME/bin
# 使环境变量生效
source /etc/profile
```

