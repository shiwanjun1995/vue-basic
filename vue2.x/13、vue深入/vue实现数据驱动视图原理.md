## vue.js 实现数据驱动视图原理

数据驱动是 vuejs 最大的特点。在 vuejs 中，所谓的数据驱动就是当数据发生变化的时候，用户界面发生相应的变化，开发者不需要手动的去修改 dom。

### 数据驱动

![mvvm.png](./images//mvvm.png)

> MVVM 模型示意图：
> Model 模型 ==> 数据保存 对应的是 js 对象；
> View 视图 ==> 用户界面 对应的是 DOM 结构；
> ViewModel ==> 在 vue 中指的是 vue 实例 充当数据和视图之间通信的桥梁

## MVVM 框架

Vuejs 的数据驱动是通过 MVVM 这种框架来实现的。MVVM 框架主要包含 3 个部分:model、view 和 viewmodel。

- Model:指的是数据部分，对应到前端就是 javascript 对象
- View:指的是视图部分，对应前端就是 dom
- Viewmodel:就是连接视图与数据的中间件

数据(Model)和视图(View)是不能直接通讯的，而是需要通过 ViewModel 来实现双方的通讯。当数据变化的时候，viewModel 能够监听到这种变化，并及时的通知 view 做出修改。同样的，当页面有事件触发时，viewMOdel 也能够监听到事件，并通知 model 进行响应。Viewmodel 就相当于一个观察者，监控着双方的动作，并及时通知对方进行相应的操作。

### vue.js 是通过一个观察者来实现数据的驱动的

![data.png](https://v2.cn.vuejs.org/images/data.png)

1.在 vue 实例化的过程中 会遍历实例化对象选项中的 data 选项 遍历其所有属性并使用 Object.defineProperty() 添加 getter 和 setter 每个组件实例都对应一个 watcher 它是用来观测数据的对象 这个对象中包含了待渲染的关联 DOM 元素

2.在模版编译的过程中 会把“接触”过的数据属性记录为依赖 也就是 watcher 做取值的时候 就会触发 getter 这里我们就会把 data 选项（观察对象）注册为自己的一个订阅者 并作为当前 watcher 的一个依赖收集起来，这样就建立了视图与数据之间的联系

3.之后当渲染视图的数据依赖发生改变，就会触发 setter 就是当 data 选项（观察对象）被赋值的时候 会 notify 通知所有订阅自己的 watcher 重新取值 并触发相应的更新 使 watcher 对象中关联的组件重新渲染

这样 我们就完成了数据改变到视图更新的一个自动过程

> 参考链接
>
> > 1.https://www.cnblogs.com/caizhenbo/p/6418284.html 2.https://www.cnblogs.com/showcase/p/10577604.html 3.https://segmentfault.com/a/1190000006599500#articleHeader2
