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
