## 在组件上使用
```html
当你在带有单个根元素的自定义组件上使用 class attribute 时，这些 class 将被添加到该元素中。此元素上的现有 class 将不会被覆盖。。
```

```js
例如，如果你声明了这个组件：
const app = Vue.createApp({})

app.component('my-component', {
  template: `<p class="foo bar">Hi!</p>`
})

然后在使用它的时候添加一些 class：
<div id="app">
  <my-component class="baz boo"></my-component>
</div>

HTML 将被渲染为：
<p class="foo bar baz boo">Hi</p>
```

```html
如果你的组件有多个根元素，你需要定义哪些部分将接收这个 class。可以使用 $attrs 组件 property 执行此操作：
```

```js
<div id="app">
  <my-component class="baz"></my-component>
</div>

const app = Vue.createApp({})

app.component('my-component', {
  template: `
    <p :class="$attrs.class">Hi!</p>
    <span>This is a child component</span>
  `
})
```