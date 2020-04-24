## 全局API
在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。
```js
this.$nextTick(() => {
  // 可以拿到异步更新的DOM
})
```

往响应式对象中添加一个属性 并确保这个属性同样也是响应式的 且触发视图的更新 它必须用于向响应式对象上添加新属性 因为vue是无法探测到普通的新增属性(比如用对象点语法添加的新属性)
```js
target: 目标元素 
propertyName/index: 属性名或者index值
value: 要添加的响应式的值
{Object | Array} target
{string | number} propertyName/index
{any} value
this.$set(target, propertyName/index, value)
```

删除对象的属性。如果对象是响应式的，确保删除能触发更新视图。这个方法主要用于避开 Vue 不能检测到属性被删除的限制，但是你应该很少会使用它。
```js
this.$delete(target, propertyName/index)
```

注册或获取全局指令 用于操作dom
Vue.directive('指令名',{钩子函数})

注册或获取全局指令 用于过滤数据
定义本地的过滤器 用管道符号隔开
filters: {}

安装插件 如果插件是一个对象，必须提供 install 方法。如果插件是一个函数，它会被作为 install 方法。install 方法调用时，会将 Vue 作为参数传入。该方法需要在调用 new Vue() 之前被调用。
```js
插件的内部需要有一个暴露到外部的install方法
import utils from './plugin/utils.js';
Vue.use(utils);
```

全局注册一个混入 会影响注册之后所有创建的每个vue实例 插件作者可以使用混入 向组件注入自定义的行为
```js
{Object} mixin
Vue.mixin(mixin)
```

## 选项/数据
data 类型：Object | Function
组件的定义只接受function
介绍：vue实例的数据对象 会递归遍历data里的属性转换为getter和setter 从而让data里的所有属性变成响应式的数据 vm.$data 返回的是一个原始数据对象 vm.$data.a = vm.a

props Array<string> | Object
可以是数组或者对象 用来接收自来自父组件的数据 简单的数组或者对象（对象允许配置高级选项 如类型检测、自定义验证和设置默认值）

computed { [key: string]: Function | { get: Function, set: Function } }
计算属性将被混入到vue实例中 所有的getter和setter的this上下文都将绑定到vue的实例

methods { [key: string]: Function }
将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。[不应该使用this箭头函数来使用methods因为箭头函数绑定了父级作用域的上下文所以this将不会按照期望地指向vue实例]

watch { [key: string]: string | Function | Object | Array }
一个对象 键是需要观察的表达式 值是对应的回调函数 值也可以是方法名 或者包含选项的对象 vue实例将会在实例化时调用 $watch() 遍历watch对象的每一个属性[不应该使用this箭头函数来使用methods因为箭头函数绑定了父级作用域的上下文所以this将不会按照期望地指向vue实例]

template 一个字符串模板作为vue实例的标识使用 模板将会替换挂载的元素 挂载元素的内容都将被忽略 除非模板的内容有分发插槽

## 选项/生命周期钩子
所有的生命周期钩子自动绑定 this 上下文到实例中，因此你可以访问数据，对属性和方法进行运算。这意味着你不能使用箭头函数来定义一个生命周期方法 (例如 created: () => this.fetchTodos())。这是因为箭头函数绑定了父上下文，因此 this 与你期待的 Vue 实例不同，this.fetchTodos 的行为未定义。

created 在实例创建完成后背立即调用 在这一步 实例已经完成以下的配置：数据观测（data observer） 属性和方法的运算 watch/event 事件回调 然而 挂载阶段还没开始

mounted 实例被挂载后调用 提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。
在实例挂载之后，元素可以用 vm.$el 访问。
这时 el 被新创建的 vm.$el 替换了
注意 mounted 不会保证所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以在 mounted 内部使用 vm.$nextTick：

## 选项/资源
directives 自定义指令

filters 过滤器

components 组件

## 选项/组合
$parent 可以拿到当前子组件的父实例
$children 是一个数组 节制地使用 $parent 和 $children - 它们的主要目的是作为访问组件的应急方法。更推荐用 props 和 events 实现父子组件通信

mixins Array<Object>
选项接收一个混入对象的数组。这些混入对象可以像正常的实例对象一样包含实例选项，这些选项将会被合并到最终的选项中，使用的是和 Vue.extend() 一样的选项合并逻辑。也就是说，如果你的混入包含一个 created 钩子，而创建组件本身也有一个，那么两个函数都会被调用。

Mixin 钩子按照传入顺序依次调用，并在调用组件自身的钩子之前被调用。



