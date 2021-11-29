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