<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [使用 CSS 时遇到的一些坑](#%E4%BD%BF%E7%94%A8-css-%E6%97%B6%E9%81%87%E5%88%B0%E7%9A%84%E4%B8%80%E4%BA%9B%E5%9D%91)
  - [1. `transform` 与 `position: fixed`](#1-transform-%E4%B8%8E-position-fixed)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 使用 CSS 时遇到的一些坑

## 1. `transform` 与 `position: fixed`

1. 元素设置了  `position: fixed` 以后，元素就相对于 viewport 固定下来，脱离了文档流，固定在页面的某个位置。

2.  元素设置了  `transform` 属性以后，`position: fixed` 这个属性就会失效，降级为 `absolute` 绝对定位。相对的是它的父元素。会随着文档流的移动而移动。

3. 原因是什么呢？
   - W3C 对于 [transform](https://www.w3.org/TR/css-transforms-1/#transform-property) 的解释：
     > 对于布局受CSS盒子模型控制的元素，transform属性不会影响transformed元素周围的内容流。但是，溢出区域的范围将会考虑上transform元素。这种行为类似于元素通过相对定位发生偏移时的情况。因此，如果overflow属性的值是`scroll`或`auto`的，滚动条将显示为需要看到在可见区域外转换的内容。

     >对于布局受CSS盒子模型控制的元素，除了配置为`none`之外，其他的`transform`属性值都会创建堆栈上下文。绘制的实现必须在其父栈上下文中它所创建的层中，如果它是带有“z-index: 0”的定位元素，则使用相同的堆叠顺序。如果一个带有`transform`的元素还配置了`position`属性，那么“z-index”属性将按照[CSS2](https://www.w3.org/TR/css-transforms-1/#biblio-css2)描述的被应用，除非“auto”被视为“0”，因为会创建新的堆栈上下文。

     >对于布局受CSS盒子模型控制的元素，除了配置为`none`之外，其他的`transform`属性值都将导致元素成为一个包含块，而其固定定位的后代元素都是以此object作为他们的包含块。

      >根元素的[Fixed Backgrounds](https://www.w3.org/TR/css3-background/#fixed0)会受到该元素上配置的transform属性的影响。对于受transform影响的所有其他元素(例如，对它们应用transform属性，或者对它们的任何祖先元素应用transform属性)，  `background-attachment`属性值为`fixed`的元素会被当做它好像有配置`scroll`属性一样。其他`background-attachment`的计算值不受影响。
   - 具体而言，应用了 transform 属性的元素会导致该元素形成一个新的包含块，然后其后代元素如果有 fixed 定位的属性，那么其元素将会以该父元素作为包含块，从而会导致 fixed 布局失效。
   - fixed 元素不在固定在某个位置，失去了fixed元素特有的性质
   - fixed 元素不会脱离文档流，但是top等属性依然可用。

4. 解决方案
   - 暂时不要将使用了 transform 属性的 position 设置为 fixed。

5. 参考资料
   - [CSS3 transform对普通元素的N多渲染影响](https://www.zhangxinxu.com/wordpress/2015/05/css3-transform-affect/)
   - [transform与position:fixed的那些恩怨](https://zhuanlan.zhihu.com/p/95021620)

## 2. 元素设置 `inline` 属性和 `inline-box` 的一些注意事项

### 1. `inline` 属性需要注意的地方

1. 元素设置了 inline 属性，那么这个元素变成了行内元素，因此元素不会占据一行，而是和其他行内元素一样，挤在一行中。

2. 但是需要注意的是，元素变成行内元素以后，其内部是不能嵌套块元素的。假如说，我们在行内元素中嵌套了块元素，那么由于行内元素本身没有宽高，因此，其会被内部块元素撑开，因而占据一整行。此时这个行内元素从视觉上也是占据一行的。此种情况下，行内元素就不起作用了。

3. 如果势必要在行内元素中嵌套块元素，还想保持行内元素的特性，那么需要设置元素的 `display` 属性为 `inline-block`，那么这个元素就同时具有了行内元素和块元素的特性。

### 2. `inline-block` 属性需要注意的地方

1. 如果一个元素的 `display` 属性为 `inline-block`，那么这个元素就同时具有了行内元素和块元素的特性。

2. 设置了 `display` 属性为 `inline-block` 的元素，其默认的垂直（vertical）方向是沿着基线（baseline）对齐的。如下图所示：
   ![img.png](img/inline-block.png)
   图片来源：[关于inline-block对齐的问题](https://www.jianshu.com/p/9e0274e0f9bd)

3. 要想让各个元素垂直方向上对齐，可以设置其 `vertical-align` 属性为`top` 即可。

4. 关于属性 `vertical-align`：
    - 该属性用来设置 inline、inline-box 或者 table-cell 元素在竖直方向上的对齐方式，默认值为 baseline。
    - 该属性对于块级盒子无效。
    - 因为是垂直方向的对齐是相对于父级元素来说的，所以其设置的值代表了该元素相对于其父级如何对齐，如 ：
        - 当 `vertical-align: baseline` 时，该元素会将自身的基线（baseline）与其父元素的基线对齐。
        - 同理当 `vertical-align: vertical-top` 时，该元素会将自身的顶部与其父元素的顶部对齐。
    - 关于 `vertical-align` 的详细说明： [vertical-align MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/vertical-align)

5. 关于基线（baseline）、`inline` 元素的基线和 `inline-block` 元素的基线，请看博客：[关于inline-block对齐的问题](https://www.jianshu.com/p/9e0274e0f9bd)

### 3. 一个块元素中，`margin-right` 的计算问题

1. 对于块元素，有一些情况下，我们没有设置 margin-right，或者 设置了 margin-right，但是经过浏览器渲染以后，我们发现实际的 margin-right 值并不是我们设置值，这个是怎么回事呢？示例如下：
```html
    <style>
         .test {
             width: 200px;
             height: 100px;
             margin-left: 25px;
             margin-right: 25px;
             background: yellowgreen;
         }
    </style>
    <div class="test"></div>
```
2. 实际渲染效果：
   ![img.png](img/margin-right-1.png)
   ![img_1.png](img/margin-right-2.png)

3. 可以看出，`margin-right` 并不是设置的 `25px`，而是父容器的宽度除去 `margin-left`、元素的内容区宽度（`width`）之后的宽度。

4. 出现这种现象的原因是：如果元素的 `margin-left`、`margin-right` 被设为固定值，且其值加上元素宽度小于父容器容纳块的宽度，则 `margin-right` 会自动隐式变为 `auto`（宽度填满父容器）。

5. 在 css 规范中有这样一段描述：
   > 10.3.3 Block-level, non-replaced elements in normal flow
   The following constraints must hold among the used values of the other properties:
   >
   > 'margin-left' + 'border-left-width' + 'padding-left' + 'width' + 'padding-right' + 'border-right-width' +'margin-right' = width of containing block

6. 上面的这个公式是用来计算某个属性如果设置为 `auto` 或者没有设置这个属性时，这个属性实际的值应该是多少。所以，在从左向右（ltr）布局中，优先使用 margin-left、width，同时为了满足这个公式，如果 `'margin-left' + 'border-left-width' + 'padding-left' + 'width' + 'padding-right' + 'border-right-width' +'margin-right'` 小于 `containing block`，即使我们设置了 `margin-right`，浏浏览器也不会采用，而是设置 `margin-right` 为剩余的宽度。