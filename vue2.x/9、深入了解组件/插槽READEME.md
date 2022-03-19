# 插槽内容
组件内可以放入需要渲染的html模板内容 可以将内容从父级传给具名插槽

# 编译作用域
父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。
  
# 后备内容(默认内容)
有时候为插槽设置一个默认的内容是很有用的 它只会在没有提供内容的时候被渲染 就是在
```js
<slot>写上默认内容<slot>
应用在一个父级组件上的时候 这个子组件中的内容 可以写上你需要渲染的插槽内容
```

## 具名插槽
<slot> 有一个特殊的属性 名称 name 具名插槽 实际意思就是给插槽取一个名称 一个子组件可以放多个插槽 而且可以放在不同的地方 而父组件在填充内容的时候 可以根据这个名称把内容填充到对应的插槽中

* 任何没有被包裹在带有 v-slot 的 <template>中的内容都会被视为默认插槽内容（如果你希望更明确一些 可以给其指名为 v-slot:default）
* 注意： v-slot 只能添加在 <template> 上 [这和已经废弃的 slot 的属性不同]

# 作用域插槽
有时候让插槽内容能够访问子组件中才有的数据是很有用的 
* 为了让子组件的数据在父级组件的插槽内容中也是可用的 需要将这个数据[key]作为插槽元素的一个属性绑定上去
* 绑定在插槽元素上的属性 ==> 插槽prop 现在在父级作用域中 我们可以使用带值的 v-slot 来定义我们提供的插槽 prop 的名字 [在这个例子中，我们把包含所有插槽值的对象命名为slotProps，这个名字是随便你取得]

# 独占默认插槽的缩写语法
当被提供的内容只有默认插槽的时候 组件的标签才可以被当做插槽的模板来使用 这样的话可以直接使用 v-slot 这个指令在组件上
* 注意默认插槽的缩写语法不能和具名插槽混用，因为它会导致作用域不明确
* 只要出现多个插槽，请始终为所有的插槽使用完整的基于 <template> 的语法：
```js
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</current-user>
```

# 解构插槽prop
作用域插槽的内部工作原理是将你的插槽内容包括在一个传入单个参数的函数里 这意味着 v-slot 的值实际上可以是任何能够作为函数定义中的参数的 JavaScript 表达式。
所以在支持的环境下 (单文件组件或现代浏览器)，你也可以使用 ES2015 解构来传入具体的插槽 prop，如下：

```js
<todo-list v-slot="{ item }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
```

这样可以使模板更简洁，尤其是在该插槽提供了多个 prop 的时候。它同样开启了 prop 重命名等其它可能，例如将 item 重命名为 todo：

```js
<todo-list v-slot="{ item: todo }">
  <i class="fas fa-check"></i>
  <span class="green">{{ todo }}</span>
</todo-list>
```

你甚至可以定义备用内容，用于插槽 prop 是 undefined 的情形：

```js
<todo-list v-slot="{ item = 'Placeholder' }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>
```

# 动态插槽名
动态指令参数也可以用在 v-slot 上，来定义动态的插槽名

# 具名插槽的缩写为 #  即 （v-slot:） ===> #
* 和其他指令一样 该缩写只有在其有参数的时候才可用 

<!-- 这将触发一个警告 -->

<todo-list #="{ item }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>

如果希望使用缩写的话，你必须始终以明确的插槽名取而代之：

<todo-list #default="{ item }">
  <i class="fas fa-check"></i>
  <span class="green">{{ item }}</span>
</todo-list>