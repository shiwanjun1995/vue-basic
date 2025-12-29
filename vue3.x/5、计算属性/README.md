## 计算属性缓存 vs 方法？

```js
如果我们将同样的函数定义为一个方法而不是计算属性，两种方式在结果上确实是完全相同的，但是计算属性是基于其响应式依赖被缓存的。这意味着只要响应式依赖没有发生改变，多次访问计算属性会立即返回之前的计算结果，而不会再次执行getter函数
相比之下，方法调用总是会在组件发生重新渲染的时候再次执行函数。
下面的计算属性永远不会更新，因为 Date.now() 不是响应式依赖：
const now = computed(() => {
    return Date.now();
});
因为响应式依赖的条件：
1、必须是响应式对象（ref、reactive）
2、必须在计算属性的 getter 中被访问
3、Date.now()它就是一个普通的js函数，返回的值不是响应式的，vue不会追踪它的变化，因此计算属性认为“没有依赖变化”，就一直返回缓存值
如果需要一个随着时间更新的时间戳，如下可以实现：
const time = ref(Date.now());
setInterval(() => {
    time.value = Date.now();
}, 1000);
const newTime = computed(() => {
    return new Date(time.value).toLocaleTimeString();
});
计算属性高效之处就是只有当响应式依赖发生改变的时候，才会重新计算，避免了不必要的性能开销。
```

## 最佳实践

```js
计算属性的getter应该只做计算，而没有其它副作用，比如，不要改变其它状态、在getter中做异步操作或者更改DOM。如果需要修改，可以使用帧听器（watch）来根据响应式状态的变更来创建副作用。
触发新的计算应该更新它所依赖的源状态，而不是直接修改计算属性的返回值。

```

## 展示过滤或排序后的结果

```js
可以创建返回已过滤或已排序数组的计算属性，如下：
const numbers = ref([1, 2, 3, 4, 5])

const evenNumbers = computed(() => {
  return numbers.value.filter((n) => n % 2 === 0)
})

当计算属性不可行的情况下（在多层潜逃的v-for循环中），可以使用以下方法：
<ul v-for="numbers in sets">
  <li v-for="n in even(numbers)">{{ n }}</li>
</ul>

const sets = ref([
  [1, 2, 3, 4, 5],
  [6, 7, 8, 9, 10]
])

function even(numbers) {
  return numbers.filter((number) => number % 2 === 0)
}

在计算属性中使用这两类函数一定要小心：reverse()和sort()，因为这两个函数会改变原数组，计算函数中不应该这么做，而是在调用方法前创建一个副本：
const evenNumbers = computed(() => {
    return [...numbers.value].reverse()
    // return [...numbers.value].sort((a, b) => a - b)
});


```
