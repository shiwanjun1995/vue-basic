# Provide / Inject

通常，当我们需要从父组件向子组件传递数据时，我们使用 props。想象一下这样的结构：有一些深度嵌套的组件，而深层的子组件只需要父组件的部分内容。在这种情况下，如果仍然将 prop 沿着组件链逐级传递下去，可能会很麻烦。

对于这种情况，我们可以使用一对 provide 和 inject。无论组件层次结构有多深，父组件都可以作为其所有子组件的依赖提供者。这个特性有两个部分：父组件有一个 provide 选项来提供数据，子组件有一个 inject 选项来开始使用这些数据。

![components_provide.png](https://v3.cn.vuejs.org/images/components_provide.png)

但是，如果我们尝试在此处 provide 一些组件的实例 property，这将是不起作用的：

要访问组件实例 property，我们需要将 provide 转换为返回对象的函数：

这使我们能够更安全地继续开发该组件，而不必担心可能会更改/删除子组件所依赖的某些内容。这些组件之间的接口仍然是明确定义的，就像 prop 一样。

实际上，你可以将依赖注入看作是“长距离的 prop”，除了：

父组件不需要知道哪些子组件使用了它 provide 的 property
子组件不需要知道 inject 的 property 来自哪里

## 处理响应性

在上面的例子中，如果我们更改了 todos 的列表，这个变化并不会反映在 inject 的 todoLength property 中。这是因为默认情况下，provide/inject 绑定并不是响应式的。我们可以通过传递一个 ref property 或 reactive 对象给 provide 来改变这种行为。在我们的例子中，如果我们想对祖先组件中的更改做出响应，我们需要为 provide 的 todoLength 分配一个组合式 API computed property：

```js
app.component('todo-list', {
  // ...
  provide() {
    return {
      todoLength: Vue.computed(() => this.todos.length)
    }
  }
})

app.component('todo-list-statistics', {
  inject: ['todoLength'],
  created() {
    console.log(`Injected property: ${this.todoLength.value}`) // > 注入的 property: 5
  }
})
```

在这种情况下，任何对 todos.length 的改变都会被正确地反映在注入 todoLength 的组件中。
