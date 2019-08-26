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