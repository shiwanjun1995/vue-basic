# 避免组件中的数据互相影响

在vue中，同一个组件被复用多次就会创建多个实例，如果data是一个对象的话，对象是引用值，这个对象就是共享的，一个组件实例操作了这个对象，其它组件里这个对象也会发生改变，就会有问题。为了保证组件的数据独一无二，要求每个组件都必须通过data函数返回一个对象作为组件的状态。

每一个函数的声明都创建了独立的函数作用域。
