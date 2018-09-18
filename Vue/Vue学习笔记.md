# 1、列表渲染（v-for）
## v-for
* v-for可以循环数组和json对象，语法格式如下
```
<div v-for="item in items" :key="item.id" >{{item}}</div>// key是必须的，为了让Vue区别每一条列表数据
// 如果是循环json对象
<div v-for="(item, key, index) in items" >{{key}}--{{item}}</div> // 这里item是value值，而key则是对应的key，index是索引
```
## 变异方法
* Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：

* push()
* pop()
* shift()
* unshift()
* splice()
* sort()
* reverse()
* 以上是Vue数组中的变异方法，也就是会改变原数组的方法，以下是不会改变原数组的方法
  * filter(), concat() 和 slice()；这些方法不会改变原数组，但是会返回一个新数组
  
