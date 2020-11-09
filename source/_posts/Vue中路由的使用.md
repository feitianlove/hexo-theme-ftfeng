---
title: Vue 中路由的使用
abbrlink: 8f08328
---

## 介绍路由

- **路由** : 是浏览器 **URL 中的哈希值**( # hash) 与 **展示视图内容** 之间的对应规则
  - 简单来说,路由就是一套映射规则(一对一的对应规则), 由开发人员制定规则.- 
  - 当 URL 中的哈希值( `#` hash) 发生改变后,路由会根据制定好的**规则**, 展示对应的视图内容
- **为什么要学习路由?**
  - 在 web App 中, 经常会出现通过一个页面来展示和管理整个应用的功能.
  - SPA 往往是功能复杂的应用,为了有效管理所有视图内容,前端路由 应运而生.
- **vue 中的路由** : 是 **hash** 和 **component** 的对应关系, **一个哈希值对应一个组件**


## 一 : 路由的基本使用

#### 准备工作 (3个)

- **安装**  :  `npm i vue-router`
- **引入** : 

```html
<script src="./vue.js"></script>
// 千万注意 :引入路由一定要在引入vue之后,因为vue-router是基于vue工作的
<script src="./node_modules/vue-router/dist/vue-router.js"></script>
```

- **实例路由对象 + 挂载到vue上**
  - 实例路由对象 : `const router = new VueRouter()`
  - 挂载到vue上 :  `new Vue({ router,data,methods })`
  - 验证路由是否挂载成功, 就看打开页面,最后面有没有个 

#### 具体步骤 (4个)

- 1.**入口**

- 2.**路由规则**

- 3.**组件**

- 4.**出口**

```js
# 1. 入口
	 // 方式1 : url地址为入口   调试开发用
	 输入url地址 改变哈希值 `01-路由的基本使用.html#/one`	
	 // 方式2 : 声明式导航 : router-link+to (见下面介绍)
# 2. 路由规则
// path : 路由路径
// component : 将来要展示的路由组件
routes: [
    { path: '/one', component: One }, 
    { path: '/two', component: Two }
]
# 3. 组件
// 使用返回值的这个组件名称
const One = Vue.component('one', {
  template: ` <div> 子组件 one </div> `
})
# 4. 出口
<!--  出口 组件要展示的地方-->
<router-view></router-view>

# 总结
拿到入口哈希路径, 根据路由匹配规则,找到对应的组件,显示到对应的出口位置 
```



## 二 : 由使用注意事项

- **入口**
  - 最常用的入口 是 **声明式导航 router-link**

```html
<!-- 
router-link 组件最终渲染为 a标签, to属性转化为 a标签的href属性
to 属性的值 , 实际上就是哈希值,将来要参与路由规则中进行与组件匹配
  -->
<router-link to="/one">首页</router-link>
```

- **组件**

```js
const One = {
  template: `<div> 子组件 one </div> `
}
```



- **演示 : 多个组件匹配**

```js
<div id="app">
  <!-- 1 路由入口：链接导航 -->
  <router-link to="/one">One</router-link>
  <router-link to="/two">Two</router-link>

  <!-- 4 路由出口：用来展示匹配路由视图内容 -->
  <router-view></router-view>
</div>

<!--  导入 vue.js -->
<script src="./vue.js"></script>
<!--  导入 路由文件 -->
<script src="./node_modules/vue-router/dist/vue-router.js"></script>
<script>
  // 3 创建两个组件
  const One ={
    template: '<h1>这是 one 组件</h1>'
  }
  const Two =  {
    template: '<h1>这是 two 组件</h1>'
  }

  // 0 创建路由对象
  const router = new VueRouter({
    // 2. 路由规则
    routes: [
      { path: '/one', component: One },
      { path: '/two', component: Two }
    ]
  })

  const vm = new Vue({
    el: '#app',
    //0. 不要忘记，将路由与vue实例关联到一起！
    router
  })
</script>
```



## 三  :  入口导航菜单高亮处理

- **点击导航 =**> 元素里添加了两个类

```html
<a href="#/one" class="router-link-exact-active router-link-active">One</a>
<a href="#/two" class="">Two</a>
#另外一种方式(表示只允许精确匹配)
<router-link to='/' exact></router-link>
```

- **修改方式1 : 直接修改类的样式**

```css
# router-link-exact-active 精确匹配
# router-link-active 模糊匹配，当url上的路径邦汉href的值
.router-link-exact-active,
.router-link-active {
  color: red;
  font-size: 50px;
}
```

- **修改方式2 : 使用存在过的类样式 => 修改默认高亮类名** 

```js
const router = new VueRouter({
  routes: [],
  // 修改默认高亮的a标签的类名
  // red 是已经存在过的，去掉router后面位驼峰式，加一个class
  linkActiveClass: 'red'
})
```



## 四: 路由配置

### 4.1 动态路由 => 详情列表

> 导入 : 列表三个手机都要点击进去详情页, 只需要一个组件,显示不同的数据即可

```js
# 入口
<router-link to="/detail/1">手机1</router-link>
<router-link to="/detail/2">手机2</router-link>
<router-link to="/detail/3">手机3</router-link>

<router-link to="/detail">手机4</router-link>  没有参数如何????

# 规则（？表示可传可不传）
routes: [
  // 2 . 路由规则
  { path: '/detail/:id?', component: Detail }
]

# 获取参数的三种方式
const Detail =  {
    template: `
        // 方式1 : 组件中直接读取
        <div> 显示详情页内容....{{ $route.params.id  }} </div>
    `,
    created() {
        // 方式2 : js直接读取
        // 打印只会打印一次,因为组件是复用的,每次进来钩子函数只会执行一次
        console.log(this.$route.params.id)
    },
    // 方式3 : 监听路由的参数,为什么不需要深度监听,因为一个路径变化,就会对应一个对新的路由对象(地址变)
    watch: {
        $route(to, from) {
            console.log(to.params.id)
        }
    }
}

```

### 4.2 路由对象 - $route

- 一个**路由对象 (route object)** 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息 

- 一个哈希值路径  ==> 一个路由对象

- **$route.path**

  - 类型: `string`
  - 字符串，对应当前路由的路径，总是解析为绝对路径，如 `"/foo/bar"`。
  - `# 后面?前面的内容`

- **$route.params**

  - 类型: `Object`
  - 一个 key/value 对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。
  - 一个 key/value 对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。

- **$route.query**

  - 类型: `Object`
  - **参数对象**
  - 一个 key/value 对象，表示 URL 查询参数。例如，对于路径 `/foo?user=1`，则有 `$route.query.user == 1`，如果没有查询参数，则是个空对象。

- **$route.hash**

  - 类型: `string`

    当前路由的 hash 值 (带 `#`) ，如果没有 hash 值，则为空字符串。

- **$route.fullPath**

  - 类型: `string`
  - **全路径**
  - 完成解析后的 URL，包含查询参数和 hash 的完整路径。

```js
# 演示 : 
<router-link to="/detail/4?age=21#one">detail</router-link>
{ path: '/detail/:id?', component: detail } 
在组件内 created打印 this.$route
> fullPath: "/detail/4?id=001#one"
> hash : "#one"
> params : {id:'4'}
> query : {age : 21}
> path : '/detail/4'
```

**使用wathc监听$route 路由对象获取里面的信息**

### 4.3 嵌套路由 => children

> 导入 : url测试 parent 和child, 想让child 在 parent 中显示

- parent 的内部 添加 : ` <router-view> </router-view>`
- 规则里添加 children 
- **/child 和 child 的区别**
  - 如果是`/child` => 那么访问就可以直接访问`#/child`就可以访问 子组件
  - 如果是`child` => 那么访问就应该访问`#/parent/child`才可以访问子组件

```js
const parent = {
    template: `<p>parent  <router-view> </router-view> </p>`
}
const child = {
    template: `<p>child</p>`
}

const router = new VueRouter({
    routes: [
        {
            path: '/parent',
            component: parent,
            children: [
                #注意这里child加/和不加/的区别。加/使用的路径使用/child不加/使用/parent/child
                { path: '/child', component: child }
            ]
        }
    ]
})
```



### 4.4 命名路由

- 有时候，通过一个名称来标识一个路由显得更方便一些，
- 特别是在**链接一个路由**，或者是执行一些**跳转**的时候。 ===> 场景
- 你可以在创建 Router 实例的时候，在 `routes` 配置中给某个路由设置名称。  ==> 如何命名

```
# 命名
routes: [
    {
        path: '/parent',
        name: 'parent',
        component: parent
    }
]

# 入口链接 + 跳转  (使用 path 和 name 的转换)
<!-- 方式1 : url手动写 -->

<!-- 方式2 : 入口链接 声明式导航 -->
<router-link to="/parent">点击</router-link>
<router-link :to="{ name : 'parent' }">点击</router-link>  # 忘了 带 : 原始对象类型

<!-- 方式3 : 编程式导航 -->
 fn() {
     // this.$router.push('/parent')
     this.$router.push({
         name: 'parent'
     })
 }
```



### 4.5 命名视图

> 导入 :  有时候想同时 (同级) 展示多个视图，
>
> 需求 : 访问 / 根目录 同时展示以下三个组件

- **三个组件**

```js
const header = {
    template: `<p>header  </p>`
}
const main = {
    template: `<p>main  </p>`
}
const footer = {
    template: `<p>footer  </p>`
}
```

- **规则**

```js
# 以前的那个方式只能显示三个 header
# 演示之前的效果 

routes: [
    {
        path: '/',
        components: {
            default: header,
            m: main,
            f: footer
        }
    }
]
```

- **出口**

```html
<router-view> </router-view>
<router-view name="m"> </router-view>
<router-view name="f"> </router-view>
```



### 4.6 重定向

```js
{path: '/', redirect: '/header'}
{path: '/', redirect: { name: 'header' }}
#方式三就是函数了，to为路由对象
redirect: to => {
      // console.log(to)
    return {
        name: 'about'
    }
}
```

### 4.7 组件传参

- **原始方式使用 $route获取**

```js
# 入口
    <router-link to="/header/3">123</router-link>
# 规则
routes: [
    {
        path: '/header/:id',
        component: header,
    }
]
# 获取参数
const header = {
    template: `<p>header  {{ $route.params.id }}  </p>`
}
```

- **布尔模式**

```js
# 入口
    <router-link to="/header/3">123</router-link>

# 规则
routes: [
    {
        path: '/header/:id',
        component: header,
        // 如果 props 被设置为 true，route.params 将会被设置为组件属性
        props: true
    }
]

# 获取参数
const header = {
    // 参数 id 当成参数
    props: ['id'],
    template: `<p>header   {{ id }} </p>`
}
```

- **对象模式**

```js
# 入口
 <router-link to="/header">123</router-link>

# 规则
 routes: [
     {
         path: '/header',
         component: header,
         props: { foo: '0000' }
     }
 ]
# 组件
 const header = {
        props: ['foo'],
        template: `<p>header   {{ foo }} </p>`
 }
```

- **函数模式**

```js
# 同对象模式一样也需要座位组件的属性
# 区别是props值不一样
 props: to => {
     return { foo: '0000' }
 }
 # 组件
 const header = {
        props: ['foo'],
        template: `<p>header   {{ foo }} </p>`
 }
```

- 注意 : 对象模式和函数模式参数 在props里,所以声明式导航那里就不要传参了 



## 五 : 路由进阶

### 5.1 元信息

- **规则声明**

```js
 routes: [
     {
         path: '/header',
         component: header,
         meta: {
            title: 'XXXX'
         }
     }
 ]
```

- **获取**

```js
 created() {
    document.title = this.$route.meta.title
 }
```

- **作用 :** 

- ```
  在路由导航的时候,可以用作判断
  ```



### 5.2 编程式导航

```js
const one = {
    template: ` 
<div> <button @click="handleClick('back')">返回 上一页</button>
<button @click="handleClick('push')">跳转 two页</button>
<button @click="handleClick('replace')">替换 two页</button> 
</div>`,
    methods: {
        handleClick(type) {
            if (type == 'back') {
                // 返回
                this.$router.back()
            } else if (type == 'push') {
                // 跳转 有历史记录
                this.$router.push('/two')
            } else {
                // 替换 没有历史记录，不能back
                this.$router.replace('/two')
            }
        }
    }
}
const two = {
    template: `<p>two </p>`
}
注意： 这里$router路由实例和路由$route路由对象的不同之处
```



###  5.3 导航守卫（https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E8%B7%AF%E7%94%B1%E7%8B%AC%E4%BA%AB%E7%9A%84%E5%AE%88%E5%8D%AB）

	通过跳转或者取消的方式守卫导航。（通俗说就是：只有登陆页面才可以访问目标页面，简单例子如下：)
```
router.beforeEach((to, from, next) => {
	// to: 目标路由对象$route
	// from: 来源路由对象
	// next: 跳转，只有执行了next才能进行跳转
	// 访问 login
	if (to.name == 'login') {
			// 下一步
			next()
	} else {
			// 停止跳转
			next(false)
			// 跳转到下一步
			next({ name: 'login' }) 或者 使用路径  next('/login')
	}
})
```