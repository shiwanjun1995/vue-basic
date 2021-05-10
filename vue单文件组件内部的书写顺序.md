## 组件/实例的选项应该有统一的顺序
![shunxu.png](https://www.ipicbed.com/images/2021/05/10/shunxu.png)
> 全局感知

* name: 'compName', // 组件的名字
> 模板依赖 模板内使用的资源

* components: {}, // 子组件
* directives: {}, // 自定义指令
* filters: {},  // 自定义过滤器
> 组合（向选项里合并属性）

* mixins: [],  // 混入选项对象 混入实例对象可以向正常的实例对象一样
> 接口

* model: { prop: 'checked', event: 'change' }  // 自定义组件在使用 v-model 时定制 prop 和 event
* props: {},  // 创建实例时父级组件向子组件传递的props值
> 本地状态（本地的响应式属性）

* data() {},  // 本地状态的取值对象
* computed: {},  // 计算属性 根据data里的一个或者多个计算出来的值
> 事件（通过响应式事件触发的回调）以及 生命周期钩子

* watch: {},  // 监视器 监听data里的一个属性
* created() {},
* mounted() {},
> 非响应式的属性（不依赖响应系统的实例属性）

* methods: {},  // 方法
