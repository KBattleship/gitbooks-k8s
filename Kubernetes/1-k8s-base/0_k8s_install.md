# 安装初始化K8s集群

# 使用kubeadm初始化集群主节点
`kubeadm init --kubernetes-version=v1.17.1 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/12 --ignore-preflight-errors=Swap`

