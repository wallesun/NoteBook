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
