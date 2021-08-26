# Dry Goods Collection

## axios
```js
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
config.headers.authorization = token;

axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
config.headers['Content-Type'] = 'application/x-www-form-urlencoded';
```

## 立刻执行调用
```javascript
async viewBusinessRecord(row) {
  await (() => this.curRecord = row)()
  this.$refs.reviewRecordDialog.open();
}
```

## 正则校验
```js
(wx[0-9]{6,})|(00[0-9]{6,})|(300[0-9]{5,})
```

## 编码和解码
1. encodeURI、encodeURIComponent
2. decodeURI、decodeURIComponent

## vue中key的作用和工作原理
1. key的作用主要是为了高效的更新虚拟DOM,其原理是vue在patch过程中通过key可以精准判断两个节点是否是同一个，从而避免频繁更新不同元素，使得整个patch过程更加高效，减少DOM操作量,提高性能。
2. 另外,若不设置key还可能在列表更新时引发一些隐蔽的bug
3. vue中在使用相同标签名元素的过渡切换时,也会使用到key属性,其目的也是为了让vue可以区分它们，否则vue只会替换其内部属性而不会触发过渡效果。
4. 带上原理解释一遍：
- 源码在patch的过程中，会执行patchVnode，patchVnode过程中会执行updateChildren这个方法，他会更新所有的两个新旧的子元素；
  那么在这个过程中，通过key就可以精准的判断，当前在循环的这两个节点是不是一个节点。
- 如果没有加key的话，永远会认为是相同的节点，所以能做的操作只有强硬的去更新；
  这样的话在这个过程，就没有办法避免频繁的更新过程，需要额外做很多dom操作。不加key的性能就会很差
- 如果加上key，就可以通过一系列内部优化算法，比如说猜测首尾结构的相似性，由于大部分情况下，元素不会发生大量的位置变化，
  所以会很高效的结束循环，减少大量的更新过程。
  
## computed vs methods
- 可以使用 methods 来替代 computed，效果上两个都是一样的，但是computed基于它的依赖缓存，只有相关依赖发生改变时才会重新取值；
- 而使用methods，在重新渲染的时候，函数总会重新调用执行。

## 作用域插槽
```html
子组件
<span>
  <slot :user="user">
    {{ user.lastName }}
  </slot>
</span>
```
```html
父组件
<current-user>
  <template v-slot:default="{user}">
    {{ user.firstName }}
  </template>

<!--    旧语法-->
  <template slot="default" slot-scope="{user}">
    {{ user.firstName }}
  </template>
</current-user>
```

let sourceItem = '258'
let returnShowType = undefined == null && sourceItem
returnShowType

```js
// Promise.then 成功的情况 对应await
// Promise.then 等价于 await Promise

// Promise.catch 异常的情况 对应try...catch
async function test6() {
  const p6 = Promise.reject(6)
  p6.catch(e => {
    console.error('err1', e)
  })
   try{
     const data6 = await p6
     console.log('data6', data6)
   }catch(e) {
     console.error('err2', e)
   } 
}
test6()
```
## session與cookie
- 前後端不分離的情況下，使用session驗證是爲了防御post攻击数据库查询
### 前后端分离的意义
1. 让项目逃离浏览器，不再依赖WEB

### OAuth2.0的授权实现
- 目的：前端分离出来之后，后端作为一个独立的服务，提供更安全的用户验证

## CSS 样式优先级
### CSS 的继承性
- CSS 优先规则1： 最近的祖先样式比其他祖先样式优先级高。
- CSS 优先规则2："直接样式"比"祖先样式"优先级高。
### 选择器的优先级
- CSS 优先规则3：内联样式 > ID 选择器 > 类选择器 = 属性选择器(a[href="segmentfault.com"]{}) = 伪类选择器(:hover{}) > 标签选择器 = 伪元素选择器(::before{}) > 通配选择器(*{})
- CSS 优先规则4：计算选择符中 ID 选择器的个数（a），计算选择符中类选择器、属性选择器以及伪类选择器的个数之和（b），计算选择符中标签选择器和伪元素选择器的个数之和（c）。
  按 a、b、c 的顺序依次比较大小，大的则优先级高，相等则比较下一个。若最后两个选择符中 a、b、c 都相等，则按照"就近原则"来判断。
- CSS 优先规则5：属性后插有 !important 的属性拥有最高优先级。若同时插有 !important，则再利用规则 3、4 判断优先级。
#### 所有 CSS 的选择符由上述 7 种基础的选择器或者组合而成，组合的方式有 3 种：
1. 后代选择符： .father .child{}
2. 子选择符： .father > .child{}
3. 相邻选择符: .bro1 + .bro2{}
