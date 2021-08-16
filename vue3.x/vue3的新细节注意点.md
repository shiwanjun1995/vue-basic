## 列表渲染
```json
1、在 v-for 块中，我们可以访问所有父作用域的 property。
2、在自定义组件上，你可以像在任何普通元素上一样使用 v-for：
  <my-component v-for="item in items" :key="item.id"></my-component>
  然而，任何数据都不会被自动传递到组件里，因为组件有自己独立的作用域。为了把迭代数据传递到组件里，我们要使用 props：
  <my-component
    v-for="(item, index) in items"
    :item="item"
    :index="index"
    :key="item.id"
  ></my-component>
  不自动将 item 注入到组件里的原因是，这会使得组件与 v-for 的运作紧密耦合。明确组件数据的来源能够使组件在其他场合重复使用。
3、props:['someProps'], emits:['someEmits'](这样的话html页面可以直接使用 emits 里的方法了，如果需要在methods里面使用，还可以引入 .vue 单文件里的 setup(props, {emit}) {  })
```

## 事件处理
```json
1、事件处理程序中可以有多个方法，这些方法由逗号运算符分隔：
  <!-- 这两个 one() 和 two() 将执行按钮点击事件 -->
  <button @click="one($event), two($event)">
    Submit
  </button>
```

## 表单输入绑定
```json
1、你可以用 v-model 指令在表单 <input>、<textarea> 及 <select> 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件来更新数据，并在某种极端场景下进行一些特殊处理。
2、v-model 会忽略所有表单元素的 value、checked、selected attribute 的初始值而总是将当前活动实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。
3、v-model 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：
text 和 textarea 元素使用 value property 和 input 事件；
checkbox 和 radio 使用 checked property 和 change 事件；
select 字段将 value 作为 prop 并将 change 作为事件。
```

