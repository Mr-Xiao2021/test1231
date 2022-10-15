[在 CentOS |上安装Docker文档 (docker.com)](https://docs.docker.com/engine/install/centos/)





```sh
# 安装需要的软件包
yum install -y yum-utils
# 设置阿里云的镜像仓库
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 更新yum软件包索引
yum makecache fast
# 安装最新版本的 Docker 引擎、容器化和 Docker 撰写，或转到下一步以安装特定版本：
yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin



# 启动
systemctl start docker
# 查看版本
docker version
# 测试启动
docker run hello-world
```

![img](https://wx3.sinaimg.cn/mw2000/008rcJvVly1h6ga1mdvabj30ra0iawlc.jpg)



![img](https://wx2.sinaimg.cn/mw2000/008rcJvVly1h6ga3awlrxj30ro0gck3a.jpg)







> 设置阿里云镜像加速

![img](https://wx2.sinaimg.cn/mw2000/008rcJvVly1h6ganvenkkj30if06040i.jpg)



