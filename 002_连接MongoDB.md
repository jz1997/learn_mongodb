# 连接 MongoDB

## 进入 MongoDB 容器

使用命令 `docker exec -it 7ca9f9867f0d /bin/bash` 
进入docker容器。

## 使用 MongoSh 连接 MongoDB

### 链接命令

**格式:**

`mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
`

**参数解释:**

* `mongodb://`: 固定格式
* `username`: 用户名
* `password`: 密码
* `host1`: 必须的指定至少一个host, host1 是这个URI唯一要填写的。它指定了要连接服务器的地址。如果要连接复制集，请指定多个主机地址。
* `port`: 指定端口
* `database`: 连接的数据库, **不指定默认连接 `test`**

**样例:**

`mongosh mongodb://root:123456@localhost/admin`