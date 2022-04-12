## v-model 的解释
* 该指令应用在 input、select、textarea 元素上 会根据控件类型的不同而自定选取不同的方法来更新元素 本质上是语法糖 它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理

* 把该指令应用在 components 组件上 默认会利用名为 value 的 prop 值 和名为 input 的 event 事件 但是像单选框、复选框等类型的输入控件可能会将 value 特性用于不同的目的。model 选项可以用来避免这样的冲突

注意：给组件添加 v-model 指令时 默认会把 value 作为组件的属性 然后把 input 事件作为给组件绑定的事件名 如果我们需要是 checked 属性的时候 当点击这个单选框的时候只会触发 change 事件而不会触发 oninput 事件
那么 自定义组件的 v-model 值可以通过自定义的 prop/event 来完成我们需要的结果