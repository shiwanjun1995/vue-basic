# 一般在两种情况下会这么做

1. 为了过滤一个列表中的项目（在这种情形下，请将 users 替换为一个计算属性 (比如 activeUsers)，让其返回过滤后的列表。）

  ```js
    v-for="user in users" v-if="user.isActive"
  ```

2. 为了避免渲染本应该隐藏的列表（这种情形下，请将 v-if 移动至容器元素上 (比如 ul、ol)。）

  ```js
    v-for="user in users" v-if="shouldShowUsers"
  ```

原因：这是因为当 Vue 处理指令时，v-for 比 v-if 具有更高的优先级。【v-for 会遍历创建对应的 dom 节点，如果 v-if 为 false，会删除这个 dom 节点，增加页面渲染的性能】

解决办法：将其更换为一个计算属性，然后再进行遍历。

  ```js
    computed: {
      activeUsers: function () {
        return this.users.filter(function (user) {
          return user.isActive
        })
      }
    }
  ```

- 过滤后的列表只会在 users 数组发生相关变化时才被重新运算，过滤更高效。
- 使用 v-for="user in activeUsers" 之后，我们在渲染的时候只遍历活跃用户，渲染更高效。
- 解耦渲染层的逻辑，可维护性 (对逻辑的更改和扩展) 更强。

> 参考链接：1.https://blog.csdn.net/weixin_44475093/article/details/110607035
