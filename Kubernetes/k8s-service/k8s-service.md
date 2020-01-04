# 深入Service
>Service是Kubernetes最为核心的概念。Service可以为一组具有相同功能的容器应用提供一个统一的入口地址，并且将请求负载分发至后端的各个容器应用上。

#### 1.Service参数定义

```yaml
apiVersion: v1
kind: Service
metadata:
    name: string
    namespace: string
    labels:
      - name: string
    annotations:
      - name: string
spec:
    selector: []
    type: string
    clusterIP: string
    sessionAffinity: string
    ports:
      - name: string
        protocol: string
        port: int
        targetPort: int
        nodePort: int
    status:
        loadBalancer: 
            ingress:
                ip: string
                hostname: string
```