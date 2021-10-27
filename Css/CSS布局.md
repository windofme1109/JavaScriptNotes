<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [css 基础的布局](#css-%E5%9F%BA%E7%A1%80%E7%9A%84%E5%B8%83%E5%B1%80)
  - [1. 两列布局](#1-%E4%B8%A4%E5%88%97%E5%B8%83%E5%B1%80)
  - [2. 三列布局](#2-%E4%B8%89%E5%88%97%E5%B8%83%E5%B1%80)
    - [1. 圣杯布局](#1-%E5%9C%A3%E6%9D%AF%E5%B8%83%E5%B1%80)
    - [2. 双飞翼布局](#2-%E5%8F%8C%E9%A3%9E%E7%BF%BC%E5%B8%83%E5%B1%80)
    - [3. flex](#3-flex)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# css 基础的布局

## 1. 两列布局

1. 要求：左边固定宽度，右边自适应

2. 实现：  
   html结构如下：
   ```html
     <div class="container">
        <div class="left">左侧</div>
        <div class="right">右侧</div>
     </div>
   ```
   
3. 浮动布局（两种方法）   
   - css样式如下：
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
   - css 代码：
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
   - css 代码：
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
     
## 2. 三列布局

要求：两边固定宽度，中间自适应

### 1. 圣杯布局

### 2. 双飞翼布局

### 3. flex

1. html结构是：
   ```html
      <div class="container">
         <div class="left">我是左边内容</div>
         <div class="main">我是主要内容</div>
         <div class="right">我是右边内容</div>
      </div>
   ```
2. css样式：
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