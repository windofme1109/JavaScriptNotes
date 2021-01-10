### 1. 两列布局
1. 要求：左边固定宽度，右边自适应
2. 实现：  
   html结构如下：
   ```html
     <div class="container">
        <div class="left">左侧</div>
        <div class="right">右侧</div>
     </div>
   ```
   
   1. 浮动布局（两种方法）   
   css样式如下：
      ```css
          .container {
               overflow: hidden;
          }
          .container .left {
              float: left;
              width: 200px;
              background: #005cc5;
          }
          .container .right {
              background: #669fff;
              /*方案1 使用BFC。BFC区域不会与float元素发生重叠，使用overflow:hidden来触发BFC，右侧就不会与左侧发生重叠。*/
              overflow: hidden;
              /*方案2 margin-left，左侧区域空出来*/
              margin-left: 200px;
          }
      ```
   2. 绝对定位（两种方法）
      ```css
          .container {
              position: relative;
          }
          .container .left {
              position: absolute;
              top: 0;
              left: 0;
              width: 200px;
              background: #005cc5;
          }
          .container .right {
              background: #669fff;
              /*方案1 margin-left，左侧区域空出来*/
              margin-left: 200px;
          }
      ```
   3. flex布局
      ```css
         .container {
             display: flex;
         }
         .container .left {
             width: 200px;
             background: #005cc5;
         }
         .container .right {
             flex: 1;
             background: #669fff;
         }
      ```
### 2. 三列布局
要求：两边固定宽度，中间自适应
#### 1. 圣杯布局
#### 2. 双飞翼布局
#### 3. flex
html结构是：
```html
   <div class="container">
       <div class="left">我是左边内容</div>
       <div class="main">我是主要内容</div>
       <div class="right">我是右边内容</div>
   </div>
```
css样式：
```css
   .container {
       display: flex;
   }
   .left {
       width: 100px;
       background: #005cc5;
   }
   .main {
      flex: 1;
      background: #1dc400;
   }
   .right {
      width: 200px;
      background: #5b6169;
   }
```   