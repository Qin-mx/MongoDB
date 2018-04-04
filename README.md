### 安装
* [安装教程](http://www.runoob.com/mongodb/mongodb-window-install.html)
* 安装完成以后
在d盘创建data\db文件夹以后
通过命令 启动
```
cd C:\Program Files\MongoDB\Server\3.6\bin //先进入
C:\Program Files\MongoDB\Server\3.6\bin\mongod --dbpath d:\data\db 
```
* 再启动一个cmd ，输入mongo连接数据库
### Mongo常用命令
[学习链接](http://jspang.com/2017/12/16/mongdb/)

**在MongoDB中使用语法javascript输出不用console.log().而是用print()**
**基本命令1**
* show dbs  查看已有数据库，默认有admin config local
* use admin  进入数据库，成功提示 ： switched to db admin
* show collections  显示数据库中的集合
* db 显示当前位置

**基本命令2**
* use db  ：如果没有当前库，会创建一个库，通过show collectios发现为空，说明集合为空，因此查询不出
* db.集合.insert() ：新建数据集合插入数据 
* db.集合.find() ：查看集合的所有数据
* db.集合.findOne() ： 查询第一个数据
* db.集合.update({查询},{修改}) ：第一个是查询条件，第二个是要修改成的值
* db.集合.remove({查询})：删除指定的数据
* db.集合.drop() : 删除当前集合
* db.dropDatabase() : 删除整个数据库

MongoDB的使用方式
#### 1.基本结构
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