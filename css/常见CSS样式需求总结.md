<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [常见 CSS 样式](#%E5%B8%B8%E8%A7%81-css-%E6%A0%B7%E5%BC%8F)
  - [1. 文本超出长度隐藏，并显示省略号（...）](#1-%E6%96%87%E6%9C%AC%E8%B6%85%E5%87%BA%E9%95%BF%E5%BA%A6%E9%9A%90%E8%97%8F%E5%B9%B6%E6%98%BE%E7%A4%BA%E7%9C%81%E7%95%A5%E5%8F%B7)
    - [1. 单行文本](#1-%E5%8D%95%E8%A1%8C%E6%96%87%E6%9C%AC)
    - [2. 多行文本](#2-%E5%A4%9A%E8%A1%8C%E6%96%87%E6%9C%AC)
  - [2. span 鼠标悬浮显示省略文字](#2-span-%E9%BC%A0%E6%A0%87%E6%82%AC%E6%B5%AE%E6%98%BE%E7%A4%BA%E7%9C%81%E7%95%A5%E6%96%87%E5%AD%97)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 常见 CSS 样式

## 1. 文本超出长度隐藏，并显示省略号（...）

### 1. 单行文本

1. 参考资料：[text-overflow属性的使用](https://blog.csdn.net/weixin_41342585/article/details/79522065)

2. css 样式：
   ```css
      overflow: hidden;
      text-overflow:ellipsis;
      white-space: nowrap;
   ```
2. 为了保证兼容性，最好给元素添加上 `width` 属性。

3. `text-overflow` 对块级元素有效，对行内元素无效，所以要对行内元素如span使用这个属性，需要将其 `display` 设置为 `block` 或者 `inline-block`。然后给这个元素设置 `width` 属性。


### 2. 多行文本

1. 参考资料：[CSS单行、多行文本溢出显示省略号](https://segmentfault.com/a/1190000009262433)

2. css 样式：
   ```css
      overflow:hidden;
      text-overflow:ellipsis;
      display:-webkit-box;
      -webkit-line-clamp:2; (两行文字)
      -webkit-box-orient:vertical;
   ```
3. 在webkit浏览器或移动端（绝大部分是webkit内核的浏览器）可以直接使用webkit 的 css 扩展属性（webkit是私有属性）`-webkit-line-clamp`。

4. 注意：这是一个不规范的属性，它没有在CSS的规范草案中。
5. `-webkit-line-clamp` 用来限制在一个块元素显示的文本行数，为了实现效果，他要与其他的 webkit 属性结合使用：
    - `display:-webkit-box;`：必须结合的属性，将对象作为弹性伸缩盒子模型展示
    - `ebkit-box-orient`：必须结合的属性，设置或检索伸缩盒对象的子元素的排列方式

6. 更通用的方式：
   ```css
      p {
           position:relative;
           line-height:1.4em;
           /*设置容器高度为3倍行高就是显示3行*/
           height:4.2em;
           overflow:hidden;
      }
      p::after {
           content:'...';
           font-weight:bold;
           position:absolute;
           bottom:0;
           right:0;
           padding:0 20px 1px 45px;
           background:#fff;
       }
   ```
   7. 设置相对定位的容器高度，用包含省略号（...）的元素模拟实现

   8. 还可以通过 js 的方式实现。









## 2. span 鼠标悬浮显示省略文字

1. 由于 span 元素宽度有限，文字内容比较多的时候，我们设置了文字超出宽度隐藏并显示省略号的效果。

2. 设置 span 元素的 `title` 属性，title 内容为文本内容，这样当鼠标悬浮到span 元素上面的时候，就能显示完整的文本内容。

3. 示例：
   ```jsx
      <div className="app-detail-introduction" style={{paddingTop: '25px'}}>
            <span style={{display: 'inherit'}} title={formVal.appInstruction}>产品说明：{formVal.appInstruction || '生态产品说明'}</span>
       </div>
   ```