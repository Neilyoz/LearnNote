# 0x02 MongoDB 练习手札

MongoDB 文档之间的关系

- 一对一 （one to one）

  - 中国法律规定的夫妻关系，一个丈夫对应一个妻子

    在MongoDB可以通过内嵌文档的方式，体现一对一的关系

实现例子：

```javascript
db.wifeAndHusbands.insertOne({
    name: 'Han Meimei',
    husband: {
        name: 'Li Lei'
    }
})

db.wifeAndHusbands.insertOne({
    name: 'Zhao Min',
    husband: {
        name: 'Zhang Wuji'
    }
})

// or 使用批量插入

db.wifeAndHusbands.insert([
    {
        name: 'Han Meimei',
        husband: {
            name: 'Li Lei'
        }
    },
    {
        name: 'Zhao Min',
        husband: {
            name: 'Zhang Wuji'
        }
    }
])
```

- 一对多 （one to many）/ 多对一 （many to one）

  - 父母 - 孩子

  - 用户 - 订单

  - 文章 - 评论

    也可以使用内嵌文档来映射一对多的关系

实现例子（用户 - 订单）：

```javascript
// 先插入两个用户
db.users.insert([
    {
        username: '李寻欢'
    },
    {
        username: '令狐冲'
    }
])

// 订单
var user_id = db.users.findOne({username: '李寻欢'})._id
db.orders.insertOne({
    user_id: user_id,
    orders: ['飞刀', '小酒仙', '牛肉']
})

var user_id = db.users.findOne({username: '令狐冲'})._id
db.orders.insertOne({
    user_id: user_id,
    orders: ['独孤九剑', '五粮液', '吸星大法']
})
```

- 多对多 （many to many）
  - 分类 - 商品
  - 老师 - 学生

实现例子（老师 - 学生）：

```javascript
// 加入老师集合
db.teachers.insert([
	{
		teacher_name: '王重阳'
	},
	{
		teacher_name: '张三丰'
	},
	{
		teacher_name: '扫地僧'
	}
])

// 查询老师
db.teachers.find()

// 添加学生，一个学生可以有多个老师
db.students.insert([
    {
        student_name: '学生1',
        teacher_ids: [
            db.teachers.findOne({teacher_name: '王重阳'})._id,
            db.teachers.findOne({teacher_name: '张三丰'})._id
        ]
    },
    {
        student_name: '学生2',
        teacher_ids: [
            db.teachers.findOne({teacher_name: '王重阳'})._id,
            db.teachers.findOne({teacher_name: '张三丰'})._id,
            db.teachers.findOne({teacher_name: '扫地僧'})._id
        ]
    }
])
```



