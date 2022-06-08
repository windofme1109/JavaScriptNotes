# Vue2 的基本用法

## 1. Vue2 中的插值语法

1. Vue 使用模板语法声明式地将数据渲染进 DOM 的系统。在一个 html 文件中允许我们这样声明：
   ```html
      <div id="root">
          {{name}}
      </div>
   ```
2. 双大括号 `{{}}` 被称为是插值语法，双大括号里面包裹的是一个合法的 js 表达式。Vue 在解析这段模板的时候，会对执行这个表达式，将表达式替换为最终的结果。

3. 为了能顺利解析上面这段模板，我们需要引入 Vue，并且要声明一个 Vue 的实例：
   - 在 html 中引入 Vue：`<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>`
   - 声明 Vue 实例：
       ```html
           <script>
               new Vue({
                  el: '#root',
                  data: {
                     name: 'world'
                  }
               })
           </script>
       ```
     Vue 是一个构造函数，这个构造函数接收一个配置对象作为参数。这个配置项的作用就是定义我们需要的数据、函数以及其他的 Vue 提供的内容。这里不对配置项进行详细说明。只说我们用到的：
     - `el`：容器元素，每个 Vue 实例都需要一个容器元素，在这个指定的容器元素内，Vue 实例才能发挥作用，模板才能被解析，我们使用的各种 Vue 的特性，api 才能起作用。`el` 可以是一个 DOM 元素或者是一个 css 选择器。
     - `data`：模板中使用的数据。我们在模板中使用的各种数据，就在 `data` 这个配置项中配置。在模板中使用的数据没有在 `data` 中定义，Vue 会在控制台输出警告。

4. 通过生成一个 Vue 的实例，数据和 DOM 建立了联系，而且所有东西都是响应式的。也就是我们修改了 `data` 中配置的数据，那么页面也会自动更新，将最新的数据渲染到页面上。

## 2. Vue2 中的指令


### 1. 指令的基本说明

1. 指令语法用来给 html 标签添加或者绑定属性，这个属性包括 html 标签原有的属性，或者是自定义属性。vue 中的指令均以 `v-` 开头。

2. 来自 Vue 官网对于指令的说明：
   > 指令 (Directives) 是带有 v- 前缀的特殊 attribute。指令 attribute 的值预期是单个 JavaScript 表达式 (v-for 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

3. 举个例子，一个 html 标签有 class 属性、id 属性，a 标签有 href 属性。众所周知，双大括号 `{{}}` 语法用于动态地设置标签内的文本。但是标签的属性不能使用双大括号 `{{}}` 语法动态设置值。因此，这里使用一个指令 - `v-bind` 来动态设置标签的属性。举例如下：  
   `<a v-bind:class="myClass" v-bind:href="url"></a>`

4. 一些指令能够接收一个参数，在指令名称之后以冒号表示。例如上面的例子。`v-bind` 用于响应式地更新 html 标签的属性。冒号 `:` 后面跟着的是具体的属性名称。等号 `=` 后面跟着的是一个字符串，字符串的值是我们要给这个属性绑定的变量。如上面的例子，给 `a` 标签的 `class` 属性绑定了 `myClass` 这个变量，给 `href` 这个属性绑定了 `url` 这个变量。当我们动态更新 `myClass`、 `url` 这两个变量的值的时候，`class` 和 `href` 的值也会同步更新。

### 2. v-bind 

1. `v-bind` 作用是动态地绑定一个或多个属性，或绑定一个组件的 prop 到表达式。

2. 在绑定 class 或 style 属性时，支持其它类型的值，如数组或对象。

3. 在绑定 prop 时，prop 必须在子组件中声明。可以用修饰符指定不同的绑定类型。

4. 没有参数时，可以绑定到一个包含键值对的对象。注意此时 class 和 style 绑定不支持数组和对象。

5. v-bind 可以缩写为 `:`，即 `v-bind:class="myClass"` 等同于 `:class="myClass"`

6. 基本示例：
   ```html
      <div class="root">
        <h1>插值语法</h1>
        <h1>hello, {{name}}</h1>
        <br>
        <h1>Vue 指令语法</h1>
        <a v-bind:href="school.url">{{school.name}}</a>
        <a :href="school.url">{{school.name}}</a>
      </div>
     <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
      <script>
          new Vue({
              el: '.root',
              data: {
                  name: 'world',
                  school: {
                      name: 'bupt',
                      url: '#'
                  }
              }
          })
      </script>
   ```
7. 绑定 class - 使用字符串、数组和对象：
   ```html
      <!--    绑定类名方式 1：使用字符串-->
      <div class="basic" :class="mood" @click="handleClick">{{name}}</div>
       <!--    绑定类名方式 2：使用数组-->
       <div class="basic" :class="classArr1">{{name}}</div>
       <div class="basic" :class="classArr2">{{name}}</div>
       <div class="basic" :class="classArr3">{{name}}</div>
       <!--绑定类名方式 3：使用对象-->
       <div class="basic" :class="classObj">{{name}}</div>
       <script>
             const vm = new  Vue({
               el: '#root',
               data: {
                   name: 'Apple',
                   mood: 'happy',
                   classArr1: ['item-1', 'item-2'],
                   classArr2: ['item-1', 'item-3'],
                   classArr3: ['item-1', 'item-2', 'item-3'],
                   // class 设置为对象，key 为 class，value 为布尔值，true 表示采用这个 class，false 表示不采用这个 class
                   classObj: {
                       'item-1': true,
                       'item-2': false,
                   },
               },
        
             });
       </script>
       
   ```

8. 绑定 style：
   ```html
    <!--    绑定样式，即使用 style 标签，设置其值为样式对象 -->
    <div class="basic" :style="styleObj">{{name}}</div>
    <script>
    
    const vm = new  Vue({
        el: '#root',
        data: {
            name: 'Apple',
            styleObj: {
                fontSize: '25px',
                background: '#666',
                borderRadius: '10px'
            }

        }
    });
   </script>
   ```

9. 给自定义组件绑定 props：
   ```vue
      <MyComponent 
           :name="name"
           :age="age"
      />
      <script>
           export default {
               data() {
                  return {
                      name: 'zs',
                      age: 20
                  }
               }
           }
      </script>
   ```
10. 总结：
    - 属性如果是动态变化的，或者说属性的值需要设置为一个变量，那么就使用 `v-bind` 进行绑定。
    - 属性如果是固定的，或者说就是一个字符串，那么就不需要使用 `v-bind`。
    - 不使用 `v-bind` 进行绑定，那么即使给属性赋值为变量，最终也会被解析为字符串，这点需要注意。

### 3. v-model

1. `v-model` 指令主要用于双向绑定表单元素。通过 `v-model` 指令，我们将表单元素的 `value` 属性和某个变量进行绑定，这样就能收集到输入的值，同时对变量的修改也能反映到表单元素上。这就是双向绑定。

2. 下面是对 `v-model` 指令的官方说明：
   > v-model 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

3. `v-model` 本质上是语法糖，即 Vue 替我们处理了表单输入元素时触发的事件，如 input、change 事件等。然后将 value 的值赋给对应的变量。同样，当变量的值发生变化，也会将变量的最新值赋给 value。

4. `v-model` 在内部为不同的表单元素使用不同的属性并抛出不同的事件：
   - text 和 textarea 元素使用 value 属性和 input 事件。
   - checkbox 和 radio 使用 checked 属性和 change 事件。
   - select 字段将 value 属性作为 prop 并将 change 作为事件。

5. `v-model` 的用法示例：
   ```html
      <div class="root">
       <!-- 从 Vue 实例中的 data 流向页面，也可以从页面流向实例中的 data，双向绑定只能用在表单类元素上，v-model 指令用于双向绑定，形式是：v-model:value="xxx" 简写形式是：v-model="xxx"-->
        双向数据绑定：<input v-model:value="name" type="text">
      </div>
      <script>
        new Vue({
          el: '.root',
          data: {
            value: 'bupt-1024',
            name: 'curry'
          }
        })
      </script>
   ```

6. `v-model` 也可以使用简写形式，即 `v-model:value="name"` 简写为：`v-model="name"`。

7.对于其他表单元素的双向绑定，如 `checkbox`、`radio`、`select`，请查看 Vue 官网，这里不再详述。地址：[表单输入绑定](https://cn.vuejs.org/v2/guide/forms.html)

### 4. v-on

1. v-on 指令用来给元素或者组件绑定事件。

2. 下面是对 v-on 指令的官方说明：
   > 绑定事件监听器。事件类型由参数指定。表达式可以是一个方法的名字或一个内联语句，如果没有修饰符也可以省略。

3. 对于普通的 DOM 元素，`v-on` 只能监听原生 DOM 事件。对于自定义组件，可以监听子组件触发的自定义事件。

4. `v-on` 的语法是 `v-on:eventName="handler"`，冒号后面跟着的是事件名称，等号后面跟着的是事件处理函数，事件处理函数默认接收一个 event 事件对象参数。

5. `v-on` 的基本使用：
   ```html
      <div id="root">
        <h1>Welcome to {{address}}</h1>
        <!--   v-on 指令用来绑定事件，如 click 事件：v-on:click="xxx"，change 事件：v-on:change="xxx"     -->
        <button v-on:click="showInfo">点我</button>
      </div>
      <script>
       const vm = new Vue({
        el: '#root',
        data: {
            address: '北京',
        },
        methods: {
            showInfo(event) {
                alert('clicked');
                console.log('event', event);
            }
        }
      })
   ```
   methods 配置项用来配置各种方法。一般来说，定义方式使用传统函数形式，不使用箭头函数，使用传统函数，内部的 this 指向的 Vue 的实例对象，这样能用到 Vue 实例的各种方法和属性。

6. v-on 也可以使用简写形式，如 `v-on:click="showInfo"` 等同于 `@click="showInfo"`。

7. 可以给事件处理函数传递参数，在绑定事件处理函数时，给函数传入参数，其中使用 `$event` 参数， `$event` 表示占位符，占位的是 event，也就是 `$event` 的位置会传入 event 对象，其他位置就是我们传递的参数。如下所示：
   ```html
       <button @click="showInfo3($event, 100)">传递参数</button>
       <script>
           const vm = new Vue({
               el: '#root',
               data: {
                   address: '北京',
               },
               methods: {
                   showInfo3(event, num) {
                       alert(`当前的数字是：${num}`);
                       console.log(event);
                   }
               }
           });
      </script>
   ```

#### 1. 事件修饰符

1. 在事件处理函数中，使用 `event.preventDefault()` 或者 `event.stopPropagation()` 是很常见的需求，但是我们希望事件处理方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节（来自 Vue 官网）。

2. 基于此 Vue 为我们提供了事件修饰符，就是以点开头的指令后缀组成，如阻止事件的默认行为：`.prevent`，阻止事件冒泡：`.stop`。
3. Vue 提供了下面几个的修饰符：
   - `.stop` 阻止事件冒泡
   - `.prevent` 阻止事件的默认行为
   - `.capture` 添加事件监听器时使用事件捕获模式
   - `.self` 只当在 event.target 是当前元素自身时触发处理函数，也就是事件绑定的元素和发生事件的元素是同一个
   - `.once` 只触发一次事件处理函数
   - `.passive`  对应 addEventListener 中的 passive 选项

4. 使用方法就是跟着事件后面，例如：`v-on:click.prevent="xxx"`，阻止点击事件的默认行为。`@click.stop="xxx"` 阻止事件的冒泡。

5. 示例：
   ```html
     <div id="root">
        <h1>Welcome to {{address}}</h1>
        <a href="https://www.baidu.com" @click.prevent="showInfo">跳转</a>
        <div class="submit">
        <!--    .once 修饰符，只触发一次事件处理函数    -->
             <button @click.once="handleSubmit">提交</button>
         </div>
         <!--.self 修饰符，只有触发事件的元素（e.target）和绑定事件的元素是同一个，才能执行事件处理函数-->
         <div class="wrapper" @click.self="handleClick">
             <div class="area"></div>
             <div class="area"></div>
         </div>
     </div>
      <script>
       const vm = new Vue({
           el: '#root',
           data: {
               address: '北京',
           },
        methods: {
            showInfo(e) {
                // 阻止事件的默认行为
                // e.preventDefault();
                alert('hello world');

            },
            handleClick(e) {
                alert('点击事件被触发了');
            },
            handleSubmit(e) {
                alert('只触发一次');
            }
        }
    });
      </script>
   ```

#### 2. 键盘修饰符

1. 对于键盘事件，我们需要在事件处理函数内部，去判断具体是哪个按键。

2. 为了简化我们的操作，Vue 提供了键盘事件的修饰符，常用键盘事件有：`keydown`、`keyup` 等。基于这些键盘事件提供了事件修饰符。

3. Vue 提供的键盘事件修饰符如下：
   - `.enter`   回车
   - `.tab`     tab 键
   - `.delete`  捕获删除键和退格键
   - `.esc`  esc 键
   - `.space`    空格
   - `.up`       上箭头
   - `.down`     下箭头
   - `.left`     左箭头
   - `.right`    右箭头

4. 如只有按下回车键才能触发：`@keydown.enter="xxx"`。

5. 也可以使用 `keyCode` 作为修饰符，如回车的 `keyCode` 是 `13`，按下回车键才能触发：`@keydown.13="xxx"`。

6. **注意**：`keyCode` 是被废弃的属性，不建议在生产环境中使用，建议使用按键别名，如 `enter`、`tab`、`space` 等。

7. 自定义键名：`Vue.config.keyCodes.自定义键名 = 键码`，可以去定制按键别名。

8. 系统修饰键（用法特殊）：`ctrl`、`alt`、`shift`、`meta`，单独按下，不会触发事件。需要配合其他按键一起使用。用法如下
   - 配合 `keyup` 使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发。
   - 配合 `keydown` 使用：正常触发事件。

9. `div` 等⾮输⼊性质的元素（与其对应的可输⼊性元素有 `input`, `textarea`），是不可被聚焦的。所以⽆法监听其的键盘事件。

10. 表单类的元素可以绑定键盘事件。

11. 示例：
    ```html
       <div class="key-operation">
          监听键盘
          <input type="text" @keydown.enter="handleSubmit">
       </div>
       <script>
             const vm = new Vue({
                 el: '#root',
                 data: {
                     address: '北京',
                 },
                 methods: {
                     handleSubmit(e) {
                         // 阻止事件的默认行为
                         alert(`${e.code}`);
                         console.log('e', e);
                     }
                 }
             });
       </script>
    ```
### 5. v-if / v-else / v-else-if

1. `v-if` 这个指令的作用是条件渲染。v-if 后面跟着一个表达式，表达式为 true 的时候渲染，表达式为 false 的时候不渲染这个元素，且 dom 结构中没有这个元素。

2. `v-if` 的示例用法：
   ```html
      <div id="root">
         <h2 v-if="show">{{name}}</h2>
         <button @click="changeType">切换</button>
         
      </div>
      <script>
          const vm = new Vue({
              el: '#root',
              data: {
                  show: true,
                  n: 1,
                  name: 'Apple'
              },
              methods: {
                  changeType() {
                      this.show = !this.show;
                  },
              }
          })
      </script>
   ```

3. `v-if` 还可以和 `v-else`、`v-else-if` 连用，连用时，中间不能掺杂其他内容，即不能够被打断。`v-else` 后面不跟任何表达式。`v-else-if` 后面跟着表达式。v-if、v-else-if、v-else 连一起的用法类似于 if 、else if、else 的用法。v-if、v-else-if、v-else 连用时，中间不能掺杂其他内容，即不能够被打断，如下所示：
   - 正确用法：
     ```html
        <h2 v-if="n === 1">Apple</h2>
        <h2 v-else-if="n === 2">Amazon</h2>
        <h2 v-else-if="n === 3">Tiktok</h2>
        <h2 v-else>Facebook</h2>
     ```
   - 错误用法 - 中间多了其他结构：
     ```html
        <h2 v-if="n === 1">Apple</h2>
        <h2 v-else-if="n === 2">Amazon</h2>
        <h2>aaaaa</h2>
        <h2 v-else-if="n === 3">Tiktok</h2>
        <h2 v-else>Facebook</h2>
     ```
   
### 6. v-show

1. `v-show` 这个指令也可以用于条件渲染。`v-show` 后面跟着一个表达式，表达式为 true 的展示元素，为 false 的时候不展示元素，不展示元素的时候，dom结构中存在，只不过其样式中的 `display` 属性为 `none`。

### 7. v-for

1. `v-for` 指令的作用是用于展示列表数据。即列表数据渲染为 dom 结构。

2. 语法：`v-for="(item, index) in list" :key="index"`。list 是待循环的列表数据，item 是列表元素，index 是索引，key 使用 v-for 指令进行列表循环渲染时给元素增加的一个属性。主要是新旧虚拟 dom 进行 diff 时起作用。一般情况下，key 取一个唯一值。

3. 可遍历数据结构有：
   - 数组（用的最多）
   - 对象
   - 字符串（用的很少）
   - 指定次数（用的很少）

4. 示例：
   ```html
      <div id="root">
             <h2>人员列表</h2>
             <ul>
                <li v-for="p in person" :key="p.id">
                    <span>id: {{p.id}}</span>
                    <span>name: {{p.name}}</span>
                    <span>age: {{p.age}}</span>
                </li>
             </ul>
      </div>
      <script>
           const vm = new Vue({
               el: '#root',
               data: {
                   person: [
                       {id: '001', name: 'apple', age: 18},
                       {id: '002', name: 'banana', age: 20},
                       {id: '003', name: 'orange', age: 25},
                       {id: '004', name: 'grape', age: 26},
                   ]
               }
           })
      </script>
   ```
   在 `v-for` 块中，我们可以访问所有父作用域的属性，也就是 li 包裹的区域内部可以访问 p 的属性。

5. v-for 还支持一个可选的第二个参数，即当前项的索引 - index，示例如下：
   ```html
      <li v-for="(p, index) in person" :key="p.id">
                <span>index: {{index}}</span>
                <span>id: {{p.id}}</span>
                <span>name: {{p.name}}</span>
                <span>age: {{p.age}}</span>
      </li>
   ```

6. `v-for` 也可以用来变量对象，语法是：`v-for="(value, key, index) in object"`。`value` 是 `obj` 对象的属性的值，`key` 是 `obj` 对象的属性的名，`index` 是索引。示例如下：
   ```html
      <div id="root">
          <h2>遍历对象</h2>
           <ul>
               <li v-for="(value, key, index) in company" :key="value">
                   {{index}} - {{key}} : {{value}}
               </li>
           </ul>
      </div>
      <script>
          const vm = new Vue({
              el: '#root',
              data: {
                  company: {
                      name: 'MicroSoft',
                      location: 'Beijing',
                      country: 'USA'
                  }
              }
          })
     </script>
   ```
   注意：遍历对象用的比较少。

### 8. 自定义指令

1. 详见 Vue 的官方文档：[自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)

2. 主要使用挂在 Vue 构造函数上的函数 directive 函数来实现自定义指令。

3. directive 函数的详解：[Vue.directive](https://cn.vuejs.org/v2/api/#Vue-directive)

### 9. 指令总结

1. 常用指令总结：

   指令|                                       基本用法                                       |          简写           |简短说明
   :---:|:--------------------------------------------------------------------------------:|:---------------------:|:---:
   v-bind|                                v-bind:href="url"                                 | `<a :href="url"></a>` |绑定属性，props 
   v-model|                               v-model:value="age"                                |     v-model="age"     |表单元素双向绑定
   v-on|                             v-on:click="handleClick"                             | @click="handleClick"  |绑定原生事件或者自定义事件
   v-if|                        `<h1 v-if="show">hello world</h1>`                        |                       |条件渲染，条件为 true 的时候渲染，为 false 的时候不渲染
   v-else|            `<h1 v-if="show">hello world</h1>`  `<h1 v-else>你好世界</h1>`            |                       |条件渲染，不单独使用，与 v-if 和 v-else-if 一起使用，条件不满足 v-if 和 v-else-if 时渲染
   v-else-if| `<h1 v-if="show === 0">hello world</h1>`  `<h1 v-else-if="show === 1">你好世界</h1>` |                       | 条件渲染，不单独使用，与 v-if 一起使用
   v-show|                     `<h1 v-show="display">hello world</h1>`                      |                       | 条件渲染，条件为 true 的时候渲染，为 false 的时候不渲染，不渲染的情况下，dom 元素存在，只不过其样式的 display 属性为 none。
   v-for|  `<li v-for="(p, index) in person" :key="p.id"><span>id: {{p.id}}</span></li>`   |                       | 列表渲染，可以遍历数组，对象，字符串，还可以指定遍历次数。最常用的是数组。


## 3. Vue2 中的配置项：计算属性 - computed

## 4. Vue2 中的配置项：监视属性 - watched

## 5. Vue2 中的配置项：过滤器 - filters

## 6. Vue2 中的生命周期

## 6. 单文件组件

## 7. 单文件组件中的 ref 与 props

## 8. 混入 - mixins

## 9. 插件 - plugin

## 10. 插槽 - slot

## 11. Vue2 中的自定义事件
