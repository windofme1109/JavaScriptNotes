# webpack 打包图片

## 1. 背景

1. 在 webpack 中，引入图片是一个值得我们注意的地方，如果配置不正确，则图片不会被打包，或者打包后引入的路径不对。所以有必要总结一下，webpack 中打包图片的几种方式。

2. 参考资料
   - [webpack踩坑之路 (2)——图片的路径与打包](https://www.cnblogs.com/ghost-xyx/p/5812902.html)
   - [2-webpack4.x打包图片三大方法](https://www.jianshu.com/p/ee741b1dbac9)

## 2. 配置 file-loader

1. 打包图片的静态资源，推荐使用 file-loader 或者 url-loader。关于这两个 loader 的用法，这里不再详述。

2. file-loader 中的 `output.publicPath` 表示资源的发布地址，当配置过该属性后，打包文件中所有通过相对路径引用的资源都会被配置的路径所替换。 

3. 

## 3. js （React）中引入

1. 在 js 中，想要引入图片等静态资源，不能使用绝对路径或者相对路径的方式，因为这样引入的图片不会被打包。

2. 在 js 中想要引入图片，必须通过模块导入的方式，无论使用 commonJS 或者是 ES Module 的方式，导入后，才可以使用，如下所示：
   - js
     ```javascript
        import favImg from "./09.jpg";
        
        const img = document.createElement('img');
        img.src = favImg;
        const divElement = document.querySelector('.root');
        divElement.appendChild(img);
     ```
    - react
      ```jsx
         import favImg from "./09.jpg";
         function App(props) {
              return <img src={favImg} />
         }
      ```
3. webpack 会对引入的图片进行打包，并根据图片的相对路径，在打包生成的文件中，生成新的相对路径，这里可能不需要配置 `output.publicPath`。


## 4. css 中引入

1. 引入一个背景图片：
   ```css
      div {
         background: url('./public/background-image.jpg') no-repeat;;
      }
   ```
2. 背景图片的引入方式是通过相对路径的方式引入的。因此 webpack 能识别 css 中的通过相对路径引入的静态资源，然后将这些图片打包，并根据图片的相对路径，在打包生成的文件中，生成新的相对路径。

3. 如果我们配置了 `output.publicPath`，那么打包后的 css 文件中，通过相对路径引用的资源都会被配置的路径所替换，即在相对路径的前面拼接 `output.publicPath` 指定的路径。该属性的好处在于当你配置了图片 CDN 的地址，本地开发时引用本地的图片资源，上线打包时就将资源全部指向 CDN 了。


## 5. html 中引入

1. 通过 html 中的 img 标签引入图片。需要我们安装另外一个 loader —— html-withimg-loder。

2. 安装  html-withimg-loder：`npm install html-withimg-loader --save-dev`

3. 配置：
   ```javascript
      // webpack.config.js
      module.exports = {
          module: {
              rules: [
                  {
                      test: /\.html$/,
                      use: 'html-withimg-loder'
                  }
              ]
          }
      }
   ```
4. 在 html 文件中正常使用 img 标签就好了：
   ```html
      <div>
          <img src="./src/img.jpg" alt="">
      </div>
   ```