# 安装初始化K8s集群
## 1.安装K8s
### 1.1CentOS安装
+ 预先准备工作

    ```shell
    # 修改设置主机名称
    hostnamectl set-hostname master
    # 绑定主机各节点hosts
    192.168.0.1 master
    192.168.0.2 node1
    192.168.0.3 node2
    # 验证每节点的Mac地址与UUID是否唯一
    # mac地址注意查看网卡
    cat /sys/class/net/eth1/address
    cat /sys/class/dmi/id/product_uuid
    # 关闭缓存交换swap
    swapoff -a  # 临时关闭
    sed -i.bak '/swap/s/^/#/' /etc/fstab    #永久关闭
    ```
    
+ 安装Kubernetes
    ```shell
    # 设置K8s安装源，由于防火墙问题使用阿里云源
    cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
    # 更新源缓存
    yum clean all
    yum -y makecache
    
    # 查看k8s版本
    yum list kubelet --showduplicates | sort -r 
    
    # 默认安装最新版本
    yum install -y kubelet kubeadm kubectl
    # 选择指定版本进行安装
    yum install -y kubelet-<version> kubeadm-<version> kubectl-<version>
    ```

### 1.2MacOS安装
```shell
Docker-Desktop版内置(需要访问科学上网)
```
## 2.准备Kubernetes依赖镜像
```shell
k8s.gcr.io/kube-apiserver:v1.17.1
k8s.gcr.io/kube-controller-manager:v1.17.1
k8s.gcr.io/kube-proxy:v1.17.1
k8s.gcr.io/kube-scheduler:v1.17.1
k8s.gcr.io/coredns:v1.17.1
k8s.gcr.io/etcd:v1.17.1
```
由于国外站点问题，需要科学上网，或者通过其他镜像仓库拉去，然后通过`docker tag`打标签的形式保存在docker仓库
```shell
# 通过科学上网拉去官网仓库镜像
docker pull k8s.gcr.io/kube-apiserver:v1.17.1
...
k8s.gcr.io/etcd:v1.17.1
## 然后通过本地惊醒仓库打包导出tar的形式
docker save k8s.gcr.io/kube-apiserver:v1.17.1 > k8s.tar.gz
## 导入目标服务器的本地镜像仓库
docker load < k8s.tar.gz
```

## 3.使用kubeadm初始化集群主节点

```shell
kubeadm init --kubernetes-version=v1.17.1 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/12 --ignore-preflight-errors=Swap
```





