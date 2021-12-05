<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Node 中的 buffer 模块](#node-%E4%B8%AD%E7%9A%84-buffer-%E6%A8%A1%E5%9D%97)
  - [1. 参考资料](#1-%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
  - [2. Buffer 的基本说明](#2-buffer-%E7%9A%84%E5%9F%BA%E6%9C%AC%E8%AF%B4%E6%98%8E)
    - [1. Node 中的 Buffer](#1-node-%E4%B8%AD%E7%9A%84-buffer)
    - [2. Buffer 的解释](#2-buffer-%E7%9A%84%E8%A7%A3%E9%87%8A)
    - [3. Buffer 与 Stream](#3-buffer-%E4%B8%8E-stream)
    - [4. Buffer 与 ArrayBuffer](#4-buffer-%E4%B8%8E-arraybuffer)
      - [1. 二进制数组](#1-%E4%BA%8C%E8%BF%9B%E5%88%B6%E6%95%B0%E7%BB%84)
      - [2. ArrayBuffer](#2-arraybuffer)
      - [3. TypedArray](#3-typedarray)
      - [4. DataView - 视图](#4-dataview---%E8%A7%86%E5%9B%BE)
      - [5. Buffer 与 ArrayBuffer](#5-buffer-%E4%B8%8E-arraybuffer)
    - [4. Buffer 中的编码格式](#4-buffer-%E4%B8%AD%E7%9A%84%E7%BC%96%E7%A0%81%E6%A0%BC%E5%BC%8F)
      - [1. Node 支持的字符集编码](#1-node-%E6%94%AF%E6%8C%81%E7%9A%84%E5%AD%97%E7%AC%A6%E9%9B%86%E7%BC%96%E7%A0%81)
        - [1. utf8](#1-utf8)
        - [2. utf16le](#2-utf16le)
        - [3. latin1](#3-latin1)
      - [2. Node 支持的二进制转文本的编码](#2-node-%E6%94%AF%E6%8C%81%E7%9A%84%E4%BA%8C%E8%BF%9B%E5%88%B6%E8%BD%AC%E6%96%87%E6%9C%AC%E7%9A%84%E7%BC%96%E7%A0%81)
        - [1. base64](#1-base64)
        - [2. base64url](#2-base64url)
        - [3. hex](#3-hex)
      - [3. Node 支持的遗留的编码](#3-node-%E6%94%AF%E6%8C%81%E7%9A%84%E9%81%97%E7%95%99%E7%9A%84%E7%BC%96%E7%A0%81)
        - [1. ascii](#1-ascii)
        - [2. binary](#2-binary)
        - [2. ucs2](#2-ucs2)
  - [3. Buffer 常用的 API](#3-buffer-%E5%B8%B8%E7%94%A8%E7%9A%84-api)
    - [1. Buffer.alloc(size[, fill[, encoding]])](#1-bufferallocsize-fill-encoding)
    - [2. Buffer.allocUnsafe(size)](#2-bufferallocunsafesize)
    - [3. Buffer.from(array)](#3-bufferfromarray)
    - [4. Buffer.from(arrayBuffer[, byteOffset[, length]])](#4-bufferfromarraybuffer-byteoffset-length)
    - [5. Buffer.from(buffer)](#5-bufferfrombuffer)
    - [6. Buffer.from(object[, offsetOrEncoding[, length]])](#6-bufferfromobject-offsetorencoding-length)
    - [7. Buffer.from(string[, encoding])](#7-bufferfromstring-encoding)
    - [8. Buffer.concat(list[, totalLength])](#8-bufferconcatlist-totallength)
    - [10. buf.toJSON()](#10-buftojson)
    - [11. buf.slice([start[, end]])](#11-bufslicestart-end)
    - [12 buf.values()](#12-bufvalues)
    - [13. buf.write(string[, offset[, length]][, encoding])](#13-bufwritestring-offset-length-encoding)
    - [14. buf.copy(target[, targetStart[, sourceStart[, sourceEnd]]])](#14-bufcopytarget-targetstart-sourcestart-sourceend)
  - [4. Buffer 与 Stream 的转换](#4-buffer-%E4%B8%8E-stream-%E7%9A%84%E8%BD%AC%E6%8D%A2)
    - [1. fs.createWriteStream](#1-fscreatewritestream)
    - [2. Transform](#2-transform)
    - [3. Duplex](#3-duplex)
    - [4. Readable](#4-readable)
    - [5. Writable](#5-writable)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Node 中的 buffer 模块

## 1. 参考资料

1. [1127-buffer定义及常用方法 & 进制转换 & base64转码规则](https://juejin.cn/post/6844903726654865416)

2. [Nodejs 进阶：核心模块 Buffer 常用 API 使用总结](https://juejin.cn/post/6960843000939282469)

3. [[1.3万字] 玩转前端二进制](https://juejin.cn/post/6846687590783909902)

4. [JS 的二进制家族：base64、File、Blob、ArrayBuffer 的关系](https://juejin.cn/post/6844904149809627149)

5. [前端下载普通文件与二进制流文件](https://juejin.cn/post/6878912072780873742)

6. [我妈都看得懂的 Buffer 基础](https://juejin.cn/post/6911487429471895560)

7. [[译]一篇帮你彻底弄懂NodeJs中的Buffer](https://juejin.cn/post/6844903688188723208)

8. [javascript处理二进制之ArrayBuffer](https://juejin.cn/post/6844903759064088590)

9. [JS中的二进制数据处理](https://juejin.cn/post/6954281377080541221)

10. [浅析ArrayBuffer、TypedArray和Buffer](https://juejin.cn/post/6844903889364336654)

11. [ArrayBuffer - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)

12. [NodeJS的Buffer和ArrayBuffer的异同](https://blog.y2nk4.com/read/translated-difference-between-buffer-and-arraybuffer.html)

13. [arraybuffer](https://es6.ruanyifeng.com/#docs/arraybuffer)

14. [理解Node中的Buffer与stream](https://zhuanlan.zhihu.com/p/368045575)

15. [Nodejs将Buffer转化成Stream](https://www.cnblogs.com/zhangjpn/p/7968005.html)

16. [How to pass a Buffer as argument of fs.createReadStream](https://stackoverflow.com/questions/45891242/how-to-pass-a-buffer-as-argument-of-fs-createreadstream/45891702#45891702)

17. [How to wrap a buffer as a stream2 Readable stream?](https://stackoverflow.com/questions/16038705/how-to-wrap-a-buffer-as-a-stream2-readable-stream#16039177)

## 2. Buffer 的基本说明

### 1. Node 中的 Buffer

2. 官网上是这样对 buffer 进行说明的：
   > Buffer objects are used to represent a fixed-length sequence of bytes. Many Node.js APIs support Buffers.
   >
   > The Buffer class is a subclass of JavaScript's Uint8Array class and extends it with methods that cover additional use cases. Node.js APIs accept plain Uint8Arrays wherever Buffers are supported as well.
   >
   > The Buffer class is within the global scope, making it unlikely that one would need to ever use require('buffer').Buffer.

3. 翻译过来，就是说：
   - Buffer 对象用来表示固定长度的字节序列，许多 Node.js 的 API 支持 Buffer。
   - Buffer 类是 JavaScript 中的 Uint8Array 类的子类，并使用覆盖其他用例的方法对其进行扩展，Node.js 的 API 支持纯的 Uint8Array，而 Buffer 也支持。
   - 全局环境下存在 Buffer 类，因此，我们无需导入即可使用 Buffer。

4. JavaScript 语言没有读取或操作二进制数据流的机制。 Buffer 类被引入作为 Node.js API 的一部分，使其可以在 TCP 流或文件系统操作等场景中处理二进制数据流。也就是说，引入了 Buffer，使得我们可以使用 Node.js 的 API 直接操作二进制数据。


### 2. Buffer 的解释

1. Buffer 的含义是缓冲区，用来缓冲二进制数据。众所周知，计算机中的图片、视频、音频等各种文件，本质上都是一堆二进制的数据：`0100111000101...`。处理这些数据，需要将这些数据传送到 cpu 或者 gpu 那里。

2. 一般来说，如果传送数据的速度和 cpu 处理的速度相当，即传来多少数据，cpu 就能处理多少数据，这种情况就不会浪费时间。但是，这样涉及到一个问题，就是如果传送数据的速度快于 cpu 处理的速度，那么就会有一部分数据处于等待状态，如果传输数据的速度慢于 cpu 处理的速度，那么 cpu 就要等待，只有足够多的数据构成有意义的能解读的整体，cpu 才能继续处理。

3. 理想情况下，我们希望数据传输的速度和 cpu 的处理速度一致，传来多少数据，cpu 就能处理多少数据。但实际上，不可能达到理想状况，数据传输的速度有可能比 cpu 处理的快，也有可能慢，无论哪种情况，我们都需要在内存中开辟一个固定大小的区域，用来存放这些暂时用不到的数据，如果数据传输速度比 cpu 快，这个区域就存着提前到达的数据；如果数据传输速度比 cpu 慢，这个区域就存够一批足够能用的数据再给 cpu，因此，这个区域就被称为缓冲区，即 Buffer。

### 3. Buffer 与 Stream

1. 数据据流（stream of data）是从一个源头（source）向目的地（destination）传输数据的过程。

2. 创建一个可读流以后，从源头源源不断地读取数据，然后将数据放到一个内存区域中，当这个内存区域满了以后，就暂停读取数据，等待消费者消费，当消费者将区域中的数据消费完以后，读取数据的过程恢复，继续向内存中添加数据。

3. 上面提到的暂时存放数据的内存区域，就是缓冲区 —— Buffer。Buffer 作为可读流或者可写流的暂存数据的区域，肩负着缓存数据的责任。没有消费者消费数据，数据就从源头放到 Buffer 中暂存。有消费者消费如果消费者消费数据比较快，或者从数据传送地慢，导致 Buffer 没有被填满，那么消费者就得等待。 

4. 一个有关 Buffer 和 Stream 的典型例子就是我们在线看视频的时候。视频数据是以流的方式从服务器传送到客户端的，即视频是一块一块的传送过来的。在客户端有一个 Buffer 来接收视频流数据，当这个 Buffer 满了以后，客户端才会消费这些视频数据，也就是播放视频。如果我们的网络足够快，数据流（Stream）就可以足够快，可以让 Buffer 迅速填满然后发送和处理，然后处理下一个，再发送，再处理另一个，再发送，然后整个视频流就传输完毕，我们观看视频就可以一路到底。

5. 但是当我们的网络连接很慢，当处理完当前的数据后，我们的播放器就会暂停，或出现缓冲/加载等类似的字符，这表示客户端正在收集更多的数据，或者等待更多的数据到来，才能下一步处理。当 Buffer 装满并处理好，客户端就会显示数据，也就是播放视频了。在播放当前内容的时候，更多的数据也会源源不断的传输、到达并存在 Buffer 中。如果播放器已经处理完或播放完前一个数据，Buffer 仍然没有填满，缓冲/加载等类似的字符就会再次出现，等待和收集更多的数据。


### 4. Buffer 与 ArrayBuffer 

#### 1. 二进制数组

1. 二进制数组是处理二进制数据的类。

2. 在 ES6 之前，JavaScript 不具备直接操作二进制数据的能力，在 ES6 中，我们引入了二进制数组，而引入的原因是：
   > 这个接口的原始设计目的，与 WebGL 项目有关。所谓 WebGL，就是指浏览器与显卡之间的通信接口，为了满足 JavaScript 与显卡之间大量的、实时的数据交换，它们之间的数据通信必须是二进制的，而不能是传统的文本格式。文本格式传递一个 32 位整数，两端的 JavaScript 脚本与显卡都要进行格式转化，将非常耗时。这时要是存在一种机制，可以像 C 语言那样，直接操作字节，将 4 个字节的 32 位整数，以二进制形式原封不动地送入显卡，脚本的性能就会大幅提升。
    > 
    > 二进制数组就是在这种背景下诞生的。它很像 C 语言的数组，允许开发者以数组下标的形式，直接操作内存，大大增强了 JavaScript 处理二进制数据的能力，使得开发者有可能通过 JavaScript 与操作系统的原生接口进行二进制通信。
   > 
   > 以上内容来自《ECMAScript 6 标准入门》

3. 引入了二进制数组，使得 JavaScript 可以直接操作二进制数据，这样使得 JavaScript 能直接处理音视频、图像等二进制数据，同时处理效率也会大大提高。

4. 二进制数组包含三个内容：
   - ArrayBuffer 对象
   - TypedArray 视图
   - DataView 视图

#### 2. ArrayBuffer

1. ArrayBuffer 是最基础的二进制对象，是对固定长度的连续内存空间的引用，表示一段二进制数据。

2. ArrayBuffer 名字中虽然有 Array，但本质上和数组并没有什么关系。它申请了一段内存区域，但是里面放的是啥，我们并不知道，它只是存了一堆字节。我们无法直接操作它。如果如果我们想操作这段内存空间怎么办呢？这时候就需要一个称之为视图（TypedArray 或者是 DataView）的东西来进行操作。

3. ArrayBuffer 也是一个构造函数，可以分配一段可以存放数据的连续内存。
   ```js
      const buffer = new ArrayBuffer(12); // 生成一个可以12个字节的连续内存，每个字节的默认值是0
   ```
4. 以 DataView 视图方式读取。
   ```js
      const buffer = new ArrayBuffer(12);
      const dataView = new DataView(buffer); // 对一段12字节的内存建立DataView视图
      dataView.getUint8(0); // 0 以Uint8方式读取第一个字节
   ```
5. 以 TypedArray 视图方式读取，和 DataView 不同的是 DataView 是一个构造函数，TypedArray 则是一组构造函数。
   ```js
      const buffer = new ArrayBuffer(12);
      const x1 = new Uint8Array(buffer); // 建立Uint8Array视图
      const x2 = new Int32Array(buffer); // 建立Int32Array视图
      x1[0] = 1;
      x2[0] = 2;
      // 由于两个视图是对应的是同一段内存，所以其中一个视图更改了内存，会影响到另一个视图
      x1[0]; // 2
   ```
6. ArrayBuffer 对象还拥有 byteLength 属性和 slice 方法。slice 方法是 ArrayBuffer 对象上唯一可以读写内存的方法。

7. ArrayBuffer 有一个静态方法 isView，判断参数是否为视图实例。
   ```js
      const buffer = new ArrayBuffer(12);
      buffer.byteLength; // 12
      buffer.slice(0, 3); // 用法和数组一致，拷贝buffer的前三个字节生成一个新的ArrayBuffer对象
      ArrayBuffer.isView(buffer); // false
      const dataView = new DataView(buffer);
      ArrayBuffer.isView(dataView); // true
   ```

#### 3. TypedArray 

1. 首先解释什么是视图：ArrayBuffer 申请了一段内存区域，里面放的是啥，我们并不知道，只是存储了一堆二进制数字。对这些二进制数字我们有不同的解读方式，对二进制数字的解读方式就叫视图。

2. 视图的作用：以指定格式解读二进制数据。

3. TypedArray 是操作 ArrayBuffer 的视图的一种。TypedArray 字面意思是类型数组，就是指定了数据的类型，然后进行操作。

4. TypedArray 视图主要用来读写简单类型的二进制数据。

5. TypedArray 视图支持的数据类型一共有 9 种，每个类型同时也是构造函数，如下表所示：
   
  数据类型|字节长度|含义|对应 C 语言类型
  :---:|:---:|:---:|:---:|
   Int8Array|1|8位有符号整数|signed char
   Uint8Array|1|8位无符号整数|unsigned char
   Uint8ClampedArray|1|8位无符号整型固定数组（数值在0~255之间）|signed char|unsigned char
   Int16Array|2|16位有符号整数|short
   Uint16Array|2|16位无符号整数|unsigned short
   Int32Array|4|32 位有符号整数|int
   Uint32Array|4|32 位无符号整数|unsigned int
   Float32Array|4|32 位 IEEE 浮点数|float
   Float64Array|8|64 位 IEEE 浮点数|double

7. 以 Uint8Array 为例，这个视图是怎样解释 ArrayBuffer 中的二进制数据呢？Uint8Array 将 ArrayBuffer 中的每个字节视为一个单位。每个单位是 0 到 255 之间的数字。之所以是 255，是因为每个单位最多是 8 位，即 2^8 次方。为了表示方便，使用 16 进制形式表示一个字节，即 00 - ff 之间的数字表示一个字节。

8. Uint16Array 将 ArrayBuffer 中每 2 个字节视为一个单位。每个单位是 0 到 65535 之间的整数。原理同上。使用 16 进制形式表示就是 0000 - ffff。

9. Uint32Array 将 ArrayBuffer 中每 4 个字节视为一个单位。每个单位是 0 到 4294967295 之间的整数。原理同上。使用 16 进制形式表示就是 00000000 - fffffffff。

10. TypedArray 视图的构造函数接收的参数：
    - TypedArray 视图的构造函数接收三个参数，第一个 ArrayBuffer 对象，第二个是视图开始的字节号（默认0），第三个是视图结束的字节号（默认直到本段内存区域结束）
    - 接收 ArrayBuffer 实例作为参数，以指定格式读出二进制数据
    - 可接受普通数组作为参数，直接分配内存生成 ArrayBuffer 实例，并同时对这段内存进行赋值，再根据这段内存生成视图。
    - 可接受视图做为参数，生成的新数组复制了参数视图的值，生成新的数组和视图。（想基于同一内存生成新视图，需要传入视图.buffer）

11. TypedArray 视图和数组的区别
    - TypedArray 内的成员只能是同一类型。
    - TypedArray 成员是连续的，不会有空位。
    - TypedArray 成员的默认值为0，数组的默认值为空。
    - TypedArray 只是视图，本身不存储数据，数据都存储在底层的ArrayBuffer中，要获取底层对象必须使用 Buffer对象的属性。
    - TypedArray 可以直接操作内存，不需要进行类型转换，所以比数组快。
    - 数组有的方法 TypedArray 都可以使用，但不能使用 cancat 方法，因为 TypedArray 操作的数据长度固定。

    
#### 4. DataView - 视图

1. 如果一段数据包括多种类型（比如服务器传来的 HTTP 数据），这时除了建立 ArrayBuffer 对象的复合视图以外，还可以通过 DataView 视图进行操作。

2. DataView 视图提供更多操作选项，而且支持设定字节序。本来，在设计目的上，ArrayBuffer 对象的各种 TypedArray 视图，是用来向网卡、声卡之类的本机设备传送数据，所以使用本机的字节序就可以了；而 DataView 视图的设计目的，是用来处理网络设备传来的数据，所以大端字节序或小端字节序是可以自行设定的。

3. DataView 视图本身也是构造函数，接受一个 ArrayBuffer 对象作为参数，生成视图。
   ```js
      new DataView(ArrayBuffer buffer [, 字节起始位置 [, 长度]]);
   ```

4. 下面是使用 DataView 的一个例子：
   ```js
      const buffer = new ArrayBuffer(24);
      const dv = new DataView(buffer);
   ```
5. DataView 实例有以下属性，含义与 TypedArray 实例的同名方法相同。
   - `DataView.prototype.buffer`：返回对应的 ArrayBuffer 对象
   - `DataView.prototype.byteLength`：返回占据的内存字节长度
   - `DataView.prototype.byteOffset`：返回当前视图从对应的 ArrayBuffer 对象的哪个字节开始

6. DataView 实例提供 8 个方法读取内存。
   - `getInt8`：读取 1 个字节，返回一个 8 位整数。
   - `getUint8`：读取 1 个字节，返回一个无符号的 8 位整数。
   - `getInt16`：读取 2 个字节，返回一个 16 位整数。
   - `getUint16`：读取 2 个字节，返回一个无符号的 16 位整数。
   - `getInt32`：读取 4 个字节，返回一个 32 位整数。
   - `getUint32`：读取 4 个字节，返回一个无符号的 32 位整数。
   - `getFloat32`：读取 4 个字节，返回一个 32 位浮点数。
   - `getFloat64`：读取 8 个字节，返回一个 64 位浮点数。

#### 5. Buffer 与 ArrayBuffer

1. 现在我们就能理清 Buffer 和 ArrayBuffer 的区别了。

2. ArrayBuffer 表示内存之中的一段二进制数据，在 ES6 中被引入，使得 JavaScript 具备直接操作二进制数据的能力。

3. ArrayBuffer 对象表示对存储一段二进制数据的内存的引用。我们不能直接对这段内存进行读写。只能通过视图（TypedArray 或者 DataView）来读写。

4. 视图的作用是以指定格式解读二进制数据。分为 TypeArray 视图和 DataView 视图。

5. Buffer 是以更优化和更适合 Node.js 的方式实现了 Uint8Array 类中的 API。前面也说过：
   > Buffer 类是 JavaScript 中的 Uint8Array 类的子类，并使用覆盖其他用例的方法对其进行扩展，Node.js 的 API 支持纯的 Uint8Array，而 Buffer 也支持。

6. 总结来说，Buffer 类是视图 TypedArray 中的 Uint8Array 类的nodejs具体实现，并对 Uint8Array 类中的方法进行了扩展。对于 ArrayBuffer 表示的二进制数据，统一以无符号8位整数即一个字节为一个单位，进行解读。使用 0 - 255 之间的整数表示一个字节，实际上使用十六进制 00 - ff 表示一个字节。


### 4. Buffer 中的编码格式

1. Buffer 中存储的二进制数据，也就一个字节（byte）一个字节的数据。当 Buffer 与字符串相互转换的时候，必须指定编码格式，如果没有指定编码格式，默认的编码格式是 utf-8。



#### 1. Node 支持的字符集编码

1. 使用 `uft8`、`utf16le`、`latin1` 这几种编码格式，将 Buffer 转换为字符串被认为是解码，而将字符串转换为 Buffer 被认为是编码。

##### 1. utf8

1. utf8 编码格式使用多个字节编码 unicode 字符。

2. 许多网页和其他文档的编码格式就是 utf-8。

3. 这是 Buffer 的默认编码方式。

4. 将 Buffer 中的数据转换为字符串的时候，如果 Buffer 中的数据不完全是有效 UTF-8 数据时，Node 将使用 Unicode 替换字符 `U+FFFD�` 表示这些错误。

##### 2. utf16le

1. utf16 编码格式使用多个字节编码 unicode 字符。

2. 不像 utf8，utf16 使用 2 或者 4 个字节表示一个字符。

3. Node 只支持 UTF-16 编码的小端字节序的实现形式。

##### 3. latin1

1. latin1 是 ISO-8859-1 编码国际标准的别名。

2. latin1 支持的 Unicode 编码的范围是 U+0000 到 U+00FF。

3. latin1 对每个字符使用一个字节进行编码。不适合该范围的字符将被截断，并将映射到该范围内的字符。

4. latin1 向下兼容ASCII，其编码范围是 0x00-0xFF，0x00 - 0x7F 之间完全和 ASCII 一致，0x80-0x9F 之间是控制字符，0xA0 - 0xFF 之间是文字符号。

5. 此字符集支持部分于欧洲使用的语言，包括阿尔巴尼亚语、巴斯克语、布列塔尼语、加泰罗尼亚语、丹麦语、荷兰语、法罗语、弗里西语、加利西亚语、德语、格陵兰语、冰岛语、爱尔兰盖尔语、意大利语、拉丁语、卢森堡语、挪威语、葡萄牙语、里托罗曼斯语、苏格兰盖尔语、西班牙语及瑞典语。

#### 2. Node 支持的二进制转文本的编码

1. 对于二进制转文本编码，将 Buffer 转换为字符串通常被认为是编码，将字符串转换为 Buffer 被认为是解码。

##### 1. base64

1. base64 是一种使用 64 个可打印字符来表示二进制数据的编码方式。这样使得计算机上的二进制内容可以字符来表示，方便二进制内容在网络上的传输。
2. 64 个可打印的字符包括：`a-z`、`A-Z`、`0-9`、`+/`，在加上一个补位字符：`=`，一共 65 个字符。

3. 将一个字符串转换为 Buffer 的时候，如果指定编码格式为 base64，那么对于 [RFC 4648 Section 5](https://datatracker.ietf.org/doc/html/rfc4648#section-5) 标准中指定的安全的 URL 和文件名字母表，也能正确被处理。如果base64编码的字符串中包含空白字符，如空格（spaces）、制表符（tabs）、换行符，在转换为 Buffer 的过程中会被忽略。

##### 2. base64url

1. base64url 指的是能正确表示为 url 字符串的 base64 编码的一种。因为传统的 base64 编码存在 `+`、`/` 和 `=`，这三个字符在 url 中会造成歧义，因此传统的 base64 编码的字符串不适合作为 url 传输。

2. 为了解决这个问题，人们提出了 base64url 编码。编码方式与 base64 相同，但是将 + 替换为 -，/ 替换为 _，这样就可解决编码后的字符串不能在 url 中传输的问题了。

3. 由于 `=` 字符也可能出现在 Base64 编码中，但 `=` 用在URL、Cookie 里面会造成歧义，所以，很多 Base64 编码后会把 = 去掉：
   ```
      # 标准Base64:
      'abcd' -> 'YWJjZA=='
      # 自动去掉=:
      'abcd' -> 'YWJjZA'
   ```
4. 去掉 = 后怎么解码呢？因为 Base64 是把 3 个字节变为 4 个字节，所以，Base64 编码的长度永远是 4 的倍数，因此，需要加上 = 把 Base64 字符串的长度变为 4 的倍数，就可以正常解码了。

5. 在 [RFC 4648 Section 5](https://datatracker.ietf.org/doc/html/rfc4648#section-5) 标准中指定了 base64url 编码。

6. 将字符串转换为 Buffer 的时候，base64url 也可以正确的处理常规的 base64 编码的字符串。

7. 将 Buffer 转换为字符串的时候，base64url 这种编码方式将会省略补码 `=`。

##### 3. hex

1. hex 编码方式将每个字节编码为两个 16 进制字符。即将一个字节使用 00 - ff 之间的数字表示。

2. 当需要解码的字符串仅包含有效十六进制字符时，可能会发生数据截断。什么意思呢，就是 hex 这种编码方式，只对有效的的 十六进制字符进行解码，超出的字符就会被忽略，不进行解码。举例如下：
   ```js
      Buffer.from('1ag', 'hex');
      // Prints <Buffer 1a>, data truncated when first non-hexadecimal value
      // ('g') encountered.

      Buffer.from('1a7g', 'hex');
      // Prints <Buffer 1a>, data truncated when data ends in single digit ('7').

      Buffer.from('1634', 'hex');
      // Prints <Buffer 16 34>, all data represented.
   ```

#### 3. Node 支持的遗留的编码

##### 1. ascii

1. ascii 值支持 7比特的 ASCII 编码格式的字符。

2. 将字符串转换为 Buffer，指定 ascii 编码的方式等同于使用 latin1。

3. 将 Buffer 转换为字符串，使用 ascii 编码方式，等同于 latin1 的解码

4. 一般来说，不应该继续使用 ascii 这种编码方式，除非我们明确得知数据总是 ASCII 编码。对于编码解码只由 ascii 字符组成文本而言，utf8 是更好的方式。

5. 目前保留 ascii 这种编码方式仅仅因为是保证兼容性。


##### 2. binary

1. binary 是 latin1 编码方式的别名。

2. binary 这种编码的名称可能会产生误导，因为这里列出的所有编码格式都是用于字符串和二进制数据之间的转换。对于字符串和 Buffer 之间的转换，通常 utf-8 是正确的选择。

3. 有关 binary 这种编码其他内容请看： [binary strings - MDN](https://developer.mozilla.org/en-US/docs/Web/API/DOMString/Binary)

##### 2. ucs2

1. ucs2 是 utf16le 编码格式的别名。

2. UCS-2 代表 UTF-16 的一种变体，该变体不支持代码点大于 U+FFFF 的字符。在Node.js支持编码大于 U+FFFF 的字符。

## 3. Buffer 常用的 API

1. Node 在全局环境下提供 Buffer 这个类，因此我们无需引入 Buffer。

2. Node 不推荐使用 `new Buffer()` 的方式初始化一个 Buffer 实例。推荐使用静态方法创建 Buffer 实例。


### 1. Buffer.alloc(size[, fill[, encoding]])

1. 创建一个指定长度 Buffer 实例。这个长度指定的多少字节。

2. 调用Buffer.alloc() 可能比 Buffer.allocUnsafe() 慢得多，但可以确保新创建的 Buffer 实例内容永远不会包含以前分配的敏感数据，包括可能没有分配给缓冲区的数据。

3. fill 参数没有指定，默认用 0 去填充。

4. 参数说明：
   - size：buffer 的长度，可以理解为开辟的内存区域的大小。
   - fill：用来填充的数据。可以是字符串、Buffer、Unit8Array 或者是 integer。默认是 0。
   - encoding：编码方式，默认是 utf-8。

5. 只指定 size，使用 0 进行填充：
   ```js
      // 创建指定大小的 buffer
      const buf1 = Buffer.alloc(5);
      // <Buffer 00 00 00 00 00>
      console.log(buf1);
   ```
6. 指定 fill 参数，那么将使用 fill 指定的内容填充 Buffer：
   ```js
      const buf1 = Buffer.alloc(5, 'a');
      const buf2 = Buffer.alloc(5, 'ab');
      const buf3 = Buffer.alloc(6, 1);
      const buf4 = Buffer.alloc(10, 100);
      const buf5 = Buffer.alloc(10, 300);
      // <Buffer 61 61 61 61 61>
      console.log(buf1);
      // <Buffer 61 62 61 62 61>
      console.log(buf2);
      // <Buffer 01 01 01 01 01 01>
      console.log(buf3);
      // <Buffer 64 64 64 64 64 64 64 64 64 64>
      console.log(buf4);
      // <Buffer 2c 2c 2c 2c 2c 2c 2c 2c 2c 2c>
      console.log(buf5);
   ```
   1. fill 参数为字符串，那么会将这个字符按照 utf-8 编码的方式进行编码，然后将编码后的结果一个字节一个字节的放入 Buffer 中，如果 Buffer 满了，那么后续的内容就不再放入。如果 Buffer 没有满，而字符串已经填充完了，那么继续从字符串的头部开始，将编码后的字符放到 Buffer 中，直到 Buffer 满了。
   2. fill 参数为整数，那么将整数按照 16 进制转换，将转换后的结果存入 Buffer 的每个字节上。如果整数大于 255，那么会将其转换后，只保留后面的两位。例如最后的那个例子，300 转换成 16 进制是 12c，保留最后两位就是 2c，所以 Buffer 中的每个字节就是 2c。

7. 指定 fill 和 encoding，那么会按照指定的 encoding 方式对 fill 进行解码，然后填充到 Buffer 中：
   ```js
      const buf = Buffer.alloc(11, 'aGVsbG8gd29ybGQ=', 'base64');

      // <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
      console.log(buf);
   ```
   fill 的内容是 `aGVsbG8gd29ybGQ=`，我们指定 `fill` 的编码方式是 `base64`，那么首先对 fill 的内容进行 base64 解码，解码后的结果是 `hello world`，然后将 `hello world` 以 utf-8 编码的方式存入 Buffer 中。

### 2. Buffer.allocUnsafe(size)

1. 创建一个指定大小的 Buffer 实例。

2. 通过 Buffer.allocUnsafe(size) 这种方式创建 Buffer 实例，其被分配的内存区域不会被初始化。新创建的 Buffer 实例里面的内容是未知的，有可能包含敏感数据。

3. 推荐使用 Buffer.alloc() 创建 Buffer 实例。

4. 基本示例：
   ```js
      const buf = Buffer.allocUnsafe(10);

       // <Buffer 98 b3 42 19 1d 02 00 00 01 00>
       console.log(buf);

       buf.fill(0);
       // <Buffer 00 00 00 00 00 00 00 00 00 00>
       console.log(buf);
   ```

### 3. Buffer.from(array)

1. 将一个由整数（字节）组成的数组转换成 Buffer 实例。

2. 整数范围是 0- 255，超出这个范围会被截断。

3. 用法示例：
   ```js
      const arr = [1, 10, 100, 200, 300];

      const buf = Buffer.from(arr);

      // <Buffer 01 0a 64 c8 2c>
      console.log(buf);
   ```
   300 超出 255，转换为 16 进制是 12c，被截断后就是 2c。

### 4. Buffer.from(arrayBuffer[, byteOffset[, length]])

1. 创建一个 ArrayBuffer 的视图，而无需复制 arrayBuffer 的内容到一块新的内存中。例如，当传递对 TypedArray 实例的 `.buffer` 属性的引用时，新创建的 Buffer 实例将与 TypedArray 的底层 ArrayBuffer 共享相同的分配内存。

2. 参数：
   - arrayBuffer：ArrayBuffer 的实例（二进制数组）
   - byteOffset：整数，从 arrayBuffer 中的哪个字节开始访问，默认是 0。
   - length：访问的字节的数量，默认是 arrayBuffer.byteLength - byteOffset

### 5. Buffer.from(buffer)

1. 将传入的 Buffer 实例复制一份到新的 Buffer 实例上。

2. 参数：buffer 可以是 Buffer 的实例或者是 Unit8Array 的实例。

3. 用法示例：
   ```js
      const arr = [1, 10, 100, 200, 300];

      const buf = Buffer.from(arr);

      // <Buffer 01 0a 64 c8 2c>
      console.log(buf);

      const buf2 = Buffer.from(buf);
      // <Buffer 01 0a 64 c8 2c>
      console.log(buf2);
   ```
### 6. Buffer.from(object[, offsetOrEncoding[, length]])

1. 将一个对象转换为 Buffer 实例。这个对象的 valueOf() 方法的返回值不是严格等于这个对象。

2. 这个方法的返回值是：Buffer.from(object.valueOf(), offsetOrEncoding, length)

3. 对象的 valueOf() 一般情况下返回一个字符串，用来表示这个对象。

4. 参数：
   - object：支持 Symbol.toPrimitive 或者 valueOf() 的对象
   - offsetOrEncoding：整数或者字符串，整数表示从哪个字节开始进行转换，字符串表示编码。
   - length：转换的长度。

### 7. Buffer.from(string[, encoding])

1. 将一个字符串转换成 Buffer 实例。encoding 参数的作用是指定字符串的编码方式。按照 encoding 指定的编码方式对字符串进行解码，将其转换成字节，然后存入 Buffer 中。
2. 参数：
   - 待转换的字符串
   - 编码格式，默认是 utf8

3. 用法示例：
   ```js
      const buf = Buffer.from('hello world');
      // <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
      console.log(buf);
   ```
4. 用法示例 - 指定编码：
   ```js
      const buf2 = Buffer.from('Welcome to China', 'latin1');
      const buf3 = Buffer.from('V2VsY29tZSB0byBDaGluYQ==', 'base64');
      const buf4 = Buffer.from('aa1234', 'hex');

      // <Buffer 57 65 6c 63 6f 6d 65 20 74 6f 20 43 68 69 6e 61>
      console.log(buf2);
      // <Buffer 57 65 6c 63 6f 6d 65 20 74 6f 20 43 68 69 6e 61>
      console.log(buf3);
      // <Buffer aa 12 34>
      console.log(buf4);
   ```
### 8. Buffer.concat(list[, totalLength])

1. 对 list 中的 buffer 实例进行连接，生成一个新的 Buffer 实例。类似数组的 concat 方法。

2. 参数：
   - list：由 Buffer 实例或者 Uint8Array 实例组成的数组
   - totalLength：list 中所有的 Buffer 实例的长度总和

3. 如果 list 为空，或者 totalLength 为 0，返回一个长度为 0 的 Buffer 实例。

4. 没有指定 totalLength，它会将 list 中所有的 Buffer 实例的长度的总和作为 totalLength 的值。

5. 如果提供了 totalLength，则将其强制转换为无符号整数。如果列表中Buffer 实例的总长度超过 totalLength，则结果的 Buffer 实例的总长度依旧是 totalLength。超出 totalLength 的内容就被截断了。

6. 用法示例：
   ```js
      const buf = Buffer.from('hello ');
      const buf2 = Buffer.from('world ');
      const buf3 = Buffer.from('!');

      const buf4 = Buffer.concat([buf, buf2, buf3]);

      // <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64 20 21>
      console.log(buf4);

      // hello world !
      console.log(buf4.toString());
   ```
7. 用法示例 - 指定 totalLength：
   ```js
      const buf = Buffer.from('hello ');
      const buf2 = Buffer.from('world ');
      const buf3 = Buffer.from('!');

      const buf5 = Buffer.concat([buf, buf2, buf3], 5);


      // <Buffer 68 65 6c 6c 6f>
      console.log(buf5);
      
      // hello
      console.log(buf5.toString());
   ```
   指定了 totalLength，那么将 list 中的 buffer 拼接后，只截取 前 totalLength 个字节作为新的 Buffer 的实例的内容。

### 9. buf.toString([encoding[, start[, end]]])

1. 根据 encoding 指定的编码方式，将一个 Buffer 实例为字符串。start 和 end 指定 Buffer 实例的解码的开始和结束位置。

2. 如果 encoding 是 utf8，而 输入并不是有效的 utf-8 编码的字符序列，每一个无效的字节会被替代字符 U+FFFD 替代。

3. 参数：
   - encoding：编码方式，默认是 utf8。
   - start：Buffer 实例的解码的起始位置，默认是 0。
   - end：Buffer 实例的解码的结束位置（不包括这个位置），默认是 buf.length。

4. 用法示例：
   ```js
      const buf = Buffer.from('hello world');

      const str1 = buf.toString();
      // hello world
      console.log(str1);
   ```
   不指定编码，默认是 utf-8。

5. 用法示例 - 指定编码：
   ```js
      const buf2 = Buffer.from('V2VsY29tZSB0byBDaGluYQ==', 'base64');

      // <Buffer 57 65 6c 63 6f 6d 65 20 74 6f 20 43 68 69 6e 61>
      console.log(buf2);

      // V2VsY29tZSB0byBDaGluYQ==
      console.log(buf2.toString('base64'));
      // Welcome to China
      console.log(buf2.toString('utf8'));
      // Welcome to China
      console.log(buf2.toString('latin1'));
      
      const buf3 = Buffer.from('明天你好');

      // <Buffer e6 98 8e e5 a4 a9 e4 bd a0 e5 a5 bd>
      console.log(buf3);

      // 明天你好
      console.log(buf3.toString('utf8'));
      // æ  å¤©ä½ å¥½
      console.log(buf3.toString('latin1'));
      // 5piO5aSp5L2g5aW9
      console.log(buf3.toString('base64'));
   ```
   指定了不同的编码格式，即使是同样的字节，那么也会被编码为不同的结果。因此，为了保证输入与输出的一致性，我们最好指定同一种编码方式。
6. 用法示例 - 指定位置：
   ```js
      const buf = Buffer.from('hello world');

      const str1 = buf.toString('utf8', 0, 5);
      // hello
      console.log(str1);
      // world
      console.log(buf.toString('utf8', 6));
   ```
### 10. buf.toJSON()

1. 实例方法，返回 Buffer 实例的 JSON 表示形式。当 使用 JSON.stringify() 转换一个 Buffer 实例的时候，会隐式调用这个函数。

2. Buffer.from 方法接收由 buf.toJSON() 返回的这种格式的对象。特别地，Buffer.from(buf.toJSON()) 类似于 Buffer.from(buf)。

3. 用法示例：
   ```js
      const buf = Buffer.from('hello world');
      const buf2 = Buffer.from([1, 2, 3]);
      const json1 = buf.toJSON();
      // {
      //   type: 'Buffer',
      //   data: [
      //     104, 101, 108, 108,
      //     111,  32, 119, 111,
      //     114, 108, 100
      //   ]
      // }
      console.log(json1);
      // { type: 'Buffer', data: [ 1, 2, 3 ] }
      console.log(buf2.toJSON());
      
   ```
4. 用法示例 - Buffer.from 方法转换 Buffer.toJSON 格式的对象：
   ```js
      const buf = Buffer.from('hello world');
      const json1 = buf.toJSON();
      // <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
      console.log(Buffer.from(json1));
   ```
### 11. buf.slice([start[, end]])

1. 实例方法，返回一个新的 Buffer 实例，和原来的 Buffer 实例共享一个内存，但是起始位置和结束位置由 start 和 end 指定。

2. 类似于数组的 slice 方法，从指定的位置对 Buffer 实例进行切分。但是并不会创建一个新的内存区域来存储数据，新的 Buffer 实例与原来的 Buffer 实例共享一个内存。

3. 参数：
   - start：整数，新的 Buffer 实例的起始位置，默认是 0。
   - start：整数，新的 Buffer 实例的结束位置（不包括这个位置），默认是 buf.length。

4. 用法示例：
   ```js
      const buf2 = Buffer.from([0, 1, 2, 3, 5, 6, 7, 8, 9, 10]);

      // <Buffer 00 01 02 03 05 06 07 08 09 0a>
      console.log(buf2.slice());
      // <Buffer 00 01 02 03 05>
      console.log(buf2.slice(0, 5));
      // <Buffer 05 06 07 08 09 0a>
      console.log(buf2.slice(4));
   ```  



### 12 buf.values()

1. 实例方法。该方法创建并返回由 Buffer 实例中的字节的数值组成的 Iterator 对象。

2. 使用 for..of 遍历一个 Buffer 实例的时候，会自动调用这个方法。

3. 用法示例：
   ```js
      const buf = Buffer.from('buffer');

      for (const value of buf.values()) {
          console.log(value);
      }
      // Prints:
      //   98
      //   117
      //   102
      //   102
      //   101
      //   114

      for (const value of buf) {
          console.log(value);
      }
      // Prints:
      //   98
      //   117
      //   102
      //   102
      //   101
      //   114
   ```
   
### 13. buf.write(string[, offset[, length]][, encoding])

1. 这个方法用于向一个 Buffer 实例中写入一个字符串。写入的字符串在 Buffer 实例中的起始位置由 offset 决定，字符串的编码格式由 encoding 指定。length 参数指定了写入的字符的数量。即不一定会把所有的字符串写入 Buffer 中。

2. 如果 buffer 的实例中没有足够的空间容纳整个字符串，则只会写入部分字符串。但是将不会写入部分编码的字符。

3. write 方法的返回值是写入字符的数量。


4. 参数：
   - string：写入 Buffer 实例的字符串
   - offset：在写入字符串前跳过的字节数量。即写入字符串在 Buffer 实例中的起始位置，默认是 0。
   - length：写入的最大的字符数量。写入数量不会超过 buf.length - offset。默认是：buf.length - offset
   - encoding：写入的字符串的编码方式，默认是 utf8。
   
5. 用法示例：
   ```js
      const buf = Buffer.alloc(256);

      const len = buf.write('\u00bd + \u00bc = \u00be', 0);

      // 12 bytes: ½ + ¼ = ¾
      console.log(`${len} bytes: ${buf.toString('utf8', 0, len)}`);

      const buffer = Buffer.alloc(10);

   
      // 指定起始位置
      const length = buffer.write('abcd', 8);
      // 2 bytes: ab
      //
      console.log(`${length} bytes: ${buffer.toString('utf8', 8, 10)}`);

      const buffer2 = Buffer.alloc(10);
      // 指定起始位置，长度
      buffer2.write('hello world', 0, 10);

      // <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c>
      console.log(buffer2);

      // hello worl
      console.log(buffer2.toString());

      const buffer3 = Buffer.alloc(16);
      // 指定起始位置，长度，编码
      buffer3.write('aGVsbG8gd29ybGQ=', 0, 16, 'base64');
      // <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64 00 00 00 00 00>
      console.log(buffer3);
      // hello world
      console.log(buffer3.toString());
   ```

### 14. buf.copy(target[, targetStart[, sourceStart[, sourceEnd]]])

1. 从 buf 的某个区域中中复制数据到 target 的一个区域中，即使 target 的内存区域和 buf 的区域有重叠，复制过程也会继续。

2. 参数：
   - target： 一个 Buffer 或者 Uint8Array 的实例，复制的目标实例
   - targetStart：从目标实例哪个字节位置开始接收复制的实例。默认是 0。
   - sourceStart：从 buf 的哪个字节开始进行复制。默认是 0。
   - sourceEnd：buf 的复制字节的结束位置（不包括这个位置）。默认是：buf.length。

3. buf.copy 方法的返回值是复制的字节数。

4. 用法示例：
   ```js
      // Create two `Buffer` instances.
      const buf1 = Buffer.alloc(26);
      const buf2 = Buffer.alloc(26).fill('!');

      for (let i = 0; i < 26; i++) {
      // 97 is the decimal ASCII value for 'a'.
          buf1[i] = i + 97;
      }

       // Copy `buf1` bytes 16 through 19 into `buf2` starting at byte 8 of `buf2`.
       // 将 buf1 的 16 到 19 （不包括19）位置的字节数据（四个字节）复制到 buf2 中，buf2 接收复制字节的起始位置是从第 8 个字节开始
       buf1.copy(buf2, 8, 16, 20);



      // abcdefghijklmnopqrstuvwxyz
      console.log(buf1.toString());

      // <Buffer 21 21 21 21 21 21 21 21 71 72 73 74 21 21 21 21 21 21 21 21 21 21 21 21 21 21>
      console.log(buf2);

      // !!!!!!!!qrst!!!!!!!!!!!!!
      console.log(buf2.toString('ascii', 0, 25));
   ```
### 15. buf.fill(value[, offset[, end]][, encoding])

1. 实例方法。使用指定的之填充 Buffer 实例。如果没有指定 offset 和 end，会填满整个 Buffer 实例。

2. buf.fill 方法的返回值是对 buf 的一个引用。

3. 参数：
   - value：填充 Buffer 实例的数据，可以是字符串、Buffer、Uint8Array 或者是整数。
   - offset：从Buffer 实例的第几位开始填充，默认是0
   - end：停止填充的位置，默认是 buf.length
   - encoding：如果 value 是 String，那么为 value 的编码，默认是utf8。

4. 如果 value 不是字符串、Buffer 实例或整数，则将其强制为 uint32 值。如果转换后的结果整数大于255（十进制），使用 `value & 255` 填充 Buffer 实例。

5. 如果 fill() 接受的参数是一个多字节的字符，而 Buffer 实例中的空间又不足以放下整个字符，那么只能按顺序写入这个字符，直到 Buffer 实例的空间满了，未被写入的字节就放弃了。
   ```js
      // <Buffer e4 bd a0 e4 bd>
      console.log(Buffer.alloc(5).fill('你'));
   ```
   Buffer 实例一共有 5 个字节，我们使用 “你” 这个汉字填充 Buffer 实例。“你” 这个汉字占据三个字节，所以 Buffer 实例的前三个字节是被填充为 “你” 这个汉字的三个字节，之后剩下两个字节，放不下“你” 这个汉字完整的三个字节，只能放置其前两个字节。

6. 用法示例：
   ```js
      const base64Str = 'aGVsbG8gd29ybGQ=';

      const b = Buffer.allocUnsafe(10).fill('h');

      // 指定填充的开始位置、结束位置
      const b2 = Buffer.allocUnsafe(10).fill(15, 0, 5);

      // 指定编码
      const b3 = Buffer.allocUnsafe(11).fill(base64Str, 0, 11, 'base64');

      // hhhhhhhhhh
      console.log(b.toString());

      // <Buffer 0f 0f 0f 0f 0f 00 00 00 00 00>
      console.log(b2);

      // <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
      console.log(b3);

      // hello world
      console.log(b3.toString());
   ```


## 4. Buffer 与 Stream 的转换

1. 虽然fs模块的 createReadStream 方法的文档中说该模块可以接收 Buffer 对象作为参数，但实践中发现传入 buffer对象会报错：
   ```js
      // Error: ENOENT: no such file or directory, open 'hello world'
      // const readStream = fs.createReadStream(myBuf);

      // readStream.on('data', function (chunk) {
      //     console.log(chunk);
      // });
   ```
2. 经过查阅资料，发现 createReadStream 内部调用了 open() 函数，所以实际上只能接收文件路径，并不能使用 buffer 对象作为参数，因此我们必须借助其他方式，将 Buffer 实例转换成流。

3. 基本思路是使用原生 Stream 模块，创建可读流或者可写流，然后将 Buffer 实例写入流中。原生的 Stream 都支持将 Buffer 实例作为流中的数据。

4. **注意**：将 Buffer 实例转换成了流，但是并不会将 Buffer 实例自动切分成更小的块去读取，也就是 Buffer 多大，写入流中的数据就有多大，因此这种将 Buffer 实例转换成流的方式在某些情况下并不是最理想的解决方案，因为没有发挥出流的优势。

### 1. fs.createWriteStream

1. 使用 fs.createWriteStream 方法创建一个可写流，然后调用其 write 方法或者 end 方法写入 Buffer 实例。

2. 可读流的 write 方法或者 end 方法都支持接收 Buffer 实例或者 Uint8Array 实例作为参数，因此我们可以使用 write 或者 end 方法将 Buffer 实例写入可写流中。

3. 示例：
   ```js
      const myBuf = Buffer.from('hello world this is my house welcome to china java javascript php python ruby haskell swift kotlin', 'utf8');
      const writeStream = fs.createWriteStream(path.join(__dirname, './files/myBuffer.txt'), {
          encoding: 'utf8',
          flags: 'w',  // 文件操作方式是写入
          autoClose: true
      });
      // readStream.pipe(writeStream);

      writeStream.write(Buffer.from('hello ', 'utf8'));
      writeStream.write(Buffer.from('world !\n', 'utf8'));
      writeStream.write(myBuf);
      writeStream.end(Buffer.from('你好', 'utf8'));
   ```
   myBuffer.txt 文件中就会被写入 Buffer 实例中的内容，因为指定了编码格式为 UFT-8，所以 myBuffer.txt 中的内容就是转换前的字符串：
   ```
      hello world !
      hello world this is my house welcome to china java javascript php python ruby haskell swift kotlin你好
   ```

### 2. Transform

1. Transform 是转换流。可以在数据写入和读取时修改或转换数据的流。例如，在文件压缩操作中，可以向文件写入压缩数据，并从文件中读取解压数据。

2. Transform 同时实现了 Readable 和 Writable 的接口。因此既能使用可读流的 api，也能使用 可写流的 api。

3. 因此我们即可以向可写流中写入数据，也可以从可读流中消费数据。

4. 调用 Transform 的可写流的方法 —— write 或者 end 方法，将 Buffer 实例写入可写流中，然后调用我们在从可读流中消费数据，比如将可读流中的数据写入文件中。

5. 一般情况下，我们使用的是 PassThrough 这个类，PassThrough 类是转换流 Transform 的一种简易实现。

6. 示例代码 - 向可写流中写入数据：
   ```js
      const stream = require('stream');
      const transformStream = new stream.PassThrough();

      transformStream.write(Buffer.from('hello '));
      transformStream.write(Buffer.from('world '));
      transformStream.end(Buffer.from('!'));

      transformStream.on('data', function (chunk) {
          console.log('data');
          console.log(chunk);
      });
   
      // 输出：
      // data
      // <Buffer 68 65 6c 6c 6f 20>
      // data
      // <Buffer 77 6f 72 6c 64 20>
      // data
      // <Buffer 21>

   ```
7. 示例代码 - 消费可读流中数据：
   ```js
      const transformStream = new stream.PassThrough();

      transformStream.write(Buffer.from('hello '));
      transformStream.write(Buffer.from('world '));
      transformStream.end(Buffer.from('!'));
      
       // hello world !
      transformStream.pipe(process.stdout);
   ``` 
### 3. Duplex

1. Duplex 是双向流，是既可读又可写的流。

2. Duplex 同时实现了 Readable 和 Writable 的接口，因此既可以使用可读流的 api，又可以使用可写流的 api。

3. node 中，TCP 套接字、zlib、crypto 都是双向流。

4. 因此我们即可以向可写流中写入数据，也可以从可读流中消费数据。

5. 基本思路也是使用 Duplex 提供的可写流的方法，将 Buffer 实例写入可写流中，然后调用可读流的 api，对可读流中数据进行消费。

6. 示例代码 - 使用静态方法 `Duplex.from` 转换 Buffer 实例：
   ```js
      const duplexStream = stream.Duplex.from(Buffer. from('hello world !'));



      duplexStream.on('data', function (chunk) {
         console.log('data');
         console.log(chunk);
      });
      //
      duplexStream.on('end', function () {
          console.log('ended');
      });
      
     // data
     // <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64 20 21>
     // ended

   ```
   消费可读流数据：
   ```js
      const duplexStream = stream.Duplex.from(Buffer.from('hello world !'));

      // hello world !
      duplexStream.pipe(process.stdout);
   ```
7. 示例代码 - 使用 Duplex 示例：
   ```js
      const duplexStream = new stream.Duplex({
          read() {},
    
      });
  
      duplexStream.push(Buffer.from('hello world !'));
      duplexStream.push(Buffer.from('~~~~~'));
      // hello world ! ~~~~~
      duplexStream.pipe(process.stdout);
   ```
   push 方式是 Readable 的方法，用来向可读流队列中添加数据，可以接收 Buffer 实例。因此我们使用 push 方法，向可读流队列中添加 Buffer 实例，并最后消费可读流中的数据。

### 4. Readable

1. 子类继承 Readable 类，并在子类内部实现 _read() 方法，则我们就完成了一个自定义的可读流，可以使用可读流的方法实现对流的操作。

2. 示例代码：
   ```js
      /**
       * 继承 Readable 类实现可读流
       * 使用可读流的 push 方法将 Buffer 实例推入可读流中
       */

      class MyReadableStream extends stream.Readable {
          constructor() {
              super();
          }

          /**
           * 子类覆写 _read() 方法
           * @private
           */
          _read() {

          }
      }
   
      const readableStream = new MyReadableStream();
      //
      readableStream.push(Buffer.from('hello '));
      readableStream.push(Buffer.from('world '));
      readableStream.push(Buffer.from('!'));
      readableStream.pipe(writeStream);
   ```

### 5. Writable

1. 子类继承 Writable 类，并在子类内部实现 _write() 方法和 _final()。则我们就完成了一个自定义的可写流，可以使用可写流的方法实现对流的操作。

2. 所有可写流实现都必须提供一个 writable._write() 或writable._writev() 方法。当我们调用可读流的 write 方法的时候，就会调用内部的 _write() 或者 _writev() 方法。在子类覆写这个方法的主要目的是将数据收集起来，放到缓冲池中备用。

3. 子类中覆写 _final 方法。此可选 _final() 方法将在可写流关闭之前调用，在 _final 方法内部调用 callback 后会触发 finish 事件。这对于在流结束之前关闭资源或写入缓冲数据非常有用。

4. 一般来说，在 _final 方法内部，我们是将缓冲池内部的数据写入文件（destination）中。我们只有显式调用可写流的 end 方法，这样才会调用内部的 _final 方法。

5. 示例代码：
   ```js
      class MyWritableStream extends stream.Writable {
           constructor(path, encoding, options = {}) {
               super(options);
               this.path = path;
               this.chunk = [];
               this.encoding = encoding ? encoding : 'utf8';
           }

    

           /**
            * 覆写 _write() 方法
            * @param chunk
            * @param encoding
            * @param callback
            * @private
            */
           _write(chunk, encoding, callback) {
              this.chunk.push(chunk);
              if (encoding) {
                  this.encoding = encoding
              }

               callback();
           }

          /**
           * 子类中覆写 _final 方法
           * 此可选函数将在可写流关闭之前调用，在 _final 方法内部调用 callback 后，
           * 触发finish 事件
           * 这对于在流结束之前关闭资源或写入缓冲数据非常有用
           * @param callback
           * @private
           */
          _final(callback) {
              const chunk = Buffer.concat(this.chunk);

              // 将可读流数据写入文件中
              fs.writeFile(this.path, chunk, {encoding: this.encoding}, function (err) {
                  if (err) {
                    throw Error(err.message);
                  }
              });
              callback();
          }
      }

      const ws = new MyWritableStream('./files/myBuffer.txt', 'utf8');

      ws.write(Buffer.from('hello '));
      ws.write(Buffer.from('world '));
      ws.write(Buffer.from('你好 '));
      ws.write(Buffer.from('世界 '));

      ws.end('!');
   ```
