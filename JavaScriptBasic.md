## 重新阅读《JavaScript高级程序设计》的一些理解与感受
### 1. 数据类型
1. 基本数据类型
   - Undefined
   - String
   - Number
   - Boolean
   - Null
2. 引用数据类型
   - Object
3. typeof操作符

   数据类型|typeof返回值
   :---:|:---:
   未定义|undefined
   数值|number
   字符串|string
   布尔值|boolean
   函数|function
   对象或null|object
 
4. `undefined == null`，返回的是true。
5. 不同数据类型与布尔值的转换
 
   数据类型|转换为true|转换为false
   :---:|:---:|:---:
   Boolean|true|false
   String|非空字符串|''（空字符串）
   Number|非零数值（包括无穷大）|0或NaN
   Object|任何对象|null
   Undefined|N/A|undefined
   
6. null表示空对象指针，如果一个变量定义用来报存对象，那么最好将该变量初始化为null。
 
 