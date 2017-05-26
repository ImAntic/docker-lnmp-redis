# docker-lnmp-redis

Docker Images for developers(nginx,php7,mysql5.7,redis)

## Build Images
```
docker build -t antic/nginx  -f nginx/Dockerfile .
docker build -t antic/php7  -f php/Dockerfile .
docker build -t antic/mysql  -f mysql/Dockerfile .
docker build -t antic/redis  -f redis/Dockerfile .

```

## Run Container

```
docker run --name redis -p 6379:6379 -dit -v /Users/antic/docker/www/redis/data/:/data antic/redis redis-server /etc/redis.conf
docker run --name mysql -p 3306:3306 -v /Users/antic/docker/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -dit antic/mysql
docker run --name php7 -p 9000:9000  -v /Users/antic/docker/www:/var/www/html  --link mysql:mysql --link redis:redis -dit antic/php7
docker run --name nginx -p 80:80 -p 443:443 -v /Users/antic/docker/www:/var/www/html --link php7:php7 -dit antic/nginx
```


## Docker常见命令

```
docker ps  #查看正在运行的容器
dockser inspect NAMES  #查看
docker stop NAMES  #停止容器
docker start  NAMES #启动容器
docker ps -a  #查看终止状态的容器
docker rm -f NAMES  #移除正在运行的容器
docker list  #列出本地镜像
docker rmi NAMES  #删除镜像
docker exec -it NAMES /bin/bash #进已经在运行的容器
docker logs NAMES  #获取容器日志
```