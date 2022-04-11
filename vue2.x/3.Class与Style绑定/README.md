# 操作class列表与内联样式
因为他们都是属性 操作数据绑定非常常见 只需要通过表达式计算出字符串结果即可 表达式结果的类型除了字符串之外，还可以是对象或数组 [ string obj [] ] 

## 绑定 HTML Class
```html
对象语法：
<div v-bind:class="{ active: isActive }"></div>
上面的语法表示 active 这个 class 存在与否将取决于数据属性 isActive 的 布尔值 对象中可传入更多的属性来动态切换多个class 也可以与普通的class属性共存 多个属性之间用 ，隔开 
小tips: 类名长的话 可以用短横线隔开 如'text-danger'
也可以绑定一个返回对象的计算属性 通过触发计算属性重新计算值 从而触发UI更新显示 计算属性特性：只有被绑定的状态值发生变化时，则计算属性的值才会重新计算得出最新结果，因为计算属性会缓存计算结果。


数组语法：
<div v-bind:class="[activeClass, errorClass]"></div>
把一个数组传给 v-bind:class 以应用一个class列表 且数组语法中也可以使用对象语法

当有多个条件时 class 时，可以通过设置一个默认的类名放在最后。
<div v-bind:class="[{ active: 'activeClass' }, defaultClass]">
```

## 绑定内联样式
```html
对象语法：
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
上面的语法非常直观 看着像css 但其实是js对象 css属性名称可以用驼峰命名法 或者短横线分隔 需要用引号括起来 同样的，对象语法常常结合返回对象的计算属性使用。

数组语法：
<div v-bind:style="[baseStyles, overridingStyles]"></div>
可以将多个样式对象应用到同一个元素上

```
