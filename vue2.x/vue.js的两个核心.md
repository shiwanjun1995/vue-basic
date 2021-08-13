## vue.js的两个核心点
> vue 是一个提供MVVM数据双向绑定模式的库 专注于UI层面 有两个核心点：数据驱动和组件化系统

### 数据驱动
![mvvm.png](https://www.ipicbed.com/images/2021/05/10/mvvm.png)

>MVVM模型示意图：
Model 模型 ==> 数据保存 对应的是js对象；
View 视图  ==> 用户界面 对应的是DOM结构；
ViewModel  ==> 在vue中指的是vue实例 充当数据和视图之间通信的桥梁

* 从View侧看 ViewModel中的DOM Listeners工具会帮我们监听DOM事件 并且通过methods中的操作 来改变Model中的数据
* 从Model侧看 ViewModel中的Data Bingdings工具会帮助我们监听Model数据 当数据发生更新后 来更新页面中的DOM元素

1.在vue实例化的过程中 会遍历实例化对象选项中的data选项 遍历其所有属性并使用 Object.defineProperty() 添加 getter 和 setter 每个组件实例都对应一个watcher 它是用来观测数据的对象 这个对象中包含了待渲染的关联DOM元素

2.在组件渲染的过程中 会把“接触”过的数据属性记录为依赖 也就是watcher做取值的时候 就会触发getter 这里我们就会把data选项（观察对象）注册为自己的一个订阅者 并作为当前watcher的一个依赖收集起来

3.之后当依赖项的setter触发时 即当data选项（观察对象）被赋值的时候 会notify通知所有订阅自己的watcher重新取值 并触发相应的更新 使watcher对象中关联的组件重新渲染

这样 我们就完成了数据改变到视图更新的一个自动过程

### 组件化系统
![components.png](https://www.ipicbed.com/images/2021/05/10/components.png)

> 扩展HTML元素 封装可复用性代码 应用界面完全可以看成是一棵组件树

一个组件在本质上是一个拥有预定义选项的一个vue实例 对应的也就是ViewModel ViewModel树与DOM树一一对应

组件的核心选项：
* 模板（template）：模板声明了数据和最终展现给用户的DOM之间的映射关系。
* 初始数据（data）：一个组件的初始数据状态。对于可复用的组件来说，这通常是私有的状态。
* 接受的外部参数(props)：组件之间通过参数来进行数据的传递和共享。
* 方法（methods）：对数据的改动操作一般都在组件的方法内进行。
* 生命周期钩子函数（lifecycle hooks）：一个组件会触发多个生命周期钩子函数。

Webpack是一个开源的前端模块构建工具，它提供了强大的loader API来定义对不同文件格式的预处理逻辑，这是.vue后缀单文件组件形式的基础。所以在此基础上，尤大开发的vue-loader允许将模板、样式、逻辑三要素整合在同一个文件中，以.vue文件后缀形成单文件组件格式，方便项目架构和开发引用。

其它特性：
* 异步批量DOM更新：当大量数据变动时，所有受到影响的watcher会被推送到一个队列中，并且每个watcher只会推进队列一次。这个队列会在进程的下一个 tick异步执行。这个机制可以避免同一个数据多次变动产生的多余DOM操作，也可以保证所有的DOM写操作在一起执行，避免DOM读写切换可能导致的layout。
* 可扩展性：除了自定义指令、过滤器和组件，Vue.js还提供了灵活的mixin机制，让用户可以在多个组件中复用共同的特性。

参考链接：https://www.cnblogs.com/showcase/p/10577604.html
https://segmentfault.com/a/1190000006599500#articleHeader2



