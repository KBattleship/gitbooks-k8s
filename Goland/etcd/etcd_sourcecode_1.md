# Etcd源码阅读与分析①-raft demo
`Etcd是一个基于Raft协议的简单内存KV项目`
## 源码分析

本文档将以etcd作者在项目中所提供的demo程序进行源码试读。demo名称为raftexample。
路径在

### 1.项目结构
```shell
(base) {11:45}~/etcd:master ✗ ➭ tree -d -L 1 .
.
├── Documentation           # 项目文档
├── auth                    # 认证授权
├── client                  # 客户端相关(v2)
├── clientv3                # 客户端相关(v3)
├── contrib                 # (待验证)
├── default.etcd            # 已编译完成的etcd
├── embed                   # 封装的etcd函数
├── etcdctl                 # etcd操作命令，命令行客户端
├── etcdmain                # main函数入口这里
├── etcdserver              # 服务端相关
├── functional              # 目测验证功能测试套件
├── hack                    # 开发者相关
├── integration             # (待验证)
├── lease                   # 实现etcd租约
├── logos                   # 日志相关
├── mvcc                    # MVCC存储相关
├── pkg                     # 通用依赖库
├── proxy                   # 代理相关Http、Https、Socks
├── raft                    # raft一致性协议实现
├── scripts                 # 各类脚本
├── security                # 安全相关
├── tests                   # (待验证)
├── tools                   # 工具
├── vendor                  # go vendor依赖环境
├── version                 # 版本信息
└── wal                     # Write-Ahead-Log实现
```
