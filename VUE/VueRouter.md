#### vue-router小伎俩

##### 1、使用 props 将组件和路由解耦

在组件中使用 $route 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。

##### 2、捕获所有路由或 404 Not found 路由

常规参数只会匹配被 / 分隔的 URL 片段中的字符。如果想匹配任意路径，我们可以使用通配符 (*)；当使用通配符路由时，请确保路由的顺序是正确的，也就是说含有通配符的路由应该放在最后

```javascript
{
  // 会匹配所有路径
  path: '*'
}
{
  // 会匹配以 `/user-` 开头的任意路径
  path: '/user-*'
}
```

##### 3、params和query

```javascript
// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

**注意：如果提供了 `path`，`params` 会被忽略，上述例子中的 `query` 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 `name` 或手写完整的带有参数的 `path`：**

```javascript
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

##### 4、router.push和router.replace

跟 router.push 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录

##### 5、动态路由

一个“路径参数”使用冒号 : 标记。当匹配到一个路由时，**参数值会被设置到 this.$route.params**，可以在每个组件内使用。于是，我们可以更新 User 的模板，输出当前用户的 ID：

```javascript
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', name: '/user', component: User }
  ]
})

//1、 <router-link :to="{name: '/user', params: {id: '1'}, query: { name: '韩梅梅'}}">props params name</router-link>
//2、 <router-link to="/user/foo">/user/foo</router-link>
```

##### 6、props路由参数传递

如果 props 被设置为 true，route.params 将会被设置为组件属性。布尔模式、对象模式、函数模式，

```javascript
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 布尔模式，对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    },
    
    // 对象模式，它会被按原样设置为组件属性。当 props 是静态的时候有用
    {
      path: '/promotion/from-newsletter',
      component: Promotion,
      props: { newsletterPopup: false }
    },
    
    // 函数模式，可以将参数转换成另一种类型，将静态值与基于路由的值结合等等
    {
      path: '/search',
      component: SearchUser,
      props: route => ({ query: route.query.q })
    }
  ]
})
```

##### 7、导航守卫

###### next()

确保 next 函数在任何给定的导航守卫中都被严格调用一次。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错。这里有一个在用户未能验证身份时重定向到 /login 的示例：

```javascript
// BAD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  // 如果用户未能验证身份，则 `next` 会被调用两次
  next()
})

// GOOD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
```

