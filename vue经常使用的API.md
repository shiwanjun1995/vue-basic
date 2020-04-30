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
vm.$data 类型：Object | Function
实例创建后 vm.a 等价于 vm.$data.a 因为 Vue 实例也代理了 data 对象上所有的 property
以 _ 或 $ 开头的 property 不会被 Vue 实例代理，因为它们可能和 Vue 内置的 property、API 方法冲突。你可以使用例如 vm.$data._property 的方式访问这些 property。
当一个组件被定义，data 必须声明为返回一个初始数据对象的函数，因为组件可能被用来创建多个实例。如果 data 仍然是一个纯粹的对象，则所有的实例将共享引用同一个数据对象！通过提供 data 函数，每次创建一个新实例后，我们能够调用 data 函数，从而返回初始数据的一个全新副本数据对象。简单说, 在实例中data是对象, 在组件中data就得是函数返回对象

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

## 选项/其它
name 类型 string
允许组件模板递归地调用自身。注意，组件在全局用 Vue.component() 注册时，全局 ID 自动作为组件的 name

model 类型 { prop?: string, event?: string }
在自定义组件上使用 在使用 v-model 指令的时候定制prop和event 默认情况下 一个组件上的 v-model 会把 value 作为 prop 且把 input 用作 event 但是一些输入类型 比如单选框和复选框按钮可能想使用 value prop 来达到不同的目的 使用 model 选项可以回避这些情况产生的冲突

## 实例property
vm.$data 类型：Object
Vue 实例观察的数据对象。Vue 实例代理了对其 data 对象 property 的访问。

vm.$props 类型：Object
当前组件接收到的 props 对象。Vue 实例代理了对其 props 对象 property 的访问。

vm.$el 类型：Element
Vue 实例使用的根 DOM 元素。

vm.$parent 类型：Vue instance
父实例，如果当前实例有的话。

vm.$root 类型：Vue instance
当前组件树的根 Vue 实例。如果当前实例没有父实例，此实例将会是其自己。

vm.$children 类型：Array<Vue instance>
当前实例的直接子组件。需要注意 $children 并不保证顺序，也不是响应式的。如果你发现自己正在尝试使用 $children 来进行数据绑定，考虑使用一个数组配合 v-for 来生成子组件，并且使用 Array 作为真正的来源。

vm.$slots 类型：{ [name: string]: ?Array<VNode> }
用来访问被插槽分发的内容。每个具名插槽有其相应的 property (例如：v-slot:foo 中的内容将会在 vm.$slots.foo 中被找到)。default property 包括了所有没有被包含在具名插槽中的节点，或 v-slot:default 的内容。
在使用渲染函数书写一个组件时，访问 vm.$slots 最有帮助。

vm.$refs 类型：Object
一个对象，持有注册过 ref attribute 的所有 DOM 元素和组件实例。

vm.$attrs 类型：{ [key: string]: string }
如果attrs被绑定在子组件childcom上后，我们就可以在孙子组件grandcom里获取到this.$attrs的值。这个{{$attrs}}的值是父组件中传递下来的props（除了子组件childcom组件中props声明的）。

vm.$listeners 类型：{ [key: string]: Function | Array<Function> }
$listeners可以让你在孙子组件改变父组件的值

## 实例方法/生命周期
vm.$forceUpdate()
迫使 Vue 实例重新渲染。注意它仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件。

vm.$nextTick() 参数：{Function} [callback]
将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上。

## 指令
v-text 预期： String 更新元素的文本内容
v-html 预期： String 更新元素的innerHTML
v-show 预期： any    根据表达式之真假值来切换元素的display属性
v-if   预期： any    根据表达式的值的 truthiness 来有条件地渲染元素
v-else-if 预期： any  表示 v-if 的“else if 块”。可以链式调用
v-else 预期： 无     为 v-if 或者 v-else-if 添加“else 块”
v-for  预期： Array | Object | number | string | Iterable (2.6 新增) 基于源数据多次渲染元素或模板块。此指令之值，必须使用特定语法 alias in expression，为当前遍历的元素提供别名：v-for 的默认行为会尝试原地修改元素而不是移动它们。要强制其重新排序元素，你需要用特殊 attribute key 来提供一个排序提示：

v-on  预期： Function | Inline Statement | Object
v-bing  预期： any (with argument) | Object (without argument) 修饰符： .sync 语法糖，会扩展成一个更新父组件绑定值的 v-on 侦听器。

v-model  预期： 随表单控件类型不同而不同 限制： <input> <select> <textarea> components 修饰符：
.lazy - 取代 input 监听 change 事件
.number - 输入字符串转为有效的数字
.trim - 输入首尾空格过滤
在表单控件或者组件上创建双向绑定

v-slot  预期：可放置在函数参数位置的 JavaScript 表达式 (在支持的环境下可使用解构)。可选，即只需要在为插槽传入 prop 的时候使用

## 特殊attribute
key 预期：number | string
key 的特殊 attribute 主要用在 Vue 的虚拟 DOM 算法，在新旧 nodes 对比时辨识 VNodes。如果不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法。而使用 key 时，它会基于 key 的变化重新排列元素顺序，并且会移除 key 不存在的元素。

## 内置的组件
<keep-alive></keep-alive> 缓存组件实例
<slot></slot> <slot> 元素作为组件模板之中的内容分发插槽。<slot> 元素自身将被替换。


