# linux 环境构建

## VirtualBox

VM环境

## Vagrant

成品镜像

```bash
$ vagrant box add centos-7 ./centos-7.0-x86_64.box
$ vagrant init centos-7
$ vagrant up
$ vagrant ssh
```

修改端口
![20200411145929](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411145929.png)

管理员

```bash
$ su root
# 密码 vagrant
$ whoami
```

> 入门案例：[https://www.cnblogs.com/lawsssscat/p/12676477.html](https://www.cnblogs.com/lawsssscat/p/12676477.html)
> 修改语言：[https://blog.csdn.net/liupeifeng3514/article/details/79005568](https://blog.csdn.net/liupeifeng3514/article/details/79005568)




## docker

虚拟容器

安装：官网教程 [https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/) (不推荐)

```bash
# 推荐
$ sudo yum install docker
```

开启

```bash
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

阿里云容器镜像服务：[https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)

```bash
$ sudo mkdir -p /etc/docker
$ sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://eslh5xx9.mirror.aliyuncs.com"]
}
EOF
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

## MySQL

启动mysql

>异常处理：[error creating overlay mount to /var/lib/docker/overlay2](https://www.cnblogs.com/lfxiao/p/9814903.html)

```bash
$ sudo docker pull mysql:5.7
$ sudo docker images 

$ sudo docker run --name mysql --privileged=true -p 3307:3306 \
-v /dev/mydata/mysql/log:/var/log/mysql \
-v /dev/mydata/mysql/data:/var/lib/mysql \
-v /dev/mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7 
```

* `-v`   文件目录（卷）映射
* `/dev/mydata/mysql/log:/var/log/mysql` 日志映射到本地
* `/dev/mydata/mysql/data:/var/lib/mysql` 数据库数据映射到本地
* `/dev/mydata/mysql/conf:/etc/mysql` 配置文件映射到本地
* `MYSQL_ROOT_PASSWORD=root` （必须）设置数据库密码





![20200411141732](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411141732.png)

![20200411142940](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411142940.png)

进入mysql

```bash
docker exec -it mysql /bin/bash
```




![20200411143318](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411143318.png)


> Docker官网关于MySQL:5.7：[https://hub.docker.com/\_/mysql](https://hub.docker.com/_/mysql)
> Docker官网关于MySQL:5.7的Dockerfile：[https://github.com/docker-library/mysql/blob/d284e15821ac64b6eda1b146775bf4b6f4844077/5.7/Dockerfile](https://github.com/docker-library/mysql/blob/d284e15821ac64b6eda1b146775bf4b6f4844077/5.7/Dockerfile)


修改编码
```bash
vim  /dev/mydata/mysql/conf/my.cnf
```
```bash
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqld]
init_connect='SET collation_connection=utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
```



重启mysql ，并进入
```bash
 docker restart mysql
 docker exec -it mysql /bin/bash
```

![20200411145025](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411145025.png)



## redis

安装&运行

```bash
$ sudo docker pull redis

# 坑：若不加（下面 touch...），redis.conf会被认作目录挂载
$ mkdir -p /dev/mydata/redis/conf
$ touch /dev/mydata/redis/conf/redis.conf
$ sudo docker run --name redis \
-v /dev/mydata/redis/data:/data \
-v /dev/mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-p 6379:6379  \
-d redis redis-server /etc/redis/redis.conf 
```

客户端登录
```bash
docker exec -it redis redis-cli
```

![20200411190534](https://raw.githubusercontent.com/LawssssCat/piggo-vscode/master/images/20200411190534.png)

开启数据持久化

```bash
vim /dev/mydata/redis/conf/redis.conf
```
输入
```bash
# 启用AOF持久化
appendonly yes
```
```bash
docker restart redis
```

> +  Redis Desktop Manager 可视化工具
> + redis 系列: [https://blog.csdn.net/LawssssCat/article/details/105087326](https://blog.csdn.net/LawssssCat/article/details/105087326)