# 	docker

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

### Dockerfile

创建自定义镜像就需要创建一个 Dockerfile，如下为 Dockerfile 的语言：

- from：指定当前自定义镜像依赖的环境。
- copy：将相对路径下的内容复制到自定义镜像中。
- workdir：声明镜像的默认工作目录。
- run：执行的命令，可以编写多个。
- cmd：需要执行的命令（在 workdir下执行的，cmd 可以写多个，只以最后一个为准）。

```dockerfile
#示例：
from daocloud.io/library/tomcat:8.5.15-jre8
copy ssm.war /usr/local/tomcat/webapps
```



### 通过 Dockerfile 制作镜像

编写完 Dockerfile 后需要通过命令将其制作为镜像，并且要在 Dockerfile 的当前目录下，之后即可在镜像中查看到指定的镜像信息。

```dockerfile
docker build -t 镜像名称[:tag] ./
```



## 八、Docker-Compose

### 1.下载并安装Docker-Compose

### 1.1下载Docker-Compose

```
#去github官网搜索docker-compose，下载1.24.1版本的Docker-Compose下载路径：https://github.com/docker/compose/releases/download/1.24.1/docker-compose-Linux-x86_64
```

### 1.2设置权限

```
#需要将DockerCompose文件的名称修改一下，给予DockerCompose文件一个可执行的权限mv docker-compose-Linux-x86_64 docker-composechmod 777 docker-compose
```

### 1.3配置环境变量

```
#方便后期操作，配置一个环境变量#将docker-compose文件移动到了/usr/local/bin，修改了/etc/profile文件，给/usr/local/bin配置到了PATH中 mv docker-compose /usr/local/binvi /etc/profile#添加内容：export PATH=$JAVA_HOME:/usr/local/bin:$PATHsource /etc/profile
```

### 1.4测试

```
在任意目录下输入docker-compose
```

### 2.Docker-Compose管理MySQL和Tomcat容器

```
yml文件以key:value方式来指定配置信息多个配置信息以换行+缩进的方式来区分在docker-compose.yml文件中，不要使用制表符 version: '3.1'services:  mysql:           # 服务的名称    restart: always   # 代表只要docker启动，那么这个容器就跟着一起启动    image: daocloud.io/library/mysql:5.7.4  # 指定镜像路径    container_name: mysql  # 指定容器名称    ports:      - 3306:3306   #  指定端口号的映射    environment:      MYSQL_ROOT_PASSWORD: root   # 指定MySQL的ROOT用户登录密码      TZ: Asia/Shanghai        # 指定时区    volumes:     - /opt/docker_mysql_tomcat/mysql_data:/var/lib/mysql   # 映射数据卷  tomcat:    restart: always    image: daocloud.io/library/tomcat:8.5.15-jre8    container_name: tomcat    ports:      - 8080:8080    environment:      TZ: Asia/Shanghai    volumes:      - /opt/docker_mysql_tomcat/tomcat_webapps:/usr/local/tomcat/webapps      - /opt/docker_mysql_tomcat/tomcat_logs:/usr/local/tomcat/logs
```

### 3.使用docker-compose命令管理容器

```
在使用docker-compose的命令时，默认会在当前目录下找docker-compose.yml文件 #1.基于docker-compose.yml启动管理的容器docker-compose up -d #2.关闭并删除容器docker-compose down #3.开启|关闭|重启已经存在的由docker-compose维护的容器docker-compose start|stop|restart #4.查看由docker-compose管理的容器docker-compose ps #5.查看日志docker-compose logs -f
```

### 4.docker-compose配合Dockerfile使用


使用docker-compose.yml文件以及Dockerfile文件在生成自定义镜像的同时启动当前镜像，并且由docker-compose去管理容器

### 4.1docker-compose文件

```
编写docker-compose文件 # yml文件version: '3.1'services:  ssm:    restart: always    build:            # 构建自定义镜像      context: ../      # 指定dockerfile文件的所在路径      dockerfile: Dockerfile   # 指定Dockerfile文件名称    image: ssm:1.0.1    container_name: ssm    ports:      - 8081:8080    environment:      TZ: Asia/Shanghai
```

### 4.2 Dockerfile文件

```
编写Dockerfile文件 from daocloud.io/library/tomcat:8.5.15-jre8copy ssm.war /usr/local/tomcat/webapps
```

### 4.3 运行

```
#可以直接基于docker-compose.yml以及Dockerfile文件构建的自定义镜像docker-compose up -d# 如果自定义镜像不存在，会帮助我们构建出自定义镜像，如果自定义镜像已经存在，会直接运行这个自定义镜像#重新构建自定义镜像docker-compose build#运行当前内容，并重新构建docker-compose up -d --build
```

## 九、CI、CD介绍及准备

### 1.CI、CD引言


项目部署
1.将项目通过maven进行编译打包
2.将文件上传到指定的服务器中
3.将war包放到tomcat的目录中
4.通过Dockerfile将Tomcat和war包转成一个镜像，由DockerCompose去运行容器
项目更新后，需要将上述流程再次的从头到尾的执行一次，如果每次更新一次都执行一次上述操作，很费时，费力。我们就可以通过CI、CD帮助我们实现持续集成，持续交付和部署

### 2.CI介绍


CI（continuous intergration）持续集成
持续集成：编写代码时，完成了一个功能后，立即提交代码到Git仓库中，将项目重新的构建并且测试。



1.快速发现错误。
2.防止代码偏离主分支。

### 3.搭建Gitlab服务器

### 3.1.准备环境


实现CI，需要使用到Gitlab远程仓库，先通过Docker搭建Gitlab
创建一个全新的虚拟机，并且至少指定4G的运行内存，4G运行内存是Gitlab推荐的内存大小。
并且安装Docker以及Docker-Compose

### 3.2 修改ssh的22端口

```
#将ssh的默认22端口，修改为60022端口，因为Gitlab需要占用22端口 vi /etc/ssh/sshd_config  PORT 22 -> 60022systemctl restart sshd
```

### 3.3 编写docker-compose.yml

```
docker-compose.yml文件去安装gitlab（下载和运行的时间比较长的） version: '3.1'services: gitlab:  image: 'twang2218/gitlab-ce-zh:11.1.4'  container_name: "gitlab"  restart: always  privileged: true  hostname: 'gitlab'  environment:   TZ: 'Asia/Shanghai'   GITLAB_OMNIBUS_CONFIG: |    external_url 'http://192.168.199.110'    gitlab_rails['time_zone'] = 'Asia/Shanghai'    gitlab_rails['smtp_enable'] = true    gitlab_rails['gitlab_shell_ssh_port'] = 22  ports:   - '80:80'   - '443:443'   - '22:22'  volumes:   - /opt/docker_gitlab/config:/etc/gitlab   - /opt/docker_gitlab/data:/var/opt/gitlab   - /opt/docker_gitlab/logs:/var/log/gitlab
```

## 十、搭建GitlabRunner

### 1.准备文件

```
daemon.json {"registry-mirrors": ["https://registry.docker-cn.com"],"insecure-registries": [ip:ports]} 文件夹 environment里面准备maven安装包，jdk1.8安装包，Dockerfile，daemon.json以及docker-compose
```

### 2.开始搭建


创建工作目录 /usr/local/docker_gitlab-runner
将docker-compose.yml文件以及environment目录全部复制到上述目录中
在宿主机启动docker程序后先执行 sudo chown root:root /var/run/docker.sock (如果重启过 docker,重新执行)
在/usr/local/docker_gitlab-runner 目录中执行docker-compose up -d –build 启动容器
添加容器权限，保证容器可以使用宿主机的dockerdocker exec -it gitlab-runner usermod -aG root gitlab-runner
注册Runner信息到gitlab

### 3.进入后续步骤

```
docker exec -it gitlab-runner gitlab-runner register # 输入 GitLab 地址Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):http://192.168.199.109/ # 输入 GitLab TokenPlease enter the gitlab-ci token for this runner:1Lxq_f1NRfCfeNbE5WRh # 输入 Runner 的说明Please enter the gitlab-ci description for this runner:可以为空 # 设置 Tag，可以用于指定在构建规定的 tag 时触发 ciPlease enter the gitlab-ci tags for this runner (comma separated):deploy # 这里选择 true ，可以用于代码上传后直接执行（根据版本，也会没有次选项）Whether to run untagged builds [true/false]:true # 这里选择 false，可以直接回车，默认为 false（根据版本，也会没有次选项）Whether to lock Runner to current project [true/false]:false # 选择 runner 执行器，这里我们选择的是 shellPlease enter the executor: virtualbox, docker+machine, parallels, shell, ssh, docker-ssh+machine, kubernetes, docker, docker-ssh:shell
```

## 十一、整合项目入门测试

### 1.创建项目


创建maven工程，添加web.xml文件，编写HTML页面

### 2.编写.gitlab-ci.yml文件

```
stages:  - test test:  stage: test  script:    - echo first test ci   # 输入的命令
```

### 3.将maven工程推送到gitlab中

```
执行git命令推送到Gitlab git push origin master
```

### 4.查看效果


可以在gitlab中查看到gitlab-ci.yml编写的内容

## 十二、完善项目配置


添加Dockerfile以及docker-compose.yml， 并修改.gitlab-ci.yml文件

### 1.创建Dockerfile

```
# DockerfileFROM daocloud.io/library/tomcat:8.5.15-jre8COPY testci.war /usr/local/tomcat/webapps
```

### 2.创建docker-compose.yml

```
# docker-compose.ymlversion: "3.1"services:  testci:    build: docker    restart: always    container_name: testci    ports:      - 8080:8080
```

### 3.修改.gitlab-ci.yml

```
# ci.ymlstages:  - test test:  stage: test  script:    - echo first test ci    - /usr/local/maven/apache-maven-3.6.3/bin/mvn package    - cp target/testci-1.0-SNAPSHOT.war docker/testci.war    - docker-compose down    - docker-compose up -d --build    - docker rmi $(docker images -qf dangling=true)
```