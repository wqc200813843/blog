# vue-router
## 导航守卫
- 全局守卫
    
    1. 前置守卫
    
        ```javascript
        const router = new VueRouter({ ... })

        router.beforeEach((to, from, next) => {
          // ...
        })
        ```
        - to：将要进入的路由
        - from：将要离开的路由
        - next(Function):必须触发此方法resolve这个钩子
            - next()进入下一个钩子
            - next(false)回退到from的路由
            - next('路由')或者next({path:'路由'})中断当前，跳转到新的路由
            - next(error)：error是个Error实例，中断然后进入router.onError注册过的回调
        
        路由跳转前触发
        多个前置守卫时，按照创建顺序调用
    2. 解析守卫

        router.beforeResolve
    3. 后置钩子
    
        ```javascript
        router.afterEach((to, from) => {
          // ...
        })
        ```
        不用写next函数，也不会改变导航本身
- 路由独享守卫

    ```javascript
    const router = new VueRouter({
      routes: [
        {
          path: '/foo',
          component: Foo,
          beforeEnter: (to, from, next) => {
            // ...
          }
        }
      ]
    })
    ```
    beforeEnter类似前置守卫beforeEach
- 组件内守卫
    - beforeRouteEnter
    
        不能访问this,因为进入此方法时，组件还没被创建。可以通过next(vm=>{})，导航被确认的时候执行回调，来访问this
    - beforeRouteUpdate (2.2 新增)
    
        路由改变，但是组件被复用时调用，进入此方法
    - beforeRouteLeave
    
        离开该组件对应的路由时调用，铜材用来禁止在还未保存修改前突然离开，通过next(false)
- 完整的导航解析流程

  1. 导航被触发。
  2. 在失活的组件里调用离开守卫。
  3. 调用全局的 beforeEach 守卫。
  4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
  5. 在路由配置里调用 beforeEnter。
  6. 解析异步路由组件。
  7. 在被激活的组件里调用 beforeRouteEnter。
  8. 调用全局的 beforeResolve 守卫 (2.5+)。
  9. 导航被确认。
  10. 调用全局的 afterEach 钩子。
  11. 触发 DOM 更新。
  12. 用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。
## 路由元信息
  我们称呼 routes 配置中的每个路由对象为 路由记录。因为路由对象是可以嵌套的，所有一个路由匹配成功后，可能会匹配到多个路由记录，这些路由记录都会暴露为$route对象的$route.matched数组。
## 过渡动效
  `<router-view>`是基本的动态组件，所以`<transition>`的所有功能都适用
- 单个路由的过渡
  
  ```
  template: 
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  ```
  用transition嵌套在template的最外层并设置不用的name
- 基于路由的动态过渡
  ```
  <!-- 使用动态的 transition name -->
  <transition :name="transitionName">
    <router-view></router-view>
  </transition>
  // 接着在父组件内
  // watch $route 决定使用哪种过渡
  watch: {
    '$route' (to, from) {
      const toDepth = to.path.split('/').length
      const fromDepth = from.path.split('/').length
      this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
    }
  }
  ```
## 数据获取
在导航完成前获取数据
```
beforeRouteEnter (to, from, next) {
  getPost(to.params.id, (err, post) => {
    next(vm => vm.setData(err, post))
  })
},
// 路由改变前，组件就已经渲染完了
// 逻辑稍稍不同
beforeRouteUpdate (to, from, next) {
  this.post = null
  getPost(to.params.id, (err, post) => {
    this.setData(err, post)
    next()
  })
}
```
## 滚动行为
**注意: 这个功能只在 HTML5 history 模式下可用**
```
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```
第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。
- { x: number, y: number }
- { selector: string, offset? : { x: number, y: number }} (offset 只在 2.6.0+ 支持)
- return {x: 0,y: 0}滚动到顶部
## 路由懒加载
```
const Foo = () => Promise.resolve({ /* 组件定义对象 */ })
```
```
const Foo = () => import('./Foo.vue')
```
- 把组件按组分块

注释语法
```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```
## `<router-link>`
- Props
    1. to 必填
        router.push()
    2. replace
        router.replace()
    3. append
        相对路径，路径不能以/开头，基于当前路径
    4. tag
        让路由不以a标签形式展示，采用tag属性值的标签 
    5. active-class 
        链接激活时使用的CSS类名。默认router-link-active。可以通过路由的linkActiveClass配置
    6. exact boolean
        默认全包含匹配，/会点亮所有路由，
        ```
        <!-- 这个链接只会在地址为 / 的时候被激活 -->
        <router-link to="/" exact>
        ```
    7. `event string|Array<string>`
        触发导航的事件，默认click
    8. exact-active-class
        精确匹配路由时激活的class

## `<router-view>`

- 可以嵌套`<router-view>`
- 如果keep-alive和transition一起使用，要确保keep-alive在内部