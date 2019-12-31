# 深入了解Pod对象
#### 1.Pod参数定义
```yaml
# 必填,版本号
apiVersion: string
kind: Pod
metadata:
    name: string
    namespace: string
    labels: 
      - name: string
    annotations:
      - name: string
spec:
    containers:
      - name: string
        image: string
        imagePullPolicy: [Always | Never | IfNotPresent]
        command: [string]
        args: [string]
        workingDir: string
        volumeMounts:
          - name: string
            mountPath: string
            readOnly: boolean
        ports:
          - name: string
            containerPort: int
            hostPort: int
            protocol: string
        env:
          - name: string
            value: string
        resource: 
            limits:
                cpu: string
                memory: string
            requests:
                cpu: string
                memory: string
        livenessProbe:
            exec:
                command: [string]
            httpGet:
                path: string
                port: number
                host: string
                scheme: string
                httpHeader:
                  - name: string
                    value: string
            tcpSocket:
                port: number
            initialDelaySeconds: 0
            timeoutSeconds: 0
            periodSeconds: 0
            successThreshold: 0
            failureThreshold: 0
        securityContext:
            privileged: boolean
    restartPolicy: [Always | Never | OnFailure]
    nodeSelector: object
    imagePullSecrets:
      - name: string
    hostNetwork: boolean
    volumes:
      - name: string
        emptyDir: {}
        hostPath:
            path: string
        secret:
            secretName: string
            items:
              - key: string
                path: string
        configMap:
            name: string
            items:
              - key: string
                path: string
```