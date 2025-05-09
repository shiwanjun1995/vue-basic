# 组件注册大小写问题？

- 推荐使用短横线 每个短横线的首字母是一个逻辑断点 kebab-case [涉及到一个模板解析的过程在 html 模板中使用]
- Vue 的组件里面也可以有 data、template、methods、watch 等属性，需要注意的是 component 中的 data 必须为一个函数 每个组件有自己独立的数据 对象拷贝问题
- 组件注册分为全局注册、局部注册 全局注册意味着即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中。这造成了用户下载的 JavaScript 的无谓的增加。

# 组件名大小写

## 在字符串模板或单文件组件中定义组件时，定义组件名的方式有两种：

- kebab-case 短横线连接起来的方式（当使用 kebab-case (短横线分隔命名) 定义一个组件时，你在引用这个自定义元素时也必须使用 kebab-case，例如 <my-component-name>。）
- PascalCase 首字母大写 （当使用 PascalCase (首字母大写命名) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说 <my-component-name> 和 <MyComponentName> 都是可接受的。）

## 直接在 DOM (即非字符串的模板) 中使用时：

- 只有 kebab-case 是有效的。

# 在模块系统中引入组件

注意在 ES2015+ 中，在对象中放一个类似 ComponentA 的变量名其实是 ComponentA: ComponentA 的缩写，即这个变量名同时是：

- 用在模板中的自定义元素的名称
- 包含了这个组件选项的变量名

# Prop 的大小写 (camelCase vs kebab-case)

- 在 JavaScript 中是 camelCase 的
- 在 HTML 中是 kebab-case 的
  解释：HTML 中的特性名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。

# Prop 类型 ([] 和 {})

- 以字符串数组形式列出的 prop：

```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

- 希望每个 prop 都有指定的值类型 以对象形式列出 prop，这些属性的名称和值分别是 prop 各自的名称和类型：

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

# 传递静态或动态 Prop

简单实例中我们传入的值都是字符串类型的，但实际上任何类型的值都可以传给一个 prop。
可传入一个数字 v-bind:绑定、可传入布尔值、数组、对象、对象的所有属性 [都是通过 v-bind:来传递来告诉 vue 这是一个 js 表达式]

## 传入一个布尔值

<!-- 包含该 prop 没有值的情况在内，都意味着 `true`。          -->
<!-- 如果没有在 props 中把 is-published 的类型设置为 Boolean，
则这里的值为空字符串，而不是“true”。 -->

<blog-post is-published></blog-post>

此处的 prop (is-published) 为 true，前提是在自组件内部设定该 porp 为布尔值.比如经常在按钮组件设置 disabled 即可，<my-button disabled></my-button>，不需要设置 <my-button :disabled="true"></my-button>

# Prop 验证

我们可以为组件的 prop 指定验证要求，例如你知道的这些类型。如果有一个需求没有被满足，则 Vue 会在浏览器控制台中警告你。这在开发一个会被别人用到的组件时尤其有帮助。
为了定制 prop 的验证方式，你可以为 props 中的值提供一个带有验证需求的对象，而不是一个字符串数组。例如：
当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。
注意那些 prop 会在一个组件实例创建之前进行验证，所以实例的属性 (如 data、computed 等) 在 default 或 validator 函数中是不可用的。

补充一些细节：Boolean 类型的未传递 prop 将被转换为 false。这可以通过为它设置 default 来更改——例如：设置为 default: undefined 将与非布尔类型的 prop 的行为保持一致。

```js
defineProps({
  disabled: Boolean
})

<!-- 等同于传入 :disabled="true" -->
<MyComponent disabled />

<!-- 等同于传入 :disabled="false" -->
<MyComponent />
```

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
  `,
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

对于绝大多数特性来说，从外部提供给组件的值会替换掉组件内部设置好的值。所以如果传入 type="text" 就会替换掉 type="date" 并把它破坏！庆幸的是，class 和 style 特性会稍微智能一些，即两边的值会被合并起来。

## 总结

1.prop 数据单项传递，父影响子，子不影响父 2.不能在组件中直接修改 prop 传递过来的值，Vue 会给出警告
3.prop 验证时，会在实例创建之前进行验证，所以实例的属性 (如 data、computed 等) 在 4.default 或 validator 函数中是不可用的 5.非 prop 特性，组件可以接受任意的特性，而这些特性会被添加到这个组件的根元素上。

参考链接：1.https://www.jianshu.com/p/ca88cdcea7b4 2.https://www.jianshu.com/p/34d3537d1993 3.https://www.jianshu.com/p/5c9062534d9b 4.https://www.jianshu.com/p/5c9062534d9b
