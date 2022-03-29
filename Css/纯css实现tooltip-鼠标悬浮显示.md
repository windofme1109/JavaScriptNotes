# 纯css实现tooltip-鼠标悬浮显示

## 1. 要求

1. 界面上有一个 Button

2. 鼠标 hover 上去后会在 Button 上方显示一个 tooltip

3. 这个 tooltip 有圆角，下方有一个小三角形

## 2. tooltip 的实现

1. 因为要求有小三角形，这个小三角形可以通过 css 的 border 属性实现。

2. 三角形在下方，可以通过伪元素结合定位实现。就是说，将伪元素设置为一个三角形，然后使用绝对定位固定到下面。

3. tooltip 的 html 结构如下：
   ```html
        <div class="tooltip">
            这是一个提示内容
        </div>
   ```
4. tooltip 的 css 实现如下：
   - tooltip 的 css 实现：
     ```css
        .tooltip {
            width: 100px;
            height: 50px;
            padding: 10px 0;
            border: 1px solid #666;
            border-radius: 5px;
            background: #666;
        }
        
     ```
   - 伪元素实现的三角：
     ```css
        .tooltip::after {
            content: '';
            display: block;
            position: absolute;
            top: 70px;
            right: 35px;
            width: 0;
            height: 0;
            border: 15px solid #666;
            border-color: transparent transparent #666 transparent;
            transform: rotate(180deg);
        }
     ```
     实现了三角形效果后，需要将其旋转 180 度，使其变成倒三角的效果，因此这里使用了 rotate 这属性。
## 3. 鼠标 hover 出现 tooltip

1. 因为要求是 css 实现 hover 效果，那么只能使用 hover 伪类。

2. hover 伪类不能操作相邻测兄弟元素，只能操作当前元素及其子元素，因此，前面的 tooltip 这个元素不能和 button 做兄弟元素。

3. 可以设置一个父元素，将 button 和 tooltip 作为其子元素。使 tooltip 脱离文档流，这样父元素是由 button 撑开的高度。然后使用定位，将 tooltip 定位到 button 上面。因为此时父元素和 button 是同高同宽，那么我们只需要给父元素设置 hover 效果，从视觉上，就是给 button 设置 hover。当父元素被触发 hover 效果的时候，我们就可以操作子元素 tooltip，实现 tooltip 的显示与隐藏。

4. html 结构：
   ```html
      <div class="container">
        <div class="tooltip">
            这是一个提示内容
        </div>
        <button class="my-button">click</button>
    </div>
   ```
5. css 代码：
   - 父元素 container 样式：
     ```css
        .container {
            position: relative;
            width: 50px;
            height: 25px;
            margin: 150px auto;
        }
     ```
     position 为 relative 的原因是子元素 tooltip 要设置绝对定位。  
     设置 margin-top 的原因是给 tooltip 的出现留出一个空地。
   - tooltip 的样式：
     ```css
        .tooltip {
            display: none;
            width: 100px;
            height: 50px;
            position: absolute;
            left: -25px;
            top: -85px;
            padding: 10px 0;
            /*height: 100px;*/
            border: 1px solid #666;
            border-radius: 5px;
            background: #666;
        }
        .tooltip::after {
            content: '';
            display: block;
            position: absolute;
            top: 70px;
            right: 35px;
            width: 0;
            height: 0;
            border: 15px solid #666;
            border-color: transparent transparent #666 transparent;
            transform: rotate(180deg);
            /*background-color: red;*/
        }
     ```
     因为要将 tooltip 移动到 button 的上面，也是父元素的上面，在 tooltip 设为绝对定位的前提下，设置 top 和 left。top 是整个tooltip 的高度，包括由伪元素生成的三角，其总高度为：50 + 10 + 10 + 15 = 85，所以需要向上移动 85px。
   - button 的样式：
     ```css
        .my-button {
            /*margin: 15px 30px;*/
            width: 100%;
            height: 100%;
        }
     ```
     这样保证 button 和父元素是同高同宽的。
   - 触发 hover：
     ```css
        .container:hover .tooltip {
            display: block;
        }
     ```
     父元素 container 的 hover 操作被触发的时候，可以操作子元素的状态，因此可以设置 子元素 tooltip 的display 属性为 block，即可显示 tooltip。
