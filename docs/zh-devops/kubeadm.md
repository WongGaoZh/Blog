# kubeadm 搭建集群 <!-- {docsify-ignore-all} -->

## 1、硬件配置
建议至少2 cpu ,2G，非硬性要求，1cpu，1G也可以搭建起集群。但是：

1个cpu的话初始化master的时候会报	
```shell script
[WARNING NumCPU]: the number of available CPUs 1 is less than the required 2
```

部署插件或者pod时可能会报
```shell script 

warning：FailedScheduling：Insufficient cpu, Insufficient memory
```

## 2、安装docker
wget -O /etc/yum.repos.d/docker-ce.repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum -y install docker-ce
systemctl daemon-reload
systemctl start docker
systemctl enable docker

## 3、修改内核参数
开启ipv6，否则会造成coredns容器无法启动

cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward=1
EOF

sysctl --system

## 4、关闭Swap
k8s1.8版本以后，要求关闭swap，否则默认配置下kubelet将无法启动。

swapoff -a

防止开机自动挂载 swap 分区
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

## 5、开启ipvs
不是必须，只是建议，pod的负载均衡是用kube-proxy来实现的，实现方式有两种，一种是默认的iptables，一种是ipvs，ipvs比iptable的性能更好而已。
ipvs是啥？为啥要用ipvs？：https://blog.csdn.net/fanren224/article/details/86548398
后面master的高可用和集群服务的负载均衡要用到ipvs，所以加载内核的以下模块


需要开启的模块是
ip_vs
ip_vs_rr
ip_vs_wrr
ip_vs_sh
nf_conntrack_ipv4

检查有没有开启
cut -f1 -d " "  /proc/modules | grep -e ip_vs -e nf_conntrack_ipv4

没有的话，使用以下命令加载
sudo modprobe -- ip_vs
sudo modprobe -- ip_vs_rr
sudo modprobe -- ip_vs_wrr
sudo modprobe -- ip_vs_sh
sudo modprobe -- nf_conntrack_ipv4

yum install ipset -y

## 6、禁用selinux，关闭防火墙，网络，dns，ssh，ntp
集群规划
etcd节点x1 : 注意etcd必须是奇数个节点
master节点x1 : 没有奇数偶数的限制，后面可以根据实际情况再增加节点数，
node节点x3 : 真正应用负载的节点，后面可以根据实际情况很方便的增加节点数
主机名	ip	角色
master1	192.168.255.130	master+etcd
slave1-3	192.168.255.121-123	node
高可用集群部署见：https://blog.csdn.net/fanren224/article/details/86573264


一、安装 kubeadm, kubelet 和 kubectl

cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF

安装时最好加上版本号
yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes  #禁用除kubernetes之外的仓库

设置开机启动kubelet

systemctl enable kubelet 

二、初始化master
1、先提前下载初始化时需要用到的Images
在所有节点上提前下载镜像

有两种办法，参考文章： http://blog.51cto.com/purplegrape/2315451

第一种：从国内源下载好然后修改tag（推荐方式）

先查看要用到的镜像有哪些，这里要注意的是：要拉取的4个核心组件的镜像版本和你安装的kubelet、kubeadm、kubectl 版本需要是一致的。
```shell script


kubeadm config images list

#拉镜像
kubeadm config images list |sed -e 's/^/docker pull /g' -e 's#k8s.gcr.io#mirrorgooglecontainers#g' |sh -x

docker pull coredns/coredns:1.2.6


#修改tag，将镜像标记为k8s.gcr.io的名称
docker images |grep mirrorgooglecontainers |awk '{print "docker tag ",$1":"$2,$1":"$2}' |sed -e 's#mirrorgooglecontainers#k8s.gcr.io#2' |sh -x

docker tag coredns/coredns:1.2.6 k8s.gcr.io/coredns:1.2.6


#删除无用的镜像
docker images | grep mirrorgooglecontainers | awk '{print "docker rmi "  $1":"$2}' | sh -x

docker rmi coredns/coredns:1.2.6
```
2、开始初始化
初始化之前最好先了解一下 kubeadm init 参数
--apiserver-advertise-address string
API Server将要广播的监听地址。如指定为 `0.0.0.0` 将使用缺省的网卡地址。

--apiserver-bind-port int32     缺省值: 6443
API Server绑定的端口

--apiserver-cert-extra-sans stringSlice
可选的额外提供的证书主题别名（SANs）用于指定API Server的服务器证书。可以是IP地址也可以是DNS名称。

--cert-dir string     缺省值: "/etc/kubernetes/pki"
证书的存储路径。

--config string
kubeadm配置文件	的路径。警告：配置文件的功能是实验性的。

--cri-socket string     缺省值: "/var/run/dockershim.sock"
指明要连接的CRI socket文件

--dry-run
不会应用任何改变；只会输出将要执行的操作。

--feature-gates string
键值对的集合，用来控制各种功能的开关。可选项有:
Auditing=true|false (当前为ALPHA状态 - 缺省值=false)
CoreDNS=true|false (缺省值=true)
DynamicKubeletConfig=true|false (当前为BETA状态 - 缺省值=false)

-h, --help
获取init命令的帮助信息

--ignore-preflight-errors stringSlice
忽视检查项错误列表，列表中的每一个检查项如发生错误将被展示输出为警告，而非错误。 例如: 'IsPrivilegedUser,Swap'. 如填写为 'all' 则将忽视所有的检查项错误。

--kubernetes-version string     缺省值: "stable-1"
为control plane选择一个特定的Kubernetes版本。

--node-name string
指定节点的名称。

--pod-network-cidr string
指明pod网络可以使用的IP地址段。 如果设置了这个参数，control plane将会为每一个节点自动分配CIDRs。

--service-cidr string     缺省值: "10.96.0.0/12"
为service的虚拟IP地址另外指定IP地址段

--service-dns-domain string     缺省值: "cluster.local"
为services另外指定域名, 例如： "myorg.internal".

--skip-token-print
不打印出由 `kubeadm init` 命令生成的默认令牌。

--token string
这个令牌用于建立主从节点间的双向受信链接。格式为 [a-z0-9]{6}\.[a-z0-9]{16} - 示例： abcdef.0123456789abcdef

--token-ttl duration     缺省值: 24h0m0s
令牌被自动删除前的可用时长 (示例： 1s, 2m, 3h). 如果设置为 '0', 令牌将永不过期

注意：

因为后面要安装网络插件flannel ，所有这里要添加参数， --pod-network-cidr=10.244.0.0/16，10.244.0.0/16是flannel插件固定使用的ip段，它的值取决于你准备安装哪个网络插件

如果要自定义配置，先kubeadm config print init-defaults >kubeadm.conf，再修改，改完指定配置文件路径--config /root/kubeadm.conf

指定Kubenetes版本--kubernetes-version，如果不指定该参数，会从google网站下载最新的版本信息，因为它的默认值是stable-1。

若使用的是虚拟机，只分配一个cpu，所以指定了参数--ignore-preflight-errors=NumCPU，如果你的cpu足够，不要添加这个参数.

kubeadm init --pod-network-cidr=10.244.0.0/16 --ignore-preflight-errors=NumCPU --kubernetes-version=1.14.0

根据输出的内容，可以了解到初始化Kubernetes集群所需要的关键步骤。

1、[preflight] 检查系统状态。 发生错误会退出执行，除非指定了 --ignore-preflight-errors=<list-of-errors> 

2、[kubelet-start] 启动kubelet

3、[certs] 生成各种证书。可以通过 --cert-dir 指定自有的证书目录（缺省值为 /etc/kubernetes/pki）

3、[kubeconfig] 在/etc/kubernetes/ 目录，生成配置文件 admin.conf（kubectl） ，kubelet.conf 、controller-manager.conf  和 scheduler.conf 

4、[control-plane] 为 apiserver、controller manager 和 scheduler 生成创建Pod时要用到的yaml文件。

5、[etcd]生成 本地 etcd 的Pod yaml，除非指定外部 etcd

6、[wait-control-plane] 安装master的组件 apiserver、controller manager 和 scheduler

7、[apiclient] 检查组件是否健康

8、[uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace

9、[kubelet] Creating a ConfigMap "kubelet-config-1.13" in namespace kube-system with the configuration for the kubelets in the cluster

10、[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "master.hanli.com" as an annotation

11、[mark-control-plane] 将master节点标记为不可调度 

12、[bootstrap-token]  token

13、[addons] 安装 CoreDNS和kube-proxy

机器上的用户要使用 kubectl来 管理集群操作集群，需要做如下配置：

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

如果不这样做的话，使用kubectl会报错

[root@master1] ~$ kubectl get pods -n kube-system -o wide
The connection to the server localhost:8080 was refused - did you specify the right host or port?

为了使用更便捷，启用 kubectl 命令的自动补全功能。

echo "source <(kubectl completion bash)" >> ~/.bashrc

三、 将worker节点加入集群

kubeadm reset
kubeadm join ....

查看是否成功：

kubectl get nodes

kubectl get pods -n kube-system -o wide

四、 安装Flannel 网络插件
这里选用 Flannel 网络插件：https://github.com/coreos/flannel/tree/v0.10.0
只在master上操作即可
下载yml

wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

kubectl apply -f kube-flannel.yml 

查看是否创建pod正常

kubectl get pods --all-namespaces

五、检查集群是否搭建成功
如果以下状态都正常，就说明搭建成功了。
```shell script


节点状态

[root@master] ~$ kubectl get nodes
NAME               STATUS   ROLES    AGE   VERSION
master.hanli.com   Ready    master   60m   v1.13.2
slave1.hanli.com   Ready    <none>   58m   v1.13.2
slave2.hanli.com   Ready    <none>   57m   v1.13.2
slave3.hanli.com   Ready    <none>   58m   v1.13.2

组件状态

[root@master] ~$  kubectl get cs
NAME                 STATUS    MESSAGE              ERROR
controller-manager   Healthy   ok                   
scheduler            Healthy   ok                   
etcd-0               Healthy   {"health": "true"}   

服务账户

[root@master] ~$ kubectl get serviceaccount
NAME      SECRETS   AGE
default   1         44m

集群信息

[root@master] ~$ kubectl cluster-info
Kubernetes master is running at https://192.168.255.130:6443
KubeDNS is running at https://192.168.255.130:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'

验证dns功能

[root@master] ~/heapster-1.6.0-beta.1/deploy/kube-config/influxdb$ kubectl run curl --image=radial/busyboxplus:curl -it kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
If you don't see a command prompt, try pressing enter.
[ root@curl-66959f6557-r4crd:/ ]$ nslookup kubernetes.default
Server:    10.96.0.10
Address 1: 10.96.0.10 kube-dns.kube-system.svc.cluster.local

Name:      kubernetes.default
Address 1: 10.96.0.1 kubernetes.default.svc.cluster.local
```

六、 测试集群功能是否正常
我们创建一个nginx的service试一下集群是否可用。
```shell script
[root@master] ~$ kubectl run nginx --replicas=2 --labels="run=load-balancer-example" --image=nginx  --port=80
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/nginx created


[root@master] ~$ kubectl expose deployment nginx --type=NodePort --name=example-service
service/example-service exposed

[root@master] ~$ kubectl describe service example-service
Name:                     example-service
Namespace:                default
Labels:                   run=load-balancer-example
Annotations:              <none>
Selector:                 run=load-balancer-example
Type:                     NodePort
IP:                       10.107.118.34
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  30952/TCP
Endpoints:                10.244.1.4:80,10.244.3.2:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>

```


服务状态

```shell script
[root@master] ~$ kubectl get service
NAME              TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
example-service   NodePort    10.107.118.34   <none>        80:30952/TCP   15s
kubernetes        ClusterIP   10.96.0.1       <none>        443/TCP        100m

```

查看pod
```shell script
[root@master] ~$ kubectl get pods 
NAME                     READY   STATUS    RESTARTS   AGE
nginx-58db6fdb58-5wt7p   1/1     Running   0          5m21s
nginx-58db6fdb58-7qkfn   1/1     Running   0          5m21s

```

访问服务ip

[root@master] ~$ curl 10.107.118.34:80
```shell script
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

访问endpoint，与访问服务ip结果相同。这些 IP 只能在 Kubernetes Cluster 中的容器和节点访问。endpoint与service 之间有映射关系。service实际上是负载均衡着后端的endpoint。其原理是通过iptables实现的，这个不是本文内容，在此不谈。

curl 10.244.1.4:80
curl 10.244.3.2:80

访问节点ip，与访问集群ip相同,可以在集群外部访问。

curl 192.168.255.121:30952
curl 192.168.255.122:30952
curl 192.168.255.123:30952

整个部署过程是这样的：

① kubectl 发送部署请求到 API Server。

② API Server 通知 Controller Manager 创建一个 deployment 资源。

③ Scheduler 执行调度任务，将两个副本 Pod 分发到 node1 和 node2。

④ node1 和 node2 上的 kubelet 在各自的节点上创建并运行 Pod。

至此集群部署完成




