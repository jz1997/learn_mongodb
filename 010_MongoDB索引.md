# MongoDB 索引

索引通常能够极大的提高查询的效率，如果没有索引，MongoDB在读取数据时必须扫描集合中的每个文件并选取那些符合查询条件的记录。

这种扫描全集合的查询效率是非常低的，特别在处理大量的数据时，查询可以要花费几十秒甚至几分钟，这对网站的性能是非常致命的。

索引是特殊的数据结构，索引存储在一个易于遍历读取的数据集合中，索引是对数据库表中一列或多列的值进行排序的一种结构

## 创建索引

MongoDB使用 `createIndex()` 方法来创建索引。

> 注意在 3.0.0 版本前创建索引方法为 db.collection.ensureIndex()，之后的版本使用了 db.collection.createIndex() 方法，ensureIndex() 还能用，但只是 createIndex() 的别名。



### 语法

```shell
db.collection.createIndex(keys, options)
```

语法中 Key 值为你要创建的索引字段，**1 为指定按升序创建索引，如果你想按降序来创建索引指定为 -1 即可**。



### 实例

```shell
# 以 name 字段创建升序索引
test> db.test.createIndex({"name": 1})
name_1
# 获取全部索引
test> db.test.getIndexes()
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  { v: 2, key: { name: 1 }, name: 'name_1' }
]
```



## 查询全部索引

使用 `db.test.getIndexes()`查询全部索引

```shell
test> db.test.getIndexes()
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  { v: 2, key: { name: 1 }, name: 'name_1' }
]
```



## 查看索引集合大小

使用 `totalIndexSize()`查看索引集合大小

```shell
test> db.test.totalIndexSize()
57344
```

