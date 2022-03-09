# 基础用法
```html
你可以用 v-model 指令在表单 <input>、<textarea> 及 <select> 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

注意：v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。
```
v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：
* text 和 textarea 元素使用 value property 和 input 事件；
* checkbox 和 radio 使用 checked property 和 change 事件；
* select 字段将 value 作为 prop 并将 change 作为事件。
  
注意：在文本区域插值 (<textarea>{{text}}</textarea>) 并不会生效，请使用 v-model 来代替。

# 值绑定
```html
对于单选按钮，复选框及选择框的选项，v-model 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a" />

<!-- `toggle` 为 true 或 false -->
<input type="checkbox" v-model="toggle" />

<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>

但是有时我们可能想把值绑定到当前活动实例的一个动态 property 上，这时可以用 v-bind 实现，此外，使用 v-bind 可以将输入值绑定到非字符串。
<input type="radio" v-model="pick" v-bind:value="a" />

// 当选中时
vm.pick === vm.a
```

# 修饰符
在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 
```html
<!-- 在“change”时而非“input”时更新-->
<input v-model.lazy="msg" >
不加.lazy这个修饰符的时候 在文本框输入内容的时候就会自动更新视图中的数据
而加了.lazy这个修饰符 文本框输入内容的时候不会自动更新 而只有在change事件触发的时候才会更新视图中的数据

如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符：
如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符：
```

# 在组件上也可以使用v-model
HTML 原生的输入元素类型并不总能满足需求。幸好，Vue 的组件系统允许你创建具有完全自定义行为且可复用的输入组件。这些输入组件甚至可以和 v-model 一起使用！
