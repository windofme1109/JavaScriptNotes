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

4. `MiniCssExtractPlugin.loader` 中 options 中的 `publicPath` 作用说明
   - 在使用 mini-css-extract-plugin 这个插件后，需要将 css 单独打包，插件的配置如下：
     ```javascript
        module.exports = {
        
            plugins: [
                new MiniCssExtractPlugin({
                    filename: 'styles/[name].css',
                    chunkFilename: '[name].chunk.css',
                    })
            ]
        }
     ``` 
     `filename` 指定了存放路径，就是 dist 目录下的 styles 文件夹下。
   - 在 打包 css 或者 sass 或者 less 中，使用 `MiniCssExtractPlugin.loader` 替换 `style-loader`。
   - 这样就带来一个问题，就是如果在 css 中引入图片，如下所示：
     ```css
        body {
            background: url("../public/pics/background-image.jpg");
        }    
     ```
   - 就会出现打包后引用图片的相对路径的问题，说明如下：
     - 首先，打包后的图片的都放在 dist 下的 images 中
     - webpack 能够识别 css 中图片的相对路径中的图片的存放目录，如打包前，图片存放在 pics 下
     - 打包后，webapck 就会将存放目录替换为 images
     - 路劲转换是这样的：`"../public/pics/background-image.jpg"  --> "styles/images/background-image.jpg"`
     - 但是，webpack 默认地认为 images 是与 css 文件同级地，即都在 styles 目录下
     - 所以需要 `publicPath` 来解决路径的问题
   - 解决办法就是设置 `publicPath`。
     - 设置了 `publicPath`，会将 `publicPath` 与 `/images/xxx` 进行拼接，`/images/xxx` 就是存放图片的目录。
     - 如 `publicPath` 设置为：`./images`
     - 拼接：`/styles/images/images/background-image_2f852098701a67fc5e60c7e299d15685.jpg`
     - `publicPath` 设置为：`./`
     - 拼接：`/styles/images/background-image_2f852098701a67fc5e60c7e299d15685.jpg`
     - 所以设置 `publicPath` 为：`../`，表示 styles 的上级目录，即 dist
     - 最终的路径：`images/background-image_2f852098701a67fc5e60c7e299d15685.jpg`
   
5. `publicPath` 是将其指定的路径与打包后直接存放图片的路径进行拼接。

6. 解决 webpack v5 在使用 devServer 提示 `Automatic publicPath is not supported in this browser`：则需要配置 `publicPath`。

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