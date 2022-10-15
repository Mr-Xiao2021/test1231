## 启动类命令

![img](https://wx2.sinaimg.cn/mw2000/008rcJvVly1h6gazrlzmnj30tt0hhtcv.jpg)

## 镜像类命令



![img](https://wx3.sinaimg.cn/mw2000/008rcJvVly1h6gb55zeuej31720oktiq.jpg)



![img](https://wx2.sinaimg.cn/mw2000/008rcJvVly1h6gcdksmrdj30j807qabi.jpg)



````sh
# 查看已有镜像
docker images
docker image -a
docker image -q
# 查看仓库中的镜像
docker search [image-name]
docker search --limit [nums] [image-name]
docker searh --limit 5 redis
# 从仓库中拉取镜像
docker pull image-name
docker pull image-name[:TAG]
docker pull ubantu
docker pull redis:6.0.8
# 删除镜像
docker rmi -f 镜像ID  #删除单个
docker rmi -f 镜像名1:TAG 镜像名2:TAG  #删除多个
docker rmi -f $(docker images -qa) 	#删除全部

````

* 演示

  ![img](https://wx1.sinaimg.cn/mw2000/008rcJvVly1h6gb2brts1j31i20jfk4r.jpg)

  ![img](https://wx1.sinaimg.cn/mw2000/008rcJvVly1h6gb6kho4fj31jk0opnau.jpg)

  ![img](https://wx4.sinaimg.cn/mw2000/008rcJvVly1h6gcdej336j30t508vdj7.jpg)

  ![img](https://wx2.sinaimg.cn/mw2000/008rcJvVly1h6gcdgu2l0j30rp07hwgq.jpg)

  ![img](https://wx2.sinaimg.cn/mw2000/008rcJvVly1h6gcdis0v6j30w806jgnx.jpg)

  



## 容器命令

### 3.1、docker run [options]



> 查看使用文档： docker run --helper

![img](https://wx3.sinaimg.cn/mw2000/008rcJvVly1h6glcv6l0pj30pt0ebn3k.jpg)



示例：

````sh
# 查看正在运行的容器
docker ps
# 容器命令
docker run -it --name=自定义容器名称 /bin/bash
# or
docker run -it bash
````

效果：

![img](https://wx3.sinaimg.cn/mw2000/008rcJvVly1h6glgv3gpcj30yw0d313z.jpg)



### 3.2、<font color="red " size=6>docker ps [options]</font>



查看正在运行的容器

> 查看文档 docker ps --help

![img](https://wx4.sinaimg.cn/mw2000/008rcJvVly1h6glntgwzcj30u60bngsy.jpg)



示例：

![img](https://wx4.sinaimg.cn/mw2000/008rcJvVly1h6glmur1bgj311b0kwdwl.jpg)



查看容器内部的细节：

````sh
docker inspect 容器ID
````





### 3.3、<font color="red " size=6>docker 启动类命令</font>





![img](https://wx2.sinaimg.cn/mw2000/008rcJvVly1h6gm4qd1jxj30ts0e8798.jpg)

> 前台交互式启动和后台守护式启动

![img](https://wx1.sinaimg.cn/mw2000/008rcJvVly1h6gmgq3jw5j30ra05fdh1.jpg)



> 注意

![img](https://wx2.sinaimg.cn/mw2000/008rcJvVly1h6gm9qptynj30mk06aq84.jpg)

<font color="orange" size=4>关于守护式进程的小问题：那为什么执行完：`docker run -d redis:6.0.8`后再`docker ps`却发现`redis`容器并没有停止呢?</font>

因为docker启动ubuntu时并没有进程来夯住容器，所以就会退出。而redis启动是会有进程来夯住容器，不让其退出。redis 一直在监听端口啊，当然不会自杀，想不到吗？因为ubuntu没有前台进程，redis有。



### 3.4、docker 重新进入已启动服务



* docker exec -it 容器ID /bin/bash

`exec是在容器打开新的终端，并且可以启动新的进程，用exit退出不会导致容器的停止`

````sh
# 查看当前存在的容器示例的运行情况
docker ps -a

# docker restart 容器名/ 容器ID

# 以前台交互方式重新进入正在运行的容器
docker exec -it 容器ID /bin/bash

# ctrl + p + q 切换至后台运行
exit # 退出
docker ps -a # 发现容器仍在运行

````

* docker attach 容器ID

`attach直接进入容器启动命令的终端，不会启动新的进程，用exit退出会导致容器的停止`

````sh
docker attach 容器ID
exit # 退出
docker ps -a # 发现容器就停止了
````

注意：

Ctrl + p + q 后台运行容器，但是当你在使用 run 是重新创建一个新的容器，而 exec 是进入之前创建的容器





### 3.5、容器文件备份与导入出



* 容器 ==> 主机

  ````sh
  # 注意是在主机的命令行下
  docker cp 容器ID:容器路径 目标主机的路径
  docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH
  
  
  
  # 例如
  [root@iZf8z3pu4d7ueh3i6pzv5fZ ~]# docker cp d5c7962a5554:/secret.txt /tmp
  [root@iZf8z3pu4d7ueh3i6pzv5fZ ~]# ls /tmp | grep secret
  secret.txt
  [root@iZf8z3pu4d7ueh3i6pzv5fZ ~]# 
  ````

* 容器的导入导出

  `````sh
  docker ps -a
  d5c7962a5554   ubuntu        "bash"                   2 days ago   Exited (137) 4 seconds ago              myu1
  
  # 导出
  docker export d5c7962a5554 > myubuntu.tar
  ll
  -rw-r--r-- 1 root root 80358912 Sep 26 09:36 myubuntu.tar
  
  # 删除容器实例
  docker rm -f d5c7962a5554
  
  # 从tar包中的内容创建一个新的文件系统再导入为镜像
  cat tar包名.tar | docker import - 镜像用户/镜像名:版本号
  
  cat myubuntu.tar | docker import - xxr/ubuntu:1.0
  
  [root@iZf8z3pu4d7ueh3i6pzv5fZ ~]# docker images
  REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
  xxr/ubuntu    1.0       d37efa8a6d92   8 seconds ago   77.8MB
  ubuntu        latest    2dc39ba059dc   3 weeks ago     77.8MB
  `````

  



