# 非 Prop 的 Attribute
一个非 prop 的 attribute 是指传向一个组件，但是该组件并没有相应 props 或 emits 定义的 attribute。常见的示例包括 class、style 和 id attribute。可以通过 $attrs property 访问那些 attribute。


## Attribute 继承
当组件返回单个根节点时，非 prop 的 attribute 将自动添加到根节点的 attribute 中。例如，在 date-picker 组件的实例中：
```js
app.component('date-picker', {
  template: `
    <div class="date-picker">
      <input type="datetime-local" />
    </div>
  `
})
```

如果我们需要通过 data-status attribute 定义 <date-picker> 组件的状态，它将应用于根节点 (即 div.date-picker)。

<!-- 具有非 prop 的 attribute 的 date-picker 组件-->
<date-picker data-status="activated"></date-picker>

<!-- 渲染后的 date-picker 组件 -->
<div class="date-picker" data-status="activated">
  <input type="datetime-local" />
</div>

## 禁用 Attribute 继承
如果你不希望组件的根元素继承 attribute，可以在组件的选项中设置 inheritAttrs: false。

禁用 attribute 继承的常见场景是需要将 attribute 应用于根节点之外的其他元素。

通过将 inheritAttrs 选项设置为 false，你可以使用组件的 $attrs property 将 attribute 应用到其它元素上，该 property 包括组件 props 和 emits property 中未包含的所有属性 (例如，class、style、v-on 监听器等)。

如果需要将所有非 prop 的 attribute 应用于 input 元素而不是根 div 元素，可以使用 v-bind 缩写来完成。

```js
app.component('date-picker', {
  inheritAttrs: false,
  template: `
    <div class="date-picker">
      <input type="datetime-local" v-bind="$attrs" />
    </div>
  `
})
```

有了这个新配置，data-status attribute 将应用于 input 元素！

<!-- date-picker 组件使用非 prop 的 attribute -->
<date-picker data-status="activated"></date-picker>

<!-- 渲染后的 date-picker 组件 -->
<div class="date-picker">
  <input type="datetime-local" data-status="activated" />
</div>

## 多个根节点上的 Attribute 继承
与单个根节点组件不同，具有多个根节点的组件不具有自动 attribute fallthrough (隐式贯穿) 行为。如果未显式绑定 $attrs，将发出运行时警告。

<custom-layout id="custom-layout" @click="changeValue"></custom-layout>

```js
// 这将发出警告
app.component('custom-layout', {
  template: `
    <header>...</header>
    <main>...</main>
    <footer>...</footer>
  `
})

// 没有警告，$attrs 被传递到 <main> 元素
app.component('custom-layout', {
  template: `
    <header>...</header>
    <main v-bind="$attrs">...</main>
    <footer>...</footer>
  `
})
```
