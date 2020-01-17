# 2.Docker安装

+ **存储库安装**
    1. 安装`yum-config-manager`所需依赖包
        
        ```shell
        $~:sudo yum install -y yum-utils \
            device-mapper-persistent-data \
            lvm2
        ```
    2. 通过`yum-config-manager`添加存储库
        ```shell
        $~:sudo yum-config-manager \
            --add-repo \
            https://download.docker.com/linux/centos/docker-ce.repo
        ```
    3. 列出存储库中排序后可用的全部版本
        ```shell
        yum list docker-ce --showduplicates | sort -r
        ```
    4. 进行安装
        ```shell
        # 指定版本号安装
        sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
        # 安装最新版本（不指定版本号默认为最新）
        sudo yum install docker-ce docker-ce-cli containerd.io
        ```
+ **软件包安装** 