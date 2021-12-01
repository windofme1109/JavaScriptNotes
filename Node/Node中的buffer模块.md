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

## 2. Buffer 的基本说明

### 1. Node 中的 Buffer
1. 官网上是这样对 buffer 进行说明的：
   > Buffer objects are used to represent a fixed-length sequence of bytes. Many Node.js APIs support Buffers.
   >
   > The Buffer class is a subclass of JavaScript's Uint8Array class and extends it with methods that cover additional use cases. Node.js APIs accept plain Uint8Arrays wherever Buffers are supported as well.
   >
   > The Buffer class is within the global scope, making it unlikely that one would need to ever use require('buffer').Buffer.

2. 翻译过来，就是说：
   - Buffer 对象用来表示固定长度的字节序列，许多 Node.js 的 API 支持 Buffer。
   - Buffer 类是 JavaScript 中的 Uint8Array 类的子类，并使用覆盖其他用例的方法对其进行扩展，Node.js 的 API 支持纯的 Uint8Array，而 Buffer 也支持。
   - 全局环境下存在 Buffer 类，因此，我们无需导入即可使用 Buffer。

3. JavaScript 语言没有读取或操作二进制数据流的机制。 Buffer 类被引入作为 Node.js API 的一部分，使其可以在 TCP 流或文件系统操作等场景中处理二进制数据流。也就是说，引入了 Buffer，使得我们可以使用 Node.js 的 API 直接操作二进制数据。


### 2. Buffer 的解释

1. Buffer 的含义是缓冲区，用来缓冲二进制数据。众所周知，计算机中的图片、视频、音频等各种文件，本质上都是一堆二进制的数据：`0100111000101`。处理这些数据，需要将这些数据传送到 cpu 或者 gpu 那里。

2. 一般来说，如果传送数据的速度和 cpu 处理的速度相当，即传来多少数据，cpu 就能处理多少数据，这种情况就不会浪费时间。但是，这样涉及到一个问题，就是如果传送数据的速度快于 cpu 处理的速度，那么就会有一部分数据处于等待状态，如果传输数据的速度慢于 cpu 处理的速度，那么 cpu 就要等待，只有足够多的数据构成有意义的能解读的整体，cpu 才能继续处理。

3. 理想情况下，我们希望数据传输的速度和 cpu 的处理速度一致，传来多少数据，cpu 就能处理多少数据。但实际上，不可能达到理想状况，数据传输的速度有可能比 cpu 处理的快，也有可能慢，无论哪种情况，我们都需要在内存中开辟一个固定大小的区域，用来存放这些暂时用不到的数据，如果数据传输速度比 cpu 快，这个区域就存着提前到达的数据；如果数据传输速度比 cpu 慢，这个区域就存够一批足够能用的数据再给 cpu，因此，这个区域就被称为缓冲区，即 Buffer。

### 3. Buffer 与 Stream

1. 数据据流（stream of data）是从一个源头（source）向目的地（destination）传输数据的过程。

2. 创建一个可读流以后，从源头源源不断地读取数据，然后将数据放到一个内存区域中，当这个内存区域满了以后，就暂停读取数据，等待消费者消费，当消费者将区域中的数据消费完以后，读取数据的过程恢复，继续向内存中添加数据。

3. 上面提到的暂时存放数据的内存区域，就是缓冲区 —— Buffer。Buffer 作为可读流或者可写流的暂存数据的区域，肩负着缓存数据的责任。没有消费者消费数据，数据就从源头放到 Buffer 中暂存。有消费者消费如果消费者消费数据比较快，或者从数据传送地慢，导致 Buffer 没有被填满，那么消费者就得等待。 

4. 一个有关 Buffer 和 Stream 的典型例子就是我们在线看视频的时候。视频数据是以流的方式从服务器传送到客户端的，即视频是一块一块的传送过来的。在客户端有一个 Buffer 来接收视频流数据，当这个 Buffer 满了以后，客户端才会消费这些视频数据，也就是播放视频。如果我们的网络足够快，数据流（Stream）就可以足够快，可以让 Buffer 迅速填满然后发送和处理，然后处理下一个，再发送，再处理另一个，再发送，然后整个视频流就传输完毕，我们观看视频就可以一路到底。

5. 但是当我们的网络连接很慢，当处理完当前的数据后，我们的播放器就会暂停，或出现缓冲/加载等类似的字符，这表示客户端正在收集更多的数据，或者等待更多的数据到来，才能下一步处理。当 Buffer 装满并处理好，客户端就会显示数据，也就是播放视频了。在播放当前内容的时候，更多的数据也会源源不断的传输、到达并存在 Buffer 中。如果播放器已经处理完或播放完前一个数据，Buffer 仍然没有填满，缓冲/加载等类似的字符就会再次出现，等待和收集更多的数据。

## 3. Buffer 常用的 API

