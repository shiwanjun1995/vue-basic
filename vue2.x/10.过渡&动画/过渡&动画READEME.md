# 进入/离开 & 列表过渡
在进入/离开的过渡中，会有 6 个 class 切换
* v-enter
* v-enter-active
* v-enter-to
* v-leave
* v-leave-active
* v-leave-to
  常用的有 v-enter-active、v-leave-active、v-enter、v-leave-to这四种

对于这些在过渡中切换的类名来说，如果你使用一个没有名字的 <transition>，则 v- 是这些类名的默认前缀。如果你使用了 <transition name="v-name">，那么 v-enter 会替换为 v-name-enter。

# 自定义过渡的类名
他们的优先级高于普通的类名，这对于 Vue 的过渡系统和其他第三方 CSS 动画库，如 Animate.css 结合使用十分有用。
