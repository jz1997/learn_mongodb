# MongoDB文档操作

## 插入文档

### 语法

#### save()、insert()

```shell
db.collection_name.insert(document)
# or
db.collection_name.save(document)
```

* `save()`: 如果 `_id` 存在就更新数据，不存在就插入数据。在新版本中已经被废弃，可以使用 `insertOne()` 或者 `insertMany()`
* `insert()`: 若插入的数据主键已经存在，则会抛 `org.springframework.dao.DuplicateKeyException` 异常，提示主键重复，不保存当前数据。



#### **insertOne()**

```shell
db.collection.insertOne(
   <document>,
   {
      writeConcern: <document>
   }
)
```



#### **insertMany()**

```shell
db.collection.insertMany(
   [ <document 1> , <document 2>, ... ],
   {
      writeConcern: <document>,
      ordered: <boolean>
   }
)
```



**参数说明**

* `document`： 文档
* `writeConcern`: 写入策略，默认为 1，即要求确认写操作，0 是不要求
* `ordered`: 是否按照顺序写入，默认 true，按顺序写入

### 实例

```shell
# 添加文档
> db.test.insert({"name": "zhangsan", "age": 25, "sex": "男"})
WriteResult({ "nInserted" : 1 })

# 查询全部文档
> db.test.find()
{ "_id" : ObjectId("616fd6c9a98f5212630bf999"), "name" : "zhangsan", "age" : 25, "sex" : "男" }
```



可以将数据定义为变量

```shell
# 定义变量
> d = ({"name":"jiangzheng", "age": 22})
# 输出结果
{ "name" : "jiangzheng", "age" : 22 }
```

将上面定义的变量保存进集合中

```shell
# 插入上面定义的变量
> db.test.insert(d)
WriteResult({ "nInserted" : 1 })
# 查询全部文档
> db.test.find()
{ "_id" : ObjectId("616fd6c9a98f5212630bf999"), "name" : "zhangsan", "age" : 25, "sex" : "男" }
{ "_id" : ObjectId("616fd7a8a98f5212630bf99a"), "name" : "jiangzheng", "age" : 22 }
```





## 更新文档

### update()

#### 语法

更新已经存在的文档

```shell
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```

**参数说明**:

- **query** : update的查询条件，类似sql update查询内where后面的。
- **update** : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
- **upsert** : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
- **multi** : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
- **writeConcern** :可选，抛出异常的级别。

#### 实例

```shell
# 进行匹配更新
> db.test.update({ "_id" : ObjectId("616fd7a8a98f5212630bf99a") }, {$set: {name:"lisi", "age": 30, "sex": "女"}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

# 查询更新后的信息
> db.test.find()
{ "_id" : ObjectId("616fd6c9a98f5212630bf999"), "name" : "zhangsan", "age" : 25, "sex" : "男" }
{ "_id" : ObjectId("616fd7a8a98f5212630bf99a"), "name" : "lisi", "age" : 30, "sex" : "女" }
```

### save()

#### 语法

传入文档来替换已有的文档，如果 `_id` 存在就更新，否则就新增

```shell
db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)
```

**参数说明：**

- **document** : 文档数据。
- **writeConcern** :可选，抛出异常的级别。

#### 实例

```shell
# 查询当前集合
> db.test.find()
{ "_id" : ObjectId("616fdb68a98f5212630bf99b"), "name" : "zhangsan", "age" : 18 }
# _id 存在进行更新
> db.test.save({ "_id" : ObjectId("616fdb68a98f5212630bf99b"), "name" : "zhangsan", "age" : 19 })
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
# 查询
> db.test.find()
{ "_id" : ObjectId("616fdb68a98f5212630bf99b"), "name" : "zhangsan", "age" : 19 }
# _id 不同就新增
> db.test.save({ "_id" : ObjectId("616fdb68a98f5212630bf99c"), "name" : "lisi", "age" : 19 })
WriteResult({
	"nMatched" : 0,
	"nUpserted" : 1,
	"nModified" : 0,
	"_id" : ObjectId("616fdb68a98f5212630bf99c")
})
# 查询
> db.test.find()
{ "_id" : ObjectId("616fdb68a98f5212630bf99b"), "name" : "zhangsan", "age" : 19 }
{ "_id" : ObjectId("616fdb68a98f5212630bf99c"), "name" : "lisi", "age" : 19 }
```



## 删除文档

### deleteOne()

```shell
# 删除之前的文档
test> db.test.find()
[
  {
    _id: ObjectId("616fdb68a98f5212630bf99b"),
    name: 'zhangsan',
    age: 20
  },
  { _id: ObjectId("616fdb68a98f5212630bf99c"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe138470ce368d8e9c8f0"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13a470ce368d8e9c8f1"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13a470ce368d8e9c8f2"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13b470ce368d8e9c8f3"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13c470ce368d8e9c8f4"), name: 'lisi', age: 19 }
]

# 删除一个 name = "zhangsan" 的文档
test> db.test.deleteOne({ "name": "zhangsan" })
{ acknowledged: true, deletedCount: 1 }

# 删除之后的文档
test> db.test.find()
[
  { _id: ObjectId("616fdb68a98f5212630bf99c"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe138470ce368d8e9c8f0"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13a470ce368d8e9c8f1"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13a470ce368d8e9c8f2"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13b470ce368d8e9c8f3"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13c470ce368d8e9c8f4"), name: 'lisi', age: 19 }
]

```



### deleteMany()

删除名叫`lisi`的文档

```shell
test> db.test.find()
[
  { _id: ObjectId("616fdb68a98f5212630bf99c"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe138470ce368d8e9c8f0"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13a470ce368d8e9c8f1"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13a470ce368d8e9c8f2"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13b470ce368d8e9c8f3"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe13c470ce368d8e9c8f4"), name: 'lisi', age: 19 },
  { _id: ObjectId("616fe332470ce368d8e9c8f5"), name: 'zhangsan' },
  { _id: ObjectId("616fe333470ce368d8e9c8f6"), name: 'zhangsan' },
  { _id: ObjectId("616fe334470ce368d8e9c8f7"), name: 'zhangsan' },
  { _id: ObjectId("616fe335470ce368d8e9c8f8"), name: 'zhangsan' },
  { _id: ObjectId("616fe335470ce368d8e9c8f9"), name: 'zhangsan' }
]

test> db.test.deleteMany({"name":"lisi"})
{ acknowledged: true, deletedCount: 6 }

test> db.test.find()
[
  { _id: ObjectId("616fe332470ce368d8e9c8f5"), name: 'zhangsan' },
  { _id: ObjectId("616fe333470ce368d8e9c8f6"), name: 'zhangsan' },
  { _id: ObjectId("616fe334470ce368d8e9c8f7"), name: 'zhangsan' },
  { _id: ObjectId("616fe335470ce368d8e9c8f8"), name: 'zhangsan' },
  { _id: ObjectId("616fe335470ce368d8e9c8f9"), name: 'zhangsan' }
]
```



删除全部文档

```shell
test> db.test.deleteMany({})
{ acknowledged: true, deletedCount: 5 }
test> db.test.find()
```



## 查询文档

### 语法

```shell
db.collection.find(query, projection)
```

- **query** ：可选，使用查询操作符指定查询条件
- **projection** ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

格式化结果

```shell
# pretty() 用来格式化文档显示
>db.col.find().pretty()
```

> 除了 find() 方法之外，还有一个 findOne() 方法，它只返回一个文档。



### MongoDB 与 RDBMS Where 语句比较

通过下表可以更好的理解 MongoDB 的条件语句查询：

| 操作       | 格式                     | 范例                                        | RDBMS中的类似语句       |
| :--------- | :----------------------- | :------------------------------------------ | :---------------------- |
| 等于       | `{<key>:<value>`}        | `db.col.find({"by":"菜鸟教程"}).pretty()`   | `where by = '菜鸟教程'` |
| 小于       | `{<key>:{$lt:<value>}}`  | `db.col.find({"likes":{$lt:50}}).pretty()`  | `where likes < 50`      |
| 小于或等于 | `{<key>:{$lte:<value>}}` | `db.col.find({"likes":{$lte:50}}).pretty()` | `where likes <= 50`     |
| 大于       | `{<key>:{$gt:<value>}}`  | `db.col.find({"likes":{$gt:50}}).pretty()`  | `where likes > 50`      |
| 大于或等于 | `{<key>:{$gte:<value>}}` | `db.col.find({"likes":{$gte:50}}).pretty()` | `where likes >= 50`     |
| 不等于     | `{<key>:{$ne:<value>}}`  | `db.col.find({"likes":{$ne:50}}).pretty()`  | `where likes != 50`     |



### MongoDB And 查询条件

#### 语法

MongoDB 的 find() 方法可以传入多个键(key)，每个键(key)以逗号隔开，即常规 SQL 的 AND 条件。

```shell
>db.collection.find({key1:value1, key2:value2}).pretty()
```



#### 实例

```shell
test> db.test.find({"name": "wangwu", "age": 19})
[
  {
    _id: ObjectId("616fe54e470ce368d8e9c8fd"),
    name: 'wangwu',
    age: 19
  }
]
```



> 以上实例中类似于 WHERE 语句：**WHERE name='wangwu' AND age=19**



### MongoDB OR 查询条件

#### 语法

```shell
>db.col.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```



#### 实例

```shell
test> db.test.find({$or: [{"name": "wangwu"}, {"name": "lisi"}]})
[
  { _id: ObjectId("616fe514470ce368d8e9c8fa"), name: 'lisi' },
  { _id: ObjectId("616fe516470ce368d8e9c8fb"), name: 'lisi' },
  { _id: ObjectId("616fe516470ce368d8e9c8fc"), name: 'lisi' },
  {
    _id: ObjectId("616fe54e470ce368d8e9c8fd"),
    name: 'wangwu',
    age: 19
  }
]
```



#### AND 和 OR 联合使用

此查询类似于 `where age = 19 and (name = 'wangwu' or name = 'lisi')`

```shell
test> db.test.find({"age": 19, $or: [{"name": "wangwu"}, {"name": "lisi"}]})
[
  {
    _id: ObjectId("616fe54e470ce368d8e9c8fd"),
    name: 'wangwu',
    age: 19
  }
]
```

