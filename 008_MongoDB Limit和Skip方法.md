# MongoDB limit 和 skip

## limit()

限制读取指定数量的数据，**如果没有设置，则显示全部数据**。

### 语法

```shell
test> db.test.find().limit(number)
```

* `number`: 查询的条数

### 实例

查询 5 条记录

```shell
test> db.test.find().limit(5)
[
  {
    _id: ObjectId("616ff09f470ce368d8e9c904"),
    name: 'zhangsan',
    age: '1'
  },
  {
    _id: ObjectId("616ff0a2470ce368d8e9c905"),
    name: 'zhangsan',
    age: '2'
  },
  {
    _id: ObjectId("616ff0ae470ce368d8e9c906"),
    name: 'zhangsan',
    age: '3'
  },
  {
    _id: ObjectId("616ff0b0470ce368d8e9c907"),
    name: 'zhangsan',
    age: '4'
  },
  {
    _id: ObjectId("616ff0b3470ce368d8e9c908"),
    name: 'zhangsan',
    age: '5'
  }
]

```





## skip()

使用 skip 方法来跳过指定数量的数据

```shell
test> db.test.find().skip(3);
[
  {
    _id: ObjectId("616ff0b0470ce368d8e9c907"),
    name: 'zhangsan',
    age: '4'
  },
  {
    _id: ObjectId("616ff0b3470ce368d8e9c908"),
    name: 'zhangsan',
    age: '5'
  }
]
```





## 实现分页

同时使用 `skip` 和 `limit` 即可实现分页

```shell
test> db.test.find().skip(0).limit(5)
[
  {
    _id: ObjectId("616ff09f470ce368d8e9c904"),
    name: 'zhangsan',
    age: '1'
  },
  {
    _id: ObjectId("616ff0a2470ce368d8e9c905"),
    name: 'zhangsan',
    age: '2'
  },
  {
    _id: ObjectId("616ff0ae470ce368d8e9c906"),
    name: 'zhangsan',
    age: '3'
  },
  {
    _id: ObjectId("616ff0b0470ce368d8e9c907"),
    name: 'zhangsan',
    age: '4'
  },
  {
    _id: ObjectId("616ff0b3470ce368d8e9c908"),
    name: 'zhangsan',
    age: '5'
  }
]
```



