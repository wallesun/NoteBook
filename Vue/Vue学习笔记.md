## 1、列表渲染（v-for）
### v-for
* v-for可以循环数组和json对象，语法格式如下
```
<div v-for="item in items" :key="item.id" >{{item}}</div>// key是必须的，为了让Vue区别每一条列表数据
// 如果是循环json对象
<div v-for="(item, key, index) in items" >{{key}}--{{item}}</div> // 这里item是value值，而key则是对应的key，index是索引
```
### 变异方法
* Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：

* push()
* pop()
* shift()
* unshift()
* splice()
* sort()
* reverse()
* 以上是Vue数组中的变异方法，也就是会改变原数组的方法，以下是不会改变原数组的方法（替换数组）
  * filter(), concat() 和 slice()；这些方法不会改变原数组，但是会返回一个新数组
### 注意事项
* 因为JavaScript的限制，Vue不能检测下边这种变动的数组
 * 当直接利用索引改变数组的时候，``` Array[index] = string;```
 * 直接改变数组的长度 ``` Array.length = number```
 ```
 var vm = new Vue({
  data: {
   items: ['a','b','c']
  }
 })
 vm.items[1] = 'd'; // 不是响应性的
 ```
* 针对第一种情况，可以采用下边方式解决，可以触发状态更新
```Vue.set(items, index, newValue);```
``` vm.items.splice(index, 1, newValue);```
* 同样对于json对象也存在这种情况，解决办法：
``` Vue.set(object, key, value )```
* v-for同样可以用在组件中，但是不会直接将数据传递给组件，因为组件有独立作用域，需要通过props来通信
## 2、事件处理
### v-on
* v-on指令用于绑定事件，像v-bind一样也可以缩写，可缩写成@
### 事件修饰符
* ``` event.preventDefault()``` ``` event.stopPropagation()``` 这种原生的事件是很常用的，但是Vue是以数据驱动的，不应该操作dom，所以这些功能
可以借助事件修饰符来
* .stop   .prevent    .capture    .self    .once    .passive
```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```
* 事件修饰符是支持链式操作的，但是先后顺序会影响最终的操作
* 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。
### 按键修饰符
* 针对键盘事件，Vue提供了按键修饰符，只有触发对应按键事件的时候，才会执行该方法
* 全部的按键别名： .enter   .tab   .delete (捕获“删除”和“退格”键)   .esc   .space   .up   .down   .left   .right
* 可以通过全局 config.keyCodes 对象自定义按键修饰符别名
``` 
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```
### 鼠标按键修饰符
* .right    .left    .middle
## 3、表单输入绑定
### 基本用法
* 可以使用v-model在input， textarea， select上实现双向数据绑定
* v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。
### 复选框
* 单个复选框直接使用checked布尔值
```
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```
* 多个复选框可以绑定到同一个数组上
```
<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>
```
* 如果v-model绑定的是字符串则其绑定的值都是value的值
### 单选框
* 单选框绑定时候，如果v-model绑定的是字符串，则其值也是value的值，也可以绑定布尔值
```
<div id="example-4">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
```
### 选择框
* 单选时
```
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option> // 这里添加一项，可以防止ios出问题
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```
* 多选时（绑定到一个数组）
```
<div id="example-6">
  <select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
```
### 值绑定（不同于字符串绑定）
* 复选框
```
<input v-model="toggle" type="checkbox" true-value="yes" false-value="no"/>
```
### 修饰符
* .lazy
* 在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步：
* .number   如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符：这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。
* .trim   如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符
## 4、 组件基础
### 基本知识
* 因为组件是可复用的 Vue 实例，所以它们与 new Vue 接收相同的选项，例如 data、computed、watch、methods 以及生命周期钩子等。仅有的例外是像 el 这样根实例特有的选项。
* data不能像Vue实例中那样直接用对象的方式表示，而必须是函数形式
```
Vue.component('myComponent', {
 data: function() {
  return {
   name: 'sunzheng'
  }
 }
})
```
### 组件名
* 驼峰和短横线分隔命名（kebab-case）；最好还是用kebab-case命名法
### 全局注册和局部注册
```
Vue.component('my-component-name', {
  // ... 选项 ...
})
```
* 全局注册的组件，可以应用在任意Vue根实例（new Vue）中，同时在子组件中也可以相互使用
* 局部注册
```
new Vue({
  el: '#app'
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```
### 在模块系统中注册组件
```
import ComponentA from './ComponentA'
import ComponentC from './ComponentC'

export default {
  components: { // es6语法
    ComponentA,
    ComponentC
  },
  // ...
}
```
### 基础组件的自动全局化注册
* 可以在入口文件像下边这样全局注册，就不用每次使用这些基础组件都要引入了
```
import Vue from 'vue'
import upperFirst from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'

const requireComponent = require.context(
  // 其组件目录的相对路径
  './components',
  // 是否查询其子目录
  false,
  // 匹配基础组件文件名的正则表达式
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // 获取组件配置
  const componentConfig = requireComponent(fileName)

  // 获取组件的 PascalCase 命名
  const componentName = upperFirst(
    camelCase(
      // 剥去文件名开头的 `./` 和结尾的扩展名
      fileName.replace(/^\.\/(.*)\.\w+$/, '$1')
    )
  )

  // 全局注册组件
  Vue.component(
    componentName,
    // 如果这个组件选项是通过 `export default` 导出的，
    // 那么就会优先使用 `.default`，
    // 否则回退到使用模块的根。
    componentConfig.default || componentConfig
  )
})
```
### 通过prop向子组件传递数据
* 通过v-bind的形式向组件专递数据，同时在组件中声名props
```
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```
* 单个根节点
* 注册组件时候，模板内只能有一个根节点，否则报错
```
<div class="blog-post"> // 所有内容都包含在这个父元素中
  <h3>{{ title }}</h3>
  <div v-html="content"></div>
</div>
```
### 通过事件向父组件发送消息
* $emit 方法可以触发一个事件，然后向父组件传递数据
```
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```
* 在内部用$emit触发事件“enlarge-text”，这时候组件中就可以用v-on监测“enlarge-text”这个方法，并触发其对应的方法
```
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```
* 还可以用$emit抛出一个子组件内的数据，```$emit('enlarge-text', data)```这里边data就是抛出的数据，如果enlarge-text绑定的是一个方法，则data会作为第一个参数传入这个方法（函数）
### 通过插槽分发内容
* 当想往组件内放置内容的时候，可以通过slot来解决
```
<alert-box>
  Something bad happened.
</alert-box>

Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
```
* 最终显示出来 Error! Something bad happend.  也就是说，想插入的内容会出现在<slot></slot>这个元素内，位置也与slot元素位置对应
### 动态组件
* 可以使用 is 特性来实现动态组件
```
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component v-bind:is="currentTabComponent"></component>
```
### 解析Dom模板时的注意事项
* 有些 HTML 元素，诸如 ```<ul>、<ol>、<table> 和 <select>```，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 ```<li>、<tr> 和 <option>```，只能出现在其它某些特定的元素内部。
 * 此时可以使用is特性来避免这种情况
 ```
 <table>
  <tr is="blog-post-row"></tr>
</table>
 ```
 * 将组件信息用is来表示，这样就不会违反W3C规则
### Prop
* Prop的大小写———— 如果是使用驼峰命名方式，在html中表示时必须用短横线连接
```
Vue.component('blog-post', {
  // 在 JavaScript 中是 camelCase 的
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})

<!-- 在 HTML 中是 kebab-case 的 -->
<blog-post post-title="hello!"></blog-post>
```
* 注意在 JavaScript 中对象和数组是通过引用传入的，所以对于一个数组或对象类型的 prop 来说，在子组件中改变这个对象或数组本身将会影响到父组件的状态。
* 注意那些 prop 会在一个组件实例创建之前进行验证，所以实例的属性 (如 data、computed 等) 在 default 或 validator 函数中是不可用的。
### 禁用特性继承
* 如果你不希望组件的根元素继承特性，你可以设置在组件的选项中设置 inheritAttrs: false。例如：
```
Vue.component('my-component', {
  inheritAttrs: false,
  // ...
})
```
* 这里的$attr就像原型一样，当inheritAttrs设置为false时，则props中未注册的特性，就会存在$attr中，之后所有的后代组件都可以通过$attr取到这个值（无论嵌套多少层组件）



