# 深入了解Pod对象
#### 1.Pod参数定义
```yaml
# 必填,版本号
apiVersion: string
kind: Pod
# 必填,元数据
metadata:
    # 必填,Pod对象的名称(命名规范需要符合RFC 1035规范)
    name: string
    # 必填,Pod对象所属的命名空间,默认值为default
    namespace: string
    # 自定义标签列表(取值类型:List)
    labels: 
      - name: string
    # 自定义标签注解(取值类型:List)
    annotations:
      - name: string
# 必填,Pod对象中容器的详细定义  
spec:
    # 必填,Pod对象容器列表(取值类型:List)
    containers:
        # 必填,容器的名称(需要符合RFC 1035规范)
      - name: string
        # 必填,容器镜像名称
        image: string
        # 获取镜像的策略，默认值为:Always
        # Always: 每次都尝试重新下载镜像
        # Never: 仅使用本地镜像
        # IfNotPresent: 如果本地不存在，就下载镜像
        imagePullPolicy: [Always | Never | IfNotPresent]
        # 容器启动命令列表，若不指定则使用镜像打包时使用的启动命令
        command: [string]
        # 容器的启动命令参数列表
        args: [string]
        # 容器的工作目录
        workingDir: string
        # 挂载到容器内部的存储卷配置(取值类型:List)
        volumeMounts:
            # 引用Pod定义的共享存储卷的名称，需使用镜像volumes[]部分定义的共享卷名称
          - name: string
            # 存储卷在容器内Mount的绝对路径(应少于512个字符)
            mountPath: string
            # 是否只读模式,默认false(读写模式)
            readOnly: boolean
        # 容器需要暴露的端口号(取值类型:List)    
        ports:
            # 端口的名称
          - name: string
            # 容器需要监听的端口号
            containerPort: int
            # 容器所在主机需要监听的端口号，默认与containerPort相同
            # (设置hostPort时，同一台宿主机将无法启动该容器的第二副本，由于端口占用问题)
            hostPort: int
            # 端口协议[TCP/UDP],默认为TCP
            protocol: string
        # 容器运行前需要设置的环境变量列表
        env:
            # 环境变量的名称
          - name: string
            # 环境变量的值
            value: string
        # 资源限制和资源请求的设置
        resource: 
            # 资源限制设置
            limits:
                # CPU限制(单位为：core)将用于docker run --cpu-shares参数
                cpu: string
                # 内存限制(单位为：MiB/GiB)将用于docker run --memory参数
                memory: string
            # 资源限制设置(请求)
            requests:
                # CPU请求(单位为：core)将用于容器启动的初始化可用数量
                cpu: string
                # 内存请求(单位为：MiB/GiB)将用于容器启动的初始化可用数量
                memory: string
        # 对Pod对象内各个容器进行安全检查的设置，当探测无响应几次后，将自动重启该容器
        # 包含[exec | httpGet | TcpSocket]三种方式，任选其一即可
        livenessProbe:
            exec:
                # 需要执行的脚本
                command: [string]
            httpGet:
                # 请求路径
                path: string
                # 请求端口
                port: number
                host: string
                scheme: string
                httpHeader:
                  - name: string
                    value: string
            tcpSocket:
                port: number
            # 完成容器启动后首次进行探测的时间(单位为：s)
            initialDelaySeconds: 0
            # 对容器健康检查探测等待超时时间(单位为：s)，默认值为1
            timeoutSeconds: 0
            # 对容器健康检查的探测时间周期(单位为：s)，默认值为10
            periodSeconds: 0
            successThreshold: 0
            failureThreshold: 0
        securityContext:
            privileged: boolean
    #
    # Pod对象的重启策略，可选值[Always | Never | OnFailure]
    #
    # Always: Pod对象一旦终止，则不关心容器是如何停止的，kubelet都将重器容器
    #
    # Never: Pod对象终止后，kubelet将退出码返回给Master，不再重启该容器
    #
    # OnFailure: 只有当Pod对象以非零退出码终止时，kubelet才会重启该容器
    # (容器正常结束的退出码为零)
    #
    restartPolicy: [Always | Never | OnFailure]
    # 表示将Pod对象调度到包含这些label的Node上(以key:value形式指定)
    nodeSelector: object
    # Pull镜像时使用的secret名称(以name:secretValue形式指定)
    imagePullSecrets:
      - name: string
    # 是否使用主机模式(默认值为:false)
    #
    # 如果设置为true，表示容器使用宿主机网络，不再使用Docker网桥
    # 该Pod对象将无法在同一台宿主机上启动第二个副本
    hostNetwork: boolean
    # 在该Pod对象上定义的共享储存卷列表
    volumes:
        # 共享储存卷名称，一个Pod对象中每个储存卷定义一个名称(命名应按照RFC 1035规范)
      - name: string
        # Pod对象同生命周期的一个临时目录，值为{}空对象
        emptyDir: {}
        # 挂载Pod对象所在宿主机的目录
        hostPath:
            # 将用于容器中mount的目录
            path: string
        # 挂载集群中预定义的secret对象到容器内部
        secret:
            secretName: string
            items:
              - key: string
                path: string
        # 挂载集群预定义的configMap对象到容器内部
        configMap:
            name: string
            items:
              - key: string
                path: string
```
#### 2.Pod的基本用法
