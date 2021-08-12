# 事件传递参数
```html
<button v-on:click="say('hi')">Say hi</button>
<!-- 手动传入$event 并且前面传了参数 第二个参数是事件对象 -->
<a @click="sayHi('a标签跳转',$event)" href="https://www.baidu.com/">说你好</a>
```

# 事件修饰符
Vue.js 为 v-on 提供了事件修饰符。之前提过，修饰符是由点开头的指令后缀来表示的。
```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a> 不会传播事件到父盒子上 如果app加了事件的话 是不会触发
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form> 不是很清楚
<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

注意：使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。
```

# 按键修饰符
```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit"> 在上述示例中，处理函数只会在 $event.key 等于 PageDown 时被调用。
```

# 按键码
keyCode 的事件用法已经被废弃了并可能不会被最新的浏览器支持。
使用 keyCode 特性也是允许的：
```html
<input v-on:keyup.13="submit">
为了在必要的情况下支持旧浏览器，Vue 提供了绝大多数常用的按键码的别名：
.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right
```

# 其它的修饰符
https://cn.vuejs.org/v2/guide/events.html#内联处理器中的方法

# 为什么在 HTML 中监听事件?
你可能注意到这种事件监听的方式违背了关注点分离 (separation of concern) 这个长期以来的优良传统。但不必担心，因为所有的 Vue.js 事件处理方法和表达式都严格绑定在当前视图的 ViewModel 上，它不会导致任何维护上的困难。实际上，使用 v-on 有几个好处：
* 扫一眼 HTML 模板便能轻松定位在 JavaScript 代码里对应的方法。
* 因为你无须在 JavaScript 里手动绑定事件，你的 ViewModel 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试。
* 当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除。你无须担心如何清理它们。