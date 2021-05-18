# k8s原理和命令

## k8s原理
https://blog.csdn.net/qq_40378034/article/details/104884825

## helm的原理
https://www.cnblogs.com/fawaikuangtu123/p/11296574.html

## k8s的基本命令

```shell script
# 查看k8s是否支持gpu
kubectl describe node 你的主机名

# 查看nvida驱动是否装上了
nvidia-smi

# 查看nvida-docker是否安装成功
nvidia-docker volume ls

# 看看service对应的nodeport是啥 
kubectl -n acg-voice get svc -o wide 

# 查看k8s各模块状态
kubectl -n kube-system get po -o wide

# 查看命令
kubectl get pods --namespace acg-voice

#  查看pod报什么错
kubectl describe po <pod名称>

# 查看pod
kubectl get pods --namespace acg-voice

# 查看命名空间
kubectl get ns

# 查看pod的log
kubectl logs pod

# 检查集群状态
kubectl get nodes
kubectl get cs
kubectl get pods
helm list

# 查看kubelet的日志
journalctl -f -u kubelet.service
```

