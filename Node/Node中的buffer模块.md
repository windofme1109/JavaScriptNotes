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

1. 首先解释什么是视图：ArrayBuffer 对象可以存储多种类型的数据。不同类型的数据有不同的解读方式，这就叫视图。

2. 视图的作用：以指定格式解读二进制数据。

3. TypedArray 是操作 ArrayBuffer 的视图的一种。TypedArray 字面意思是类型数组，就是指定了数据的类型，然后进行操作。

4. TypedArray 视图主要用来读写简单类型的二进制数据。

5. TypedArray 视图支持的数据类型一共有 9 种，每个类型同时也是构造函数，如下表所示：
   
  数据类型|字节长度|含义|对应 C 语言类型
  :---:|:---:|:---:|:---:|
   Int8Array|1|8位有符号整数
   Uint8Array|1|8位无符号整数
   Uint8ClampedArray|1|8位无符号整型固定数组(数值在0~255之间)
   Int16Array|2|16位有符号整数
   Uint16Array|2|16位无符号整数
   Int32Array|4|32 位有符号整数
   Uint32Array|4|32 位无符号整数
   Float32Array|4|32 位 IEEE 浮点数
   Float64Array|8|64 位 IEEE 浮点数

7. 以 Uint8Array 为例，这个视图是怎样解释 ArrayBuffer 中的二进制数据呢？Uint8Array 将 ArrayBuffer 中的每个字节视为一个单位。每个单位是 0 到 255 之间的数字。之所以是 255，是因为每个单位最多是 8 位，即 2^8 次方。

8. Uint16Array 将 ArrayBuffer 中每 2 个字节视为一个单位。每个单位是 0 到 65535 之间的整数。原理同上。

9. Uint32Array 将 ArrayBuffer 中每 4 个字节视为一个单位。每个单位是 0 到 4294967295 之间的整数。原理同上。

10. TypedArray 视图的构造函数接收的参数：
    - TypedArray 视图的构造函数接收三个参数，第一个 ArrayBuffer 对象，第二个是视图开始的字节号（默认0），第三个是视图结束的字节号（默认直到本段内存区域结束）
    - 接收 ArrayBuffer 实例作为参数，以指定格式读出二进制数据
    - 可接受普通数组作为参数，直接分配内存生成 ArrayBuffer 实例，并同时对这段内存进行赋值，再根据这段内存生成视图。
    - 可接受视图做为参数，生成的新数组复制了参数视图的值，生成新的数组和视图。（想基于同一内存生成新视图，需要传入视图.buffer）

11. TypedArray 视图和数组的区别
    - TypedArray 内的成员只能是同一类型。
    - TypedArray成员是连续的，不会有空位
    - TypedArray成员的默认值为0，数组的默认值为空
    - TypedArray只是视图，本身不存储数据，数据都存储在底层的ArrayBuffer中，要获取底层对象必须使用 Buffer对象的属性。
    - TypedArray 可以直接操作内存，不需要进行类型转换，所以比数组快。
    - 数组有的方法 TypedArray 都可以使用，但不能使用 cancat 方法，因为 TypedArray 操作的数据长度固定。

    
#### 4. DataView - 视图

1. 如果一段数据包括多种类型（比如服务器传来的 HTTP 数据），这时除了建立 ArrayBuffer 对象的复合视图以外，还可以通过 DataView 视图进行操作。

2. DataView 视图提供更多操作选项，而且支持设定字节序。本来，在设计目的上，ArrayBuffer 对象的各种 TypedArray 视图，是用来向网卡、声卡之类的本机设备传送数据，所以使用本机的字节序就可以了；而DataView视图的设计目的，是用来处理网络设备传来的数据，所以大端字节序或小端字节序是可以自行设定的。

3. DataView视图本身也是构造函数，接受一个ArrayBuffer对象作为参数，生成视图。

new DataView(ArrayBuffer buffer [, 字节起始位置 [, 长度]]);
下面是一个例子。

const buffer = new ArrayBuffer(24);
const dv = new DataView(buffer);
DataView实例有以下属性，含义与TypedArray实例的同名方法相同。

DataView.prototype.buffer：返回对应的 ArrayBuffer 对象
DataView.prototype.byteLength：返回占据的内存字节长度
DataView.prototype.byteOffset：返回当前视图从对应的 ArrayBuffer 对象的哪个字节开始
DataView实例提供 8 个方法读取内存。

getInt8：读取 1 个字节，返回一个 8 位整数。
getUint8：读取 1 个字节，返回一个无符号的 8 位整数。
getInt16：读取 2 个字节，返回一个 16 位整数。
getUint16：读取 2 个字节，返回一个无符号的 16 位整数。
getInt32：读取 4 个字节，返回一个 32 位整数。
getUint32：读取 4 个字节，返回一个无符号的 32 位整数。
getFloat32：读取 4 个字节，返回一个 32 位浮点数。
getFloat64：读取 8 个字节，返回一个 64 位浮点数。

#### 5. Buffer 与 ArrayBuffer




### 4. Buffer 中的编码格式

1. Buffer 中存储的二进制数据，也就一个字节（byte）一个字节的数据。当 Buffer 与字符串相互转换的时候，必须指定编码格式，如果没有指定编码格式，默认的编码格式是 utf-8。

## 3. Buffer 常用的 API

