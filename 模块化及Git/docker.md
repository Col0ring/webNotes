# docker

## 基本操作

### 设置镜像

设置为国内镜像：

**mac:**`cd ~/.docker/daemon.json`

```json
{
  "experimental": false,
  "debug": true,
  "registry-mirrors": ["https://hub.daocloud.io/"],
  // 私服 ip 和 port
  // "insecure-registries": ["ip:port"]
}
```

然后重启 docker 服务即可。



### 镜像操作

#### 拉取镜像到本地

```shell
docker pull 镜像名称[:tag]
# 比如
docker pull nginx
```



#### 查看全部本地镜像

```shell
docker images
```



#### 删除本地镜像

```shell
# 镜像 id 可通过 docker images 查看
docker rmi 镜像id
```



#### 镜像的导入导出

- **导出本地镜像**

  ```shell
  docker save -o 导出路径 镜像id
  ```
  
- **导入本地镜像**

  ```shell
  docker load -i 镜像文件
  ```

  **注意：**导入的镜像默认是没有名字和版本号的，需要使用下面的命令进行修改。

  ```shell
  docker tag 镜像id 新镜像名称:版本号
  ```

  



### 容器操作

#### 运行容器

- **简单操作**

  ```shell
  docker run 镜像id | 镜像名称[:tag]
  ```
  
- **常用参数**

  ```shell
  docker run -d -p 宿主机端口:容器端口 --name 容器名称 镜像id | 镜像名称[:tag]
  ```

  - -d：代表后台运行容器。
  - -p 宿主机端口:容器端口：将当前主机端口与启动容器端口做映射。
  - --name 容器名称：指定容器的名称。



#### 查看正在进行的容器

```shell
docker ps [-qa]
```

- -a：查看全部的容器，包括没有运行的容器。
- -q：只查看容器的标示id。



#### 查看容器日志

```shell
docker logs -f 容器id
```

- -f；滚动查看最后几行。



#### 进入容器内部

```shell
docker exec -it 容器id bash
```



#### 启动容器

启动容器和运行容器不同，启动容器是启动已经创建的容器，运行容器的时候该容器还没有创建。

```shell
docker start 容器id
```



#### 停止容器

```shell
# 停止指定容器
docker stop 容器id
# 停止所有容器
docker stop $(docker ps -qa)
```



#### 删除容器

要删除容器，需要先停止容器。

```shell
# 删除指定容器
docker rm 容器id
# 删除全部容器
docker rm $(docker ps -qa)
```



### Docker 应用

#### 将宿主机文件复制到容器内部

```shell
docker cp 文件名称 容器id:容器内部路径
```



#### 数据卷

数据卷可以将宿主机的一个目录映射到容器的一个目录中，可以在宿主机中操作目录中的内容，那么容器内部映射的文件，也会跟着一起改变。

##### 创建数据卷

```shell
docker volume create 数据卷名称
```



##### 查看全部数据卷

```shell
docker volume ls
```



##### 查看数据卷信息

```shell
docker volume inspect 数据卷名称
```



##### 删除数据卷

```shell
docker volume rm 数据卷名称
```



##### 应用数据卷

- 使用数据卷映射，如果数据卷不存在就自动创建，同时会将容器内自带的文件存储在默认的存放路径中。

  ```shell
  docker run -v 数据卷名称:容器内部路径 镜像id
  ```

  创建完毕后，我们需要将我们在宿主机所要映射的文件放在对应的数据卷的`_data`目录下，这样容器就能直接访问到目录下的文件。

- 直接指定一个路径作为存放位置，但是路径下是空的，不会讲容器原本默认内容带出（比如 nginx 的启动欢迎页），需要手动添加内容。
  ```shell
  docker run -v 路径:容器内部路径 镜像id
  ```



## 自定义镜像