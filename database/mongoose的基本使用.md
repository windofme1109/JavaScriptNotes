# Mongoose 的基本使用

## 1. 参考文档

1. [mongoose 官方文档](https://mongoosejs.com/docs/)

2. [mongoose学习教程](https://huxinmin.github.io/post/mongoose-xue-xi-jiao-cheng/)

## 2. 安装 Mongodb

1. 请参考：
   - [MongoDB数据库在 Mac OS，Windows 及 Linux 的安装指南](https://mdnice.com/writing/526f6b938b514d03b97b024b5e14955a)
   - [Mac安装MongoDB（一步到位）](https://juejin.cn/post/7260396933649334309)
   - [Install MongoDB Community Edition](https://www.mongodb.com/docs/manual/administration/install-community/)

## 3. 基本说明

1. 从7.0.0 版本开始， mongoose 移除了对回调的支持。转而全面支持 Promise。本文档以 8.5.2 版本为基础，全面介绍基于 Promise 的 mongoose 的使用方式。

2. 7.0.0-rc0 发版记录：
![alt text](./images/mongoose-7.0.0-rc0.png)

3. 相关 issue：[Dropping callbacks support in v7 #11431](https://github.com/Automattic/mongoose/issues/11431)

## 3. 连接数据库

1. 最基本的连接方式：
```js
import mongoose from 'mongoose';

mongoose.connect('mongodb://127.0.0.1:27017/test');
```

2. mongoose 提供了 connect 函数用来连接数据库，其接收两个参数：
   - mongodb uri：连接 mongodb 的 uri，其格式为：
```
mongodb://[username:password@]host1[:port1][,...hostN[:portN]][/[defaultauthdb][?options]]
```
     关于 uri 的详情可参考官方文档： 
[standard-connection-string-format](https://www.mongodb.com/docs/manual/reference/connection-string/#standard-connection-string-format)

   - options：连接的配置参数，这个配置参数会传给MongoDB 驱动的 connect() 函数。配置参数详情可参考：
[connect](https://mongoosejs.com/docs/api/mongoose.html#Mongoose.prototype.connect())

   - callback：回调函数，在初始的连接完成以后，会调用回调函数。**注意：** callback 和 options 参数是二选一的，也就是作为 connect 函数的第二个参数，要么传 options，要么传 callback。

3. connect 函数的返回值是 Promise，该 Promise 对象包裹的值为 Mongoose 实例。如果建立连接成功，则 Promise 会变为成功状态。

4. connect 函数用法示例 - 基本形式：
```js

mongoose.connect('mongodb://user:pass@127.0.0.1:port/database');

```
5. connect 函数用法示例 - uri 形式连接：
```js

// replica sets
const uri = 'mongodb://user:pass@127.0.0.1:port,anotherhost:port,yetanother:port/mydatabase';
mongoose.connect(uri);

```

6. connect 函数用法示例 - 使用 options 进行配置：
```js
// replica sets
const uri = 'mongodb://user:pass@127.0.0.1:port,anotherhost:port,yetanother:port/mydatabase';

// with options
mongoose.connect(uri, options);

```

7. connect 函数用法示例 - 使用回调函数：
```js

// optional callback that gets fired when initial connection completed
const uri = 'mongodb://nonexistent.domain:27000';

mongoose.connect(uri, function(error) {
  // if error is truthy, the initial connection failed.
})
```

8. 因为 connect 函数的返回值是 Promise，因此还可以使用 `ayanc/await` 实现连接数据库：
```js
import mongoose from 'mongoose';
async function main() {
    await mongoose.connect('mongodb://user:pass@127.0.0.1:port/database');
    
    // 连接成功以后，做其他的工作
}

main().then(err => {console.log(err)})

```
9. 数据库连接成功以后，connect 函数返回的 Promise 转换为成功状态（resolve），此时我们就可以继续进行其他工作，如增删改查等。如果连接数据库失败，则 Promise 转换为失败状态（reject）。

## 4. 创建 Model

### 1. Schema

### 2. Collection

## 5. 新增数据

## 6. 查询数据

## 7. 修改数据

## 8. 删除数据

## 9. Mongodb 操作符

