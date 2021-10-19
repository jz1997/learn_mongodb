# Docker Compose 安装 Mongo

## 1. docker-compose.yml

```yaml
version: '2'
services:
  mongodb:
    image: mongo
    ports:
        - 27017:27017
    volumes:
        - "./data/configdb:/data/configdb"
        - "./data/db:/data/db"
    command: mongod --auth
    environment:
      - MONGO_INITDB_ROOT_USERNAME=root       #初始化管理员用户名和密码
      - MONGO_INITDB_ROOT_PASSWORD=123456
    tty: true
```

在 `docker-compose.yml` 目录下使用`docker-compose up -d` 命令进行启动