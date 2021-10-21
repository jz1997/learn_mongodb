# MongoDB条件操作符

## 描述

条件操作符用于比较两个表达式并从mongoDB集合中获取数据。

MongoDB中条件操作符有：

- (>) 大于 - $gt
- (<) 小于 - $lt
- (>=) 大于等于 - $gte
- (<= ) 小于等于 - $lte



## 实例

### 单个使用（$gt、$gte、$lt、$lte）

获取 年龄大于19的数据

```shell
test> db.test.find({"age": {$gte: 19}})
[
  {
    _id: ObjectId("616fe54e470ce368d8e9c8fd"),
    name: 'wangwu',
    age: 19
  }
]

```



### 组合使用

查询 age 在 19 ~ 20 之间的用户

```shell
test> db.test.find({"age": {$gte: 19, $lte: 20}})
[
  {
    _id: ObjectId("616fe54e470ce368d8e9c8fd"),
    name: 'wangwu',
    age: 19
  }
]

```

