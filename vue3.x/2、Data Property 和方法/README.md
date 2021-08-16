# Vue 2 过渡

# 1、@vue/compiler-sfc 替换掉了 vue-template-compiler
```json
1、vue-template-compiler: 模板解析器（使用vue-template-compiler直接读取处理了.vue文件SFC内容，将.vue文件处理为一个AST。）
    这个包可用于将Vue 2.0模板预编译为呈现函数，以避免运行时编译开销和CSP限制。在大多数情况下，应该将它与vue-loader一起使用，只有在编写具有非常特定需求的构建工具时才会单独使用它。在 vue 工程中，安装依赖时，需要 vue 和 vue-template-compiler 版本必须保持一致。
2、@vue/compiler-sfc: 用来解析SFC组件（.vue文件）
```

## 组件实例 property
```json
1、在 data 中定义的 property 是通过组件实例暴露的：
2、还有各种其他的组件选项，可以将用户定义的 property 添加到组件实例中，例如 methods，props，computed，inject 和 setup。我们将在后面的指南中深入讨论它们。组件实例的所有 property，无论如何定义，都可以在组件的模板中访问。
3、Vue 还通过组件实例暴露了一些内置 property，如 $attrs 和 $emit。这些 property 都有一个 $ 前缀，以避免与用户定义的 property 名冲突。
```

## 生命周期图示
1、vue2.x
![vue2.png](https://cn.vuejs.org/images/lifecycle.png)
2、vue3.x
![vue3.png](https://v3.cn.vuejs.org/images/lifecycle.svg)
