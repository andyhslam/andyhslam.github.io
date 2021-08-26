# vue-router
- 路由是页面级别的导航
* 动态路由匹配（路由传参）
1. 在路由的配置文件定义传递的key
2. 在跳转的位置传递数据
3. 接收参数：跳转到哪就在哪接收

- 有时二级路由被当做参数处理，在嵌套路由加参数会出现这种情况
- 二级路由和传递参数有冲突，因为它们都是以路径作为导航形式处理的
- 解耦的方法：可以把路由传参当成props来进行传参

## 用于组件内部的钩子
```js
// 从另外的组件进入该组件前触发该钩子
beforeRouteEnter(to, from, next) {
    console.log("todo before enter")
    console.log(this) // 这里获取不到上下文
    next(vm => { // next里面有一个回调函数可以获取到上下文，把请求到的数据塞到vue对象中
        console.log(vm)
    });
},
// 同一个组件，不同的是触发,常用于同一个组件，当传入不同参数时，展示不同的数据
beforeRouteUpdate(to, from, next){
    console.log("todo update enter")
    next();
},
// 该组件离开跳转到另外的组件时触发该钩子,常应用于用户表单，当用户填了内容点击提交时，需要提醒用户是否离开页面
beforeRouteLeave(to, from, next){
    console.log("todo leave enter")
    if(global.confirm('are you sure')){
        next();
    }
},
```

# Vue 中 this.$router 与 this.$route 的区别 以及 push() 方法

## 通过注入路由器，我们可以在任何组件内通过 this.$router 访问路由器，也可以通过 this.$route 访问当前路由
- this.$router 相当于一个全局的路由器对象，包含了很多属性和对象（比如 history 对象），任何页面都可以调用其 push(), replace(), go() 等方法。
this.$route 表示当前路由对象，每一个路由都会有一个 route 对象，是一个局部的对象，可以获取对应的 name, path, params, query 等属性。

## 关于 push() 方法：
- 想要导航到不同的 URL，则使用 router.push 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。
- 当你点击 <router-link> 时，这个方法会在内部调用，所以说，点击 <router-link :to="..."> 等同于调用 router.push(...)。

### push() 方法的调用：

```js
//字符串
this.$router.push('home')

//对象
this.$router.push({path:'home'})

//命名的路由
this.$router.push({name:'user', params:{userId: '123'}})

//带查询参数，变成 /register?plan=private
this.$router.push({path:'register', query:{plan:private}})
```
> 注意：如果提供了 path，params 会被忽略，上述例子中的 query 并不属于这种情况。
  取而代之的是下面例子的做法，你需要提供路由的 name 或手写完整的带有参数的 path：

```js
const userId = '123';

this.$router.push({path:`/user/${userId}`});  //->/user/123

this.$router.push({name:'user', params:{userId}});  //->/user/123

//这里的 params 不生效
this.$router.push({path:'/user', params:{userId}});
```

> 同样的规则也适用于 router-link 组件的 to 属性。
  总结：params 传参，push 里面只能是 name:'xxx'，不能是 path:'/xxx'，因为 params 只能用 name 来引入路由，
  如果这里写成了 path ，接收参数页面会是 undefined。
 
### 路由传参的方式：

```js
// 1、手写完整的 path:
this.$router.push({path: `/user/${userId}`});
// 获取参数：this.$route.params.userId

// 2、用 params 传递：
this.$router.push({name:'user', params:{userId: '123'}});
// 获取参数：this.$route.params.userId
// url 形式：url 不带参数，http:localhost:8080/#/user

// 集成的params传递
  this.$router.push({
    name: 'large',
    params: {
      ...this.$route.params,
      processId: process.id,
    },
  });

// 3、用 query 传递：
this.$router.push({path:'/user', query:{userId: '123'}});
// 获取参数：this.$route.query.userId
// url 形式：url 带参数，http:localhost:8080/#/user?userId=123

// 集成的query传递
  this.$router.push({
    path: '/large',
    query: {
      ...this.$route.query,
      processId: process.id,
    },
  });
```

> 直白的说，query 相当于 get 请求，页面跳转的时候可以在地址栏看到请求参数，params 相当于 post 请求，参数不在地址栏中显示。
  要注意，以 / 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。


