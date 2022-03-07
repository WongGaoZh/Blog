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

# 删除docker镜像
docker rmi `CONTAINER ID`
-f 表示强制删除


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


```
# 实际操作 : 

1 . 启动容器  
docker run -it -rm -v /Users/yanguangyuan/docker_file/:/tmp/ -p 8080:8080 -p 8081:8081  --name ygy  -d ygy:latest

-it 是保证不退出容器

-rm 是容器终止后会立刻删除。--rm参数和-d参数不能同时使用。

覆盖命令 , 如果dockerfile里面是ENTRYPOINT,使用
docker run --entrypoint="/bin/bash ..." ...，给出容器的新Shell

如果dockerfile里面,是CMD
docker run -it -rm  --name zgw -d 容器名 <新命令>

2 . 删除容器
docker stop <容器id> 
docker rm <容器id>


```

## 安装教程

参考:  https://www.runoob.com/docker/docker-container-usage.html
