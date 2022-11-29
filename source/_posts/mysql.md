---
title: mysql
date: 2022-08-18 20:02:49
tags:
---

# 在项目中使用mysql

## 安装mysql模块

```powershell
npm i mysql
```

## 导入并配置mysql模块

```javascript
// 导入mysql模块
const mysql = require('mysql')

// 连接mysql数据库
const db = mysql.createPool({
  host: '127.0.0.1',    // 连接的数据库所在ip地址
  user: 'root',         // 数据库账号
  password: 'admin123', // 数据库密码
  database: 'my_db_01', // 指定要操作哪个数据库
})
```

## 测试mysql模块能否正常工作

~~~javascript
// 测试mysql能否正常工作
db.query('SELECT 1', (err, results) => {
  if(err) return console.log(err.message);
  // 如果results能打印出 [ RowDataPacket { '1': 1 } ] 的结果，就说明数据库连接正常
  console.log(results);
})
~~~

## 查

```javascript
// 查询users表中的所有数据
const sqlStr = 'select * from users'
db.query(sqlStr, (err, results) => {
  if(err) return console.log(err.message);
  console.log(results);
})

```

## 增

```javascript
// 待插入数据
const user = { username: 'Jack', password: 'jack123' }
// sql语句，其中的?表示占位符
const sqlStr = 'insert into users (username, password) values (?, ?)'
// 使用数组的形式作为第二个参数，为?占位符指定具体的值
// 如果占位符只有一个，则可以省略数组
db.query(sqlStr, [user.username, user.password], (err, results) => {
  if (err) return console.log(err.message);
  if(results.affectedRows === 1) console.log('插入数据成功');
})
```

当user数据对象的属性，与数据表的字段一一对应时，可使用便捷写法

```javascript
// 待插入数据
const user = { username: 'Jack', password: 'jack123' }
// sql语句，其中的?表示占位符
const sqlStr = 'insert into users set ?'
// 使用数组的形式作为第二个参数，为?占位符指定具体的值
db.query(sqlStr, user, (err, results) => {
  if (err) return console.log(err.message);
  if (results.affectedRows === 1) console.log('插入数据成功');
})
```

## 改

```javascript
const user = { id: 7, username: 'aaa', password: '000' }// 要更新的数据对象
const sqlStr = 'update users set username=?, password=? where id=?'
db.query(sqlStr, [user.username, user.password, user.id], (err, results) => {
  if (err) return console.log(err.message);
  if (results.affectedRows === 1) console.log('更新数据成功！');
})
```

当user数据对象的属性，与数据表的字段一一对应时，可使用便捷写法

```javascript
const user = { id: 7, username: 'aaa', password: '000' }
const sqlStr = 'update users set ? where id=?'
db.query(sqlStr, [user, user.id], (err, results) => {
  if (err) return console.log(err.message);
  if (results.affectedRows === 1) console.log('更新数据成功！');
})
```

## 删

```javascript
const sqlStr = 'delete from users where id=?'
db.query(sqlStr, 7, (err, results) => {
  if (err) return console.log(err.message);
  if (results.affectedRows === 1) console.log('删除数据成功！');
})
```

使用status等状态字段来标记数据，以模拟删除

```javascript
const sqlStr = 'update users set status=? where id=?'
db.query(sqlStr, [1, 7], (err, results) => {
  if (err) return console.log(err.message);
  if (results.affectedRows === 1) console.log('删除数据成功！');
})
```

