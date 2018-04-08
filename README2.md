#### 1.基本结构
**查看是否与数据库链接：db.runCommand({ping:1})**
例如 ：
```
var userName= 'MongoDB';
var timeStamp = Date.parse(new Date()); // 生成时间戳
var jsonDataBase = {
     "loginName": userName,
    "loginTime": timeStamp
 };

 var db = connect('log'); // 链接数据库 与use log 方法一样

 db.login.insert(jsonDataBase); //插入数据

 print('[demo]log print success'); // 没有错误显示成功
```
当我们创建好数据以后，通过connect()（类似use db）链接到库，
通过db.集合.insert(xxx)插入数据，通过print来提示（类似console.log）
#### 2.修改数据
* $set  修改数据
```
var db = connect('co');
db.login.update({loginTime:'userName'},{'$set':{sex:0}})
print('修改成功')
```
* $unset  删除数据
```
var db = connect('com');
db.login.update({loginTime:'userName'},{'$unset':{sex:0}})
print('删除成功')
```
* $inc  修改数值
```
var db = connect('com');
db.login.update({loginTime:'userName'},{'$inc':{age: +2}})
print('年龄增加了2')
```
* multi 为true时表示全部修改
```
var db = connect('com');
db.login.update({loginTime:'userName'},{'$set':{'book':[]},{multi:true})
print('当前数据都添加了book这个属性')
```
* upsert 如果找不到数据时，插入这条数据（true，添加，false，不添加）
```
var db = connect('com');
db.login.update({loginTime:'xxxxx'},{'$set'{age: 20}},{upsert:true})
print('没有这条数据就添加上')
```

#### 3.修改数组的方法
* $push   追加数组
```
var db = connect('com');
db.login.update({loginTime:'userName'},{'$push':{'book':'draw'})
print('最后数据为' {book:['draw']})
```
* $pop  删除数组的值，1 从末尾，2从前端
```
db.login.update({loginTime:'userName'},{'$pop':{'book':1});
print('最后数据为' {book:[]})
```
* $addToSet 有则会修改，没有就添加
```
db.login.update({loginTime:'userName'},{'$addToSet':{'book':'MongoDB'});
print('最后数据为' {book:['MongoDB']})
```
* $each     批量添加
```
var newBook = ['code','js','css'];
db.login.update({loginTime:'userName'},{'$addToSet':{insterset:{$each:newBook}}})
print('输出数据为：book:['code','js','css']')
```
* 数组定位修改  通过.int
```
db.login.update({loginTime:'userName'},{'$set':{insterset.2:'html'})
print('输出数据为：book:['code','js','html']')
```
#### 4.find的查询修饰符
##### 筛选返回的字段（单条件查询）
```
db.workmate.find(
    {age:{$lt:30,$gt:25}},
    {name:1,_id:0}
)
```
当前代码返回的字段就是json格式，只返回name，其中1为true，0为false不返回

例子中的的符号表示
* $lt       小于
* $lte      小于等于
* $gt       大于
* $gte      大于等于
* $ne       不等于
#### 多条件查询
##### $in
```
db.workmate.find(
    {age:{$in:[$25,30]}},
    {name:true,age:true._id:false}
)
```
当前查询的就是年龄是25岁和30岁的信息
##### $or
```
db.workmate.find(
    {
    $or:[
        {age:{$gt:30}},
        {name:'XiaoWang'}
    ]
    },
    {name:1,age:1,_id:0}
)
```
当前查询的是年龄是大于30的或者姓名是小王的数据
##### $and
```
db.workmate.find(
    {
    $and:[
        {age:{$gt:30}},
        {name:'XiaoWang'}
    ]
    },
    {name:1,age:1,_id:0}
)
```
当前查询的大于30的并且是小王的数据
##### $not
```
db.workmate.find(
    {age:{
    $not:{
        $gt:20,
        $lt:30,
    }}},
    {name:true,age:true._id:false}
)
```
查询的是大于20小于30以外的值
#### 数组的查询
* 基本查询
```
* db.workmate.find({
    insterest:'看电视'
},
{name:1,age:1}
)
```
* $all多项查询
```
db.workmate.find({
    insterset:{$all:['看电视','听歌']}
})
```
* $in
```
db.workmate.find(
    {age:{$in:[$25,30]}},
    {name:true,age:true._id:false}
)
```
只要满足一项就可以查出来
* $size
```
db.workmate.find(
    {insterest:{$size:3}}
)

```
会显示出当前有3个insterest的数据

$slice 
```
db.workmate.find(
    {},
    {name:1,interest:{$slice:2},age:1,_id:0} 
)
```
会显示所有数据的interest的前两项

#### find的一些参数
* limit     返回的数量
* skip      跳过的个数
* sort      排序，1表示从小到大，-1反之。
例如 ：
```
var rs = db.workmate.find({},
    {name:true,age:true,_id:false}).limit(0).skip(2).sort({age:1});
printjson(rs)
```
当前数据返回的是按年龄从小到大排序，跳过前两条数据以后的获取的数据

#### 直接js运行，不再终端操作
* forEach方法
```
var rs = db.workmate.find()
rs.forEach(result=>{
    printjson(result)
})
```

#### 查询性能
一般我们使用find来查询数据，当数据多的时速度会很慢，因此我们通过建立索引来实现,快速查询
> 第一步建立索引
```
db.workmate.ensureIndex({name:1})
```
> 第二步查询现有索引
```
db.workmate.getIndexes()
```
> 第三步再次执行find会发现，时间缩短了
```
var rs = db.randomInfo.find({name:'14455'});
```

* 扩展
当多个索引查询的时候使用hint(),可以指定优先索引查询
* 删除索引
db.workmate.dropIndex('name_1')

#### 全文查询
**建立索引**
```
db.info.ensureIndex({content:'text'});
```
**全文查找**
```
db.info.find($text:{$search:'看花花'})
```
* $text     变色在全文索引中查找数据
* search    所要查找的数据
* 空格      当我们查找多个词时，通过空格来区分
* -         通过-来取下比如：'-看电视'，就不会查找改数据

**转义符**
```
db.info.find({$text:{$search:"\"love PlayGame\" drink"}})
```
表示搜索love playgame和drink
