# MongoDB 排序

## sort()

在 MongoDB 中使用 `sort()` 方法对数据进行排序，sort() 方法可以通过参数指定排序的字段，并使用 `1` 和 `-1` 来指定排序的方式，其中 `1` 为升序排列，而 `-1` 是用于降序排列。

### 语法

```shell
>db.COLLECTION_NAME.find().sort({KEY:1})
```

* `1`: 使用正序
* `-1`: 使用逆序

### 实例

```shell
# 逆序查询
test> db.test.find().sort({"age": -1})
[
  {
    _id: ObjectId("616ff0b3470ce368d8e9c908"),
    name: 'zhangsan',
    age: '5'
  },
  {
    _id: ObjectId("616ff0b0470ce368d8e9c907"),
    name: 'zhangsan',
    age: '4'
  },
  {
    _id: ObjectId("616ff0ae470ce368d8e9c906"),
    name: 'zhangsan',
    age: '3'
  },
  {
    _id: ObjectId("616ff0a2470ce368d8e9c905"),
    name: 'zhangsan',
    age: '2'
  },
  {
    _id: ObjectId("616ff09f470ce368d8e9c904"),
    name: 'zhangsan',
    age: '1'
  }
]
# 正序查询
test> db.test.find().sort({"age": 1})
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



> skip(), limilt(), sort()三个放在一起执行的时候，执行的顺序是先 sort(), 然后是 skip()，最后是显示的 limit()。