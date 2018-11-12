# Docker使用说明
可使用docker快速部署数据库及应用。  

- docker容器是无状态的，请使用挂载等方式将数据持久化在宿主机上，避免数据丢失。
- docker基于linux命名空间实现，在windows、macOS上运行借助虚拟机运行。
- docker-compose是用于管理多个容器的工具，基于python实现。
- 附件docker-compose.yml包含了mongo,redis,mysql,tomcat的配置，可根据需要修改。

## 使用方式

- 安装docker环境
```
yum install -y docker
yum install -y docker-compose
service start docker 
service start docker-compose
```

- 上传docker-compose.yml 。   

- 上传配置文件至/etc/{模块名}/ (如：/etc/redis/redis.conf)。   

- 构建容器并启动[...](https://docs.docker.com/compose/reference/up/)
```
# 在docker-compose.yml目录下执行，或用-f [filename...]指定配置文件。
docker-compose -d up # -d 以守护进程形式后台运行 
```

- 可使用logs查看日志[...](https://docs.docker.com/compose/reference/logs/)
```
docker-compose logs [SERVICE...]
```

- 可使用ps查看容器状态[...](https://docs.docker.com/compose/reference/ps/)
```
docker-compose ps
```

- 停止或启动容器[...](https://docs.docker.com/compose/reference/stop/)
```
docker-compose stop [SERVICE...]
docker-compose start [SERVICE...]
```

- 停止并卸载容器[...](https://docs.docker.com/compose/reference/down/)  
**此命令会将容器销毁。**
```
docker-compose down
```

## mongo
```
 mongodb:
        container_name: mongo
        image: mongo:3.6.3 # 镜像版本
        environment:
            - MONGO_INITDB_ROOT_USERNAME= # 初始账号及密码
            - MONGO_INITDB_ROOT_PASSWORD=
        volumes:
            - '/etc/mongodb/mongod.conf:/etc/mongod.conf' # 配置文件挂载 
            - '/etc/mongodb/db:/data/db' # 数据库挂载
        ports:
            - '12307:12307' # 宿主机端口：容器端口
        command:
            - '-f'
            - '/etc/mongod.conf'
```

## redis
```
redis:
    container_name: redis
    image: redis:4.0.9
    volumes:
        - '/etc/redis/redis.conf:/etc/redis/redis.conf'
        - '/etc/redis/data/:/data/'
    ports:
        - '19000:19000' # 宿主机端口：容器端口
    command:
        - '/etc/redis/redis.conf'
```

## mysql
```
mysql:
    container_name: mysql
    image: 'mysql:5.7'
    environment:
        MYSQL_ROOT_PASSWORD: # 初始密码
    volumes:
        - '/etc/mysql/my.cnf:/etc/my.cnf'
        - '/etc/mysql/data:/var/lib/mysql'
    ports:
        - '3336:3336' # 宿主机端口：容器端口
```

## tomcat
```
tomcat:
    image: tomcat:8
    container_name: tomcat
    volumes:
        # - '/etc/tomcat/conf/server.xml:/usr/local/tomcat/webapps/conf/server.xml'
        - '/etc/tomcat/webapps/:/usr/local/tomcat/webapps/' # webapps挂载
    ports:
        - '80:8080' # 宿主机端口：容器端口
```