#列表渲染
我们可以用 v-for 指令基于一个数组来渲染一个列表。v-for 指令需要使用 item in items 形式的特殊语法，其中 items 是源数据数组，而 item 则是被迭代的数组元素的别名。该指令可以访问所有父作用域的属性 还支持一个可选的第二个参数，即当前项的索引。你也可以用 of 替代 in 作为分隔符，因为它更接近 JavaScript 迭代器的语法：

```html
<div v-for="item of items"></div>
v-for 也可以遍历对象 
<div v-for="(value, name) in object">
  {{ name }}: {{ value }}
</div>
key 并不仅与 v-for 特别关联 取值为字符串或数值类型的值
```

数组更新检测 （变异方法）
Vue 将被侦听的数组的变异方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：
push()、pop()、shift()、unshift()、splice()、sort()、reverse()

变异方法，顾名思义，会改变调用了这些方法的原始数组。相比之下，也有非变异 (non-mutating method) 方法，例如 filter()、concat() 和 slice() 。它们不会改变原始数组，而总是返回一个新数组。当使用非变异方法时，可以用新数组替换旧数组：用一个含有相同元素的数组去替换原来的数组是非常高效的操作。
```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

##注意项
由于 JavaScript 的限制，Vue 不能检测以下数组的变动：
当你利用索引直接设置一个数组项时，例如：vm.items[indexOfItem] = newValue
当你修改数组的长度时，例如：vm.items.length = newLength
给数组中的对象动态添加属性 不能直接用点语法来添加 （如果这么做的话 视图不能够及时更新！）
（也可以把数组变成）
```js
全局方法：Vue.set(vm.items,indexOfItem,newValue);
实例方法：vm.$set(vm.items, indexOfItem, newValue); // 你也可以使用 vm.$set 实例方法，该方法是全局方法 Vue.set 的一个别名：
```

##显示过滤/排序后的结果
有时，我们想要显示一个数组经过过滤或排序后的版本，而不实际改变或重置原始数据。在这种情况下，可以创建一个计算属性(或者方法)，来返回过滤或排序后的数组。


#在 <template> 上使用 v-for
```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

##vue中DOM更新是异步的 
将数据修改之后，DOM并没有即时更新，而是等到vue下一次的渲染工作执行的时候，才会更新DOM 什么时候进行下一次渲染？比如页面数据发生改变的时候
```js
this.$nextTick(() => { 
    // 当dom更新完毕后 vue就会自动调用这个函数中的内容 改变视图中的msg值
    // 能够确保在这个函数内部 访问到的DOM对象肯定是最新的！
    this.msg2 = this.$refs.box.innerHTML;
})

```
nextTick 和 $nextTick都是用来获取最新的DOM的时候需要使用的！