### 1. flex布局的基本说明
- flex布局  
   - flex是flexbox的简称，是CSS的一种布局方案。可以实现响应式地实现各种布局。
   - 使用flex布局，我们只需要声明是哪种形式，剩下的就是浏览器根据空间的大小实现具体的布局。这就是flex布局灵活之处。不需要我们手动去计算。
### 2. flex布局语法
1. 任何一个容器都可以指定为flex布局：`div.box {display: flex;}`
2. 行元素也可以指定为flex布局：`a.local-nav {display: inline-flex;}`
3. 设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效。
### 3. 基本概念
1. 采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。  
![](img/flex布局说明.png)  
2. 容器默认存在两个轴线，水平方向的轴线和垂直方的轴线。默认情况下，水平方向的轴线被称为主轴（main axis），从左向右，垂直方向的轴线叫做侧轴（cross axis），从上到下。主轴的起始位置被称为main start，结束位置被称为main end。侧轴起始位置被称为cross start，结束位置被称为cross end。
3. 项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。
### 4. 容器属性
1. 容器属性一共有6个，分别是：
   - flex-direction
   - flex-wrap
   - justify-content
   - align-items
   - align-content
   - flex-flow
2. flex-direction
   - 用来设置主轴的方向。有4个值：`div.box {flex-direction: row|row-revers|column|column-reverse}`
   - 四个值的说明如下：
     - row（默认值）：主轴为水平方向，起点在左端。
     - row-reverse：主轴为水平方向，起点在右端。注意，项目的属性也会跟着颠倒。举个例子：设置为row，则项目顺序为123，若设置为row-reverse，则项目顺序是321。
     - column：主轴为垂直方向，起点在上沿。
     - column-reverse：主轴为垂直方向，起点在下沿。
   - 注意，主轴的方向是不确定的，需要由flex-direction确定。
3. flex-wrap
   - 默认情况下，所以的项目都会挤在一行。如果项目的宽度（或高度）总和超过了容器的宽度（或高度），那么浏览器会自动缩小每个项目的宽度或者高度，使得这些项目还是在一行。
   - flex-wrap的作用就是用来设置一行放不下这些项目时，如何换行。
   - flex-wrap有3个值可选： `div.box {flex-wrap: nowrap|wrap|wrap-reverse}`
   - 3个值的说明如下：
     - nowrap（默认值）：不换行。
     - wrap：换行，第一行在上方。
     ![](img/wrap.jpg)
     - wrap-revers：换行，第一行在下方。
     ![](img/wrap-reverse.jpg)
4. justify-content
   - justify-content定义了项目在主轴上的对齐方式。
   - **注意，使用这个属性前，一定要确定好主轴是哪个方向。**
   - justify-content有5个值：`div.box {justify-content: flex-start|flex-end|center|space-around|space-between}`
   - 5个值的说明如下（具体对齐方式与主轴的方向有关。下面假设主轴为从左到右）：
     - flex-start（默认值）：左对齐
     - flex-end：右对齐
     - center： 居中
     - space-between：两端对齐，项目之间的间隔都相等。
     - space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
   ![](img/justify-content.png)
5. align-items
   - 定义项目在交叉轴上如何对齐。适用于只有一行的情况。
   - align-items一共有5个值：`div.box {align-items: flex-start|flex-end|center|stretch|baseline}`
   - 4个值的说明如下（具体的对齐方式与侧轴的方向有关，下面假设侧轴从上到下）：
     - flex-start：侧轴的起点对齐。
     - flex-end：侧轴的终点对齐。
     - center：侧轴的中点对齐。（侧轴是垂直方向，可以实现垂直居中）
     - baseline: 项目的第一行文字的基线对齐。
     - stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
   ![](img/align-items.png)
6. align-content
   - 定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。就是说，如果有多行项目，如何与侧轴对齐。
   - 一共有6个值可选：`div.box {align-content: flex-start|flex-end|center|space-around|space-between|stretch}`
   - 6个值的说明如下：
     - flex-start：与侧轴的起点对齐。
     - flex-end：与侧轴的终点对齐。
     - center：与侧轴的中点对齐。
     - space-between：与侧轴两端对齐，轴线之间的间隔平均分布。
     - space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
     - stretch（默认值）：轴线占满整个侧轴。
   ![](img/align-content.png)
7. flex-flow
   - 这个属性是一个复合属性，把flex-direction和flex-wrap两个属性合起来一起写。
### 5. 项目属性
1. 项目的属性主要有flex、align-self、order。
2. flex
   - 属性定义了子项目占据的份数。
   - 使用这个属性的前提是不指定子元素的宽度或高度，这样可以根据剩余空间，以及子元素占几份，计算实际宽度或高度。
   - 注意，项目设置了这个属性，将确定位置或宽高的元素安置妥当后，再根据剩余空间决定项目占据多少份。
   - 可以使用这个属性实现两列、三列布局。
   - 设置方法：`div.item {flex: 1; /*default 0*/}`
   如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。
3. align-self
   - 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。 
   - 简单的说，允许我们给单独的项目设置对齐方式。主要的值的6个：`div.item {align-self: auto|flex-start|flex-end|center|baseline|stretch}`
   - 除了auto，其余属性与align-items的属性值一样。
4. order
   - 定义项目的排列顺序。数值越小，排列越靠前。
   - 设置方法：`div.item {order: -1; /*default 0*/}`



