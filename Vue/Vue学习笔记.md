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
