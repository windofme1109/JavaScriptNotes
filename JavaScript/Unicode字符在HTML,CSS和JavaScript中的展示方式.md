# 1. Unicode 字符在HTML、CSS 和 JavaScript 中的展示方式

## 1. 参考资料

1. [HTML特殊转义字符对照表](https://tool.oschina.net/commons?type=2)

2. [Unicode与前端字符编码全揭秘](https://juejin.cn/post/7070079762429034526)

## 2. Unicode 字符

1. Unicode 字符可以使用 `U+十六进制码点值` 的格式来表示一个 Unicode 码点，例如，汉字 "元" 的十六进制表示是：`U+5143`，字母 "a" 十六进制表示是 `U+0061`。注意，此格式仅用于描述，在实际定义中，每个码点都是数字，并没有前缀 `U+`。

## 3. 在 HTML 中的展示

1. html 中展示 Unicode 字符，使用 10 进制形式，形式如下：`&#10进制数字;`，注意，结尾的分号不能省略。如 `&#23433;` 表示的是汉字 "安"。

2. 示例：
```html
<p>&#23433;</p>
```

3. 一下特殊的字符，如 `§`、`©`，需要使用 html 转义字符表示，一些常见的 html 转义字符可参考：[HTML特殊转义字符对照表](https://tool.oschina.net/commons?type=2)。

## 4. 在 CSS 中的展示

1. css 中使用 Unicode 字符，使用的是 16 进制形式，形式如下：`\16进制数字`。

2. 示例：
```css
p::before {
    content: '\200B'
}
```
`U+200B` 就是零宽字符的 16 进制形式。

3. 多数情况下，我们在伪元素的 `content` 属性中使用 Unicode 字符。

## 5. 在 JavaScript 中使用

1. JavaScript 中使用 Unicode 字符，使用的是 16 进制形式，形式如下：`\u16进制数字`。如：`\u20A0` 表示的是 `₠`。

2. 示例 - 直接打印：
```javascript
console.log('\u20A0');  // ₠

console.log('\u5b89');  // 安
```

3. 示例 - 正则表达式匹配中文：
```javascript
const pattern = /[\u4e00-\u9fa5]/g;
```

## 4. 如何获取一个字符的编码

1. 使用 `charCodeAt` 方法获取一个字符的编码，只不过这个编码是 10 进制形式。

2. 示例：
```javascript
'元'.charCodeAt() // 20803
String.fromCharCode(20803) // 元
```

3. 如果需要在 js 或者 css 中使用，需要将 10 进制的编码转换为 16 进制，可以使用 toString() 进行转换。

4. 示例：
```javascript
(20803).toString(16) // '5143'
```


