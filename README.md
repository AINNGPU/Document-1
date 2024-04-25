Docker常用指令
=======
## 1：启动docker
* 普通启动
```shell
docker run -it homebrew/ubuntu20.04 bash
```

* 带GPU启动
```shell
docker run -it --gpus all homebrew/ubuntu20.04 bash
```
**_homebrew/ubuntu20.04 ：为镜像的REPOSITORY_**

## 2：退出docker后docker依然运行

在Docker中，如果你希望退出容器的交互式会话但不终止容器的运行，可以采用以下几种方法：

* 使用docker attach命令：如果你是通过docker attach命令附加到一个正在运行的容器的会话，可以通过按Ctrl-p Ctrl-q序列退出会话，这样做不会停止容器。

* 使用docker exec启动交互式会话：你可以通过docker exec命令创建一个新的会话，比如运行docker exec -it container_name bash进入容器的bash会话。退出这样的会话可以使用exit命令或者Ctrl-d，这也不会停止容器。

* 后台模式运行容器：如果容器是在后台运行的（使用了-d或--detach选项启动），则不需要特别操作来保持其运行。你可以通过docker logs查看容器日志或使用docker exec再次进入容器

## 3：进入一个正在运行的docker容器中

要进入一个正在运行的Docker容器（通常称为“Docker镜像”的实例），可以使用以下方法之一：

* 使用docker exec命令：这是进入正在运行的Docker容器最常用的方法。你可以使用这个命令启动一个新的交互式终端会话。例如，如果你想进入一个运行bash shell的容器，可以使用以下命令：

```shell
docker exec -it [容器ID或名称] bash
```
-it：参数是让Docker分配一个伪终端并保持标准输入开启，bash是你想在容器中启动的程序。如果容器中没有bash，你可以尝试使用sh或其他可用的shell。

* 使用docker attach命令：如果你想要连接到容器的主进程（通常是启动容器时指定的命令），可以使用docker attach命令。这允许你查看并与容器的主进程进行交互。使用此命令如下：
```shell
docker attach [容器ID或名称]
```
**_使用docker attach时需要注意，你将连接到容器的主进程输出流。如果退出，比如通过Ctrl-C，主进程将收到中断信号，这可能会终止容器。如果要离开而不终止容器，可以使用Ctrl-p Ctrl-q的组合键。_**


## 4：从docker的images中删除一个镜像
+ 普通删除
```shell
docker rmi 4a415e366388
```
* <镜像 ID>: 这是你希望外界访问容器时使用的端口。

+ 强制删除
```shell
docker rmi -f 4a415e366388
```

## 5：启动容器时映射端口

```shell
docker run -p <主机端口>:<容器端口> <镜像名>
```
- <主机端口>: 这是你希望外界访问容器时使用的端口。
- <容器端口>: 这是容器内部应用程序设定监听的端口。
- <镜像名>: 这是你想要启动的Docker镜像的名称。

* 示例
_假设你有一个运行Web服务器的Docker镜像，服务器在容器的80端口上监听，你希望通过主机的8080端口访问它，命令如下：_
```shell
docker run -p 8080:80 nginx
```

* 如果需要映射多个端口，你可以重复使用 
-p 选项。例如，如果你还需要将容器的443端口映射到主机的8443端口，可以这样写：
```shell
docker run -p 8080:80 -p 8443:443 nginx
```
**这样配置后，任何发往主机8080端口的请求都会被转发到容器的80端口，而发往8443端口的请求则会被转发到容器的443端口。这样就实现了端口映射，允许从主机环境通过指定的端口访问运行在容器中的应用。**

## 6：拷贝目录到容器中

```shell
docker cp <容器ID或名称>:<容器内路径> <宿主机路径>
```

* 示例
```shell
docker cp /home/user/data mycontainer:/app/data
```

## 7：加载tar文件

```shell
docker load -i <path/to/image.tar>
```

* 示例
```shell
docker load -i /root/v2ray.tar
```

