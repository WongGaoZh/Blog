# docker使用

## docker概念知识

dockerhub和 harbor得关系

harbor是企业版的docker，企业可以搭建私有化得harbor做仓库

## docker 命令

```shell script

# 下载docker
docker pull iregistry.spark-int.com/ist/bvs-algo:20210418_1618739842536

# 查看docker 
docker images

# 启动docker
docker run -it -v /Users/yanguangyuan/docker_file/:/tmp/ -p 8080:8080 -p 8081:8081  --name ygy  -d ygy:latest


# 查看启动得docker
docker ps
docker ps -l 是查看最后一个进程

# 删除docker
docker rmi `CONTAINER ID`


# 进入docker容器里面
docker exec -it `CONTAINER ID` /bin/bash

# 打镜像
docker commit -m "test" -a "ygy" 6db1a348dc61 ygy


# 把主机文件复制进镜像里面
docker cp jdk-8u281-linux-x64.tar.gz 97541767b9b4:/tmp


# 把docker保存成tar文件并导出
docker save iregistry.baidu-int.com/acg-det/ct-ocr-console:1.0.7 -o console.tar

# 加载镜像
docker load -i dianxin_human.tar 


# docker dockerfile变成镜像
docker build -t runoob/ubuntu:v1 . 

```

## 安装教程

参考:  https://www.runoob.com/docker/docker-container-usage.html
