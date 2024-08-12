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

4. connect 函数用法示例：
```
```