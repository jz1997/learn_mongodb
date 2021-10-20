# MongoDB 数据库操作

## 创建数据库

### 语法

```shell
use DATABASE_NAME;
```

**注意**: 如果数据库不存在，则创建数据库，否则切换到指定数据库。



## 实例

以下命令创建 `test`数据库

```shell
admin> use test;
switched to db test
test> db;
test
```



**查看所有数据库**：

```shell
test> show dbs;
admin     152 kB
config   36.9 kB
fed_csb   418 kB
local    73.7 kB
```

其中可能需要管理员权限，切换 `admin` 数据库, 使用 `db.auth(username, password)` 进行授权即可：

```shell
test> use admin;
switched to db admin
admin> db.auth("root", "123456");
{ ok: 1 }
```



>注意：
>
>* MongoDB 的默认数据库为 `test`，如果没有创建新的数据库，就直接使用 test 数据库。
>
>* MongoDB 中**集合只有在元素插入的时候才会创建**，也就是说创建数据库之后，需要查询一个文档、记录，集合才会真的创建。





## 删除数据库

### 语法

```shell
db.dropDatabase();
```

删除当前的数据库，默认为 `test`, 可以使用 `db` 命令查看当前的数据库。



### 实例

```shell
# 展示全部数据库
test> show dbs;
admin     152 kB
config   73.7 kB
fed_csb   418 kB
local    73.7 kB
test       41 kB

# 查看当前数据库
test> db
test

# 当前删除数据库
test> db.dropDatabase()
{ ok: 1, dropped: 'test' }

# 查看删除后的全部数据库
test> show dbs
admin     152 kB
config   73.7 kB
fed_csb   418 kB
local    73.7 kB
```

