#
## 100_docker安装mysql并挂载到宿主机上

###  下载镜像
docker pull mysql:5.7

### 创建路径
mkdir -p /Users/17532/docker/mysql/conf
mkdir -p /Users/17532/docker/mysql/data
mkdir -p /Users/17532/docker/mysql/logs

### 编写my.cnf文件
vi /Users/17532/docker/mysql/conf/my.cnf

```aidl
[mysqld]
character-set-server=utf8 

[client]
default-character-set=utf8 
[mysql]
default-character-set=utf8 

# Custom config should go here
!includedir /etc/mysql/conf.d/
```
### 启动命令
docker run --restart=always -d -v /Users/17532/docker/mysql/conf:/etc/mysql -v /Users/17532/docker/mysql/logs:/var/log/mysql -v /Users/17532/docker/mysql/data:/var/lib/mysql  -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
