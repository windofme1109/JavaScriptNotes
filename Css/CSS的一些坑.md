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