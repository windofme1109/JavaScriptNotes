# puppeteer-screen-recorder 录屏基本原理

## 1. puppeteer 提供的实例与方法：

1. CDPSession 实例用来和原生的（浏览器）Chrome Devtools Protocol 进行通信。

2. CDPSession.send 用来调用原生 Chrome Devtools Protocol 协议的方法。

3. CDPSession.on 用来订阅原生的 Chrome Devtools Protocol 协议事件。

## 2. Chrome Devtools Protocol 提供的 API

1. Page.startScreencast 方法利用触发 screencastFrame 事件，开始发送每一个截屏帧。

2. Page.stopScreencast 方法会停止发送每一个截屏帧。

3. Page.screencastFrameAck 方法用来确认前端已经收到了截屏帧。

## 2. Chrome Devtools Protocol 提供的事件

1. Page.screencastFrame 事件。每当截屏结束时，就触发这个事件。

2. 压缩由 startScreencast 方法请求的图片数据，压缩的数据格式是 base64 编码。

## 3. FFMPEG

1. FFMPEG 用来生成视频。

## 4. Transform Stream

1. 使用 PassThrough（Transform Stream 的简易实现）将视频流写入文件。


## 参考资料

1. [startScreencast](https://chromedevtools.github.io/devtools-protocol/tot/Page/#method-startScreencast)

2. [getting-started-with-cdp](https://github.com/aslushnikov/getting-started-with-cdp/blob/master/README.md)

3. [CDPSession](https://pptr.dev/#?product=Puppeteer&version=v12.0.0&show=api-class-cdpsession)

4. [Understanding Streams In NodeJS](https://medium.com/bb-tutorials-and-thoughts/understanding-streams-in-nodejs-43736e7acb4b)

## 录屏流程

1. 初始化配置：
   - 视频保存路径（path）
   - 帧率（fps）
   - 视频长宽比（aspectRatio）
   - 分辨率（resolution）

2. 初始化 ffmpeg 配置：
   - 实例化可写流（使用 transform tream 实现，目的是将 buffer 数据转换成流）
   - 配置生成视频的命令选项
   - 配置输出视频的命令选项

3. 注册 CPD （Chrome Devtools Protocol）的截屏帧事件：Page.screencastFrame，每当 puppeteer 生成一帧的截屏图像，就会触发这个 Page.screencastFrame 事件。
''
4. 调用 CPD 的方法 Page.startScreencast 开始监听 Page.screencastFrame 事件，并启动 puppeteer 截屏功能。

5. Page.screencastFrame 事件触发后，Page.screencastFrame 的事件处理函数会接收 base64 编码的图像数据和对应的时间戳信息，在其内部调用 CDP 的方法 Page.screencastFrameAck 来确保前端收到了截屏帧。

6. 前端收到截屏帧后，将其转换成二进制形式的 buffer 数据，方便后面对截屏帧进行处理。

7. 将截屏帧数据按照时间戳进行排序，生成一个视频图像序列。捕获截屏帧有顺序，但是 Page.screencastFrame 事件并不一定按照捕获截屏帧的顺序触发，因此，我们有可能需要根据截屏帧的时间戳信息对形成视频图像序列进行顺序调整操作。

8. 根据相邻帧的时间戳信息，计算出二者之间的时间差（duration），然后根据帧率，进行补帧操作。

9. 调用可写流的 write 方法，将截屏帧数据写入流中。

10. ffmpeg 将写入流数据转换成视频。



