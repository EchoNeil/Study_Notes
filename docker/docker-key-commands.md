# Docker 关键命令速查

## 0. docker相关
  0. 可以理解docker就是虚拟机
  1. docker需要有镜像来安装环境
  2. docker镜像安装后就称为容器
  3. docker compose可以很方便的批量管理容器

## 1. 环境检查

```bash
docker --version
docker info
systemctl status docker --no-pager
systemctl is-active docker
docker compose version
```

启动 Docker 并设置开机启动：

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

## 2. 镜像

```bash
docker images #查看当前拥有的镜像文件
docker pull nginx:latest  #拉取镜像
docker image inspect nginx:latest
docker rmi nginx:latest
docker system df
```

## 3. 容器

```bash
# 创建并后台运行容器 -d表示后台运行，不占用命令行
docker run -d --name nginx-test nginx:latest

# 查看容器
docker ps   #仅跑起来的容器
docker ps -a  #所有容器，包括已经暂停的

# 启停、重启、删除
docker stop nginx-test
docker start nginx-test
docker restart nginx-test
docker rm nginx-test
docker rm -f nginx-test   #移除已经跑起来的

# 日志、详情、资源使用
docker logs nginx-test
docker logs -f nginx-test
docker logs --tail=50 nginx-test
docker inspect nginx-test
docker stats

# 进入容器
docker exec -it nginx-test sh
```

## 4. 端口映射与测试

仅允许服务器本机访问：

```bash
docker run -d --name nginx-local -p 127.0.0.1:8080:80 nginx:latest  #8080 只对本机开放，127.0.0.1:8080连接到容器的80端口
curl -I http://127.0.0.1:8080/ #-I只返回头，不返回整个东西
sudo ss -lntp | grep 8080
docker port nginx-local
docker rm -f nginx-local
```

`-p 127.0.0.1:8080:80` 表示宿主机本机 `8080` 转发到容器 `80`。不写 `-p` 则不会通过端口映射发布到宿主机。

## 5. Docker 网络

```bash
docker network ls
docker network create my-net
docker network inspect my-net

docker run -d --name web-a --network my-net nginx:latest
docker run -d --name web-b --network my-net nginx:latest

# 从 web-a 访问 web-b
docker exec web-a sh -c "wget -qO- http://web-b"

# 清理实验容器和网络
docker rm -f web-a web-b
docker network rm my-net
```

## 6. 数据卷

```bash
docker volume create my-data
docker volume ls
docker volume inspect my-data

# 写入数据
docker run --rm --mount source=my-data,target=/data alpine:latest \
  sh -c 'echo "hello Docker" > /data/test.txt'

# 读取数据
docker run --rm --mount source=my-data,target=/data alpine:latest \
  cat /data/test.txt

# 删除数据卷（会删除其中的数据）
docker volume rm my-data
```

## 7. Docker Compose

在包含 `compose.yaml` 的项目目录中执行：

```bash
docker compose version
docker compose config
docker compose up -d    #启动compose
docker compose up -d --build  #顺便搭建环境
docker compose ps
docker compose logs
docker compose logs -f web
docker compose stop
docker compose start
docker compose restart
docker compose down   #直接remove容器
```

## 8. 网站排查

```bash
docker ps -a
docker compose ps
docker compose logs --tail=100
docker compose config
docker port 容器名
sudo ss -lntp
curl -I http://127.0.0.1:8080/
docker system df
```

## 9. SSH 隧道（在自己的电脑上执行）

```bash
ssh -L 8080:127.0.0.1:8080 root@你的服务器公网IP #-L建立端口转发
```

保持 SSH 连接，然后在本机浏览器打开 `http://127.0.0.1:8080`。

> 注意：谨慎使用 `docker system prune`、`docker volume rm` 和 `docker rm -f`。删除前先确认目标资源不再需要。
