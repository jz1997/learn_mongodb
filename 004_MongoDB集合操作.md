# MongoDB集合操作

## 创建集合

### 命令

```shell
db.createCollection(name, options)
```

* `name`: 集合名称
* `options`:可选参数, 指定有关内存大小及索引的选项

`options` 参数列表

| 字段        | 类型 | 描述                                                         |
| :---------- | :--- | :----------------------------------------------------------- |
| capped      | 布尔 | （可选）如果为 true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。 **当该值为 true 时，必须指定 size 参数。** |
| autoIndexId | 布尔 | 3.2 之后不再支持该参数。（可选）如为 true，自动在 _id 字段创建索引。默认为 false。 |
| size        | 数值 | （可选）为固定集合指定一个最大值，即字节数。 **如果 capped 为 true，也需要指定该字段。** |
| max         | 数值 | （可选）指定固定集合中包含文档的最大数量。                   |



### 实例

在 `test` 中创建 `student`集合

```shell
# 选中 test
test> use test
already on db test

# 创建 student 集合
test> db.createCollection("student")
{ ok: 1 }

# 查看全部集合
test> show collections
student
```



**自动创建集合**

在 MongoDB 中，不需要先创建集合，查询文档的时候会自动创建集合

```shell
# 往 teacher 集合中插入数据
test> db.teacher.insert({"name": "zhangsan", "age": "22"})
{
  acknowledged: true,
  insertedIds: { '0': ObjectId("616ea48f9b2d824f6261e444") }
}
# 查看当前全部集合
test> show collections
student
teacher
# 查询集合中的数据
test> db.teacher.find()
[
  {
    _id: ObjectId("616ea48f9b2d824f6261e444"),
    name: 'zhangsan',
    age: '22'
  }
]
```



## 删除集合

### 语法

```shell
db.collection.drop()
```



### 实例

```shell
# 当前集合列表
test> show collections
demo_collection
student
teacher

# 删除 collection_demo 集合
test> db.demo_collection.drop()
true

# 删除后的集合列表
test> show collections
student
teacher

```

