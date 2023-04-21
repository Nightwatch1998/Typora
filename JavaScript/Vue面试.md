## 什么是MVVM、MVC？

### 什么是MVVM？

Model:模型代表真实状态内容的领域对象模型，内容的数据访问层

View:视图使用户在屏幕上看到德结构、布局和外观

ViewModel：视图模型，绑定器在视图和数据绑定器之间进行通信；渲染页面，监听页面变化

优点：低耦合、可重用、独立开发

### MVC与MVVM区别

- MVC的controller演变成了viewmodel
- MVVM通过数据来显示视图层，而不是DOM操作
- 主要解决了MVC中大量的DOM操作使页面渲染性能降低，加载速度变慢

## 为什么data是一个函数

组件的data写成一个函数，数据以函数值形式返回。这样每复用一次，类似于给组件分配了一个私有的数据空间，让各个组件实例维护各自的数据。

如果是一个对象的话，组件实例化的时候会被共享，导致数据干扰

## Vue组件通讯方式

- props和$emit。父传子通过props传递，子传父通过$emit触发事件实现
- $parent 和 $children。$parent获取父组件，$children获取子组件列表
- $attrs 和 $listeners。$attrs传递非prop属性, $listeners传递父组件的事件；<button v-bind="$attrs" v-on="$listeners">
- provide和inject。可以从祖先传递到子孙组件
- $refs获取组件实例
- vuex状态管理

## Vue生命周期有哪些？

数据观测data observe是指vue如何监听组件中的数据变化，通过Object.defineProperty和Proxy两种技术实现

vue中每个组件有一个与之关联的watcher，它会监听组件的数据变化。数据发生变化时，vue会通知与之关联的watcher重新计算组件中的数据，并更新相应的视图。

- beforeCreate:在实例化之后，数据观测和event/watcher事件配置之前。**初始化数据、引入插件**
- created:实例创建完成后被调用，已完成**数据观测**、属性和方法的运算、watch/event事件回调。**数据获取、初始化事件**
- beforeMount:挂载之前，相关的render函数首次调用。**对模板进行最后修改**
- mounted:挂载之后，**真实的DOM挂载完毕**，数据完成双向绑定，可以访问得到DOM节点。**进行dom操作、调用第三方库等**
- beforeUpdate:数据更新时调用，发生在虚拟DOM重新渲染之前。在这个钩子中进一步更改状态，不会触发重新渲染过程
- updated:更新完成之后，当前的DOM已经更新完成，需要避免在此期间更新数据。
- beforeDestory:实例销毁之前，可以进行善后工作，例如清除定时器
- destoryed：Vue实例被销毁后，vue实例指向的东西会被解绑，事件监听器会被移除，子实例会被销毁。

异步请求可以在created、beforeMount、mounted中进行，这时data已经创建

不依赖dom时建议在created中使用，依赖的话建议在mounted中使用。

## v-if和v-show的区别

- v-if在编译过程会被转化为三元表达式，条件不满足时不渲染。销毁重建控制是否显示
- v-show会被编译为指令，条件不满足时将节点隐藏。**适用于需要频繁切换的场景**

扩展补充:

- display:none  隐藏后不占位置，不会被子元素继承，无法触发事件
- visibility:hidden无法触发事件
- opacity:0可以触发事件，过渡动画有效

## vue的内置指令

- v-once 仅渲染组件一次，并跳过之后的更新

- v-bind 绑定属性

- v-on 绑定事件

- v-cloak 解决初始化慢到页面闪动的问题

- v-html 将html字符串转化为真正的html元素，有可能导致XSS

- v-show 通过display进行显示隐藏

- v-if/v-else/v-else-if 条件渲染

- v-for 比v-if优先级高

- v-model 表单和实例数据双向绑定

  

## 怎样理解Vue的单项数据流

数据总是从父组件传递到子组件，子组件没有权利修改父组件传来的数据，只能请求父组件对原始数据进行修改

实现：

- 利用get set方法
- watch监听数据变化，触发$emit函数

## vue2 响应式数据的原理

整体思路是**数据劫持+观察者模式e**

1. vue2的响应式数据基于Object.defineProperty()实现。当一个Vue实例被创建时，Vue会递归遍历数据对象，为每个属性添加getter和setter方法。
2. 其中getter会返回属性的值，而setter方法会在属性值被修改时触发。在setter方法中，vue会通知所有的watcher对象，让他们更新视图，从而实现响应式变化。
3. Object.defineProperty只能劫持对象的属性，如果要监听数组或对象内部的变化，则需要对每个元素进行遍历。

具体的架构：

- Observer 劫持所有监听属性，属性变化了要通知Watcher
- Dep 订阅器，因为订阅者有多个，因此需要专门收集订阅者。在监听器Observer和订阅者Watcher之间**统一管理**
- Watcher 观察者，添加订阅者，通知更新视图
- Compile 指令解析器，对每个节点进行扫描和解析，将相关指令初始化成一个Watcher。并且初始化模板数据
- Updater 更新视图

## Vue如何检测数组变化

vue将data中的数组进行了原型链重写，指向了自己定义的数组原型方法。这样可以**通知依赖更新**，如果数组中包含着引用类型，会对数组的引用类型**再次递归便利进行监控**。这样就实现了监控数组变化。

有两种情况无法检测数组变化：

- 当利用索引直接设置一个数组项。解决办法使用vm.$set方法
- 当修改数组的长度时

```js
//使用该方法进行更新视图
 
// vm.$set，Vue.set的一个别名
 
vm.$set(vm.items, indexOfItem, newValue)
```

## Vue的父子组件生命周期钩子函数执行顺序

**加载渲染过程**

父beforeCreate->父created->父beforeMount->子beforeCreated->子created->子beforeMount->子mounted->父mounted

**子组件更新过程**

父beforeUpdate->子beforeUpdate->子updated->父updated

**父组件更新过程**

**销毁过程**

除了创建，挂载、更新、销毁都要等到子组件完成，父组件才能进行

## v-model双向绑定原理

v-model本质就是value+input方法的语法糖，根据不同的标签生成不同的事件和属性

- text/textarea输入框：value属性和input事件
- checkbox复选框：checked属性和change事件
- select单选框:value和change事件

```js
<input type="text" v-model="message"> <!-- 文本输入框 -->
<input type="checkbox" v-model="checked"> <!-- 复选框 -->
<input type="radio" v-model="picked" value="A"> <!-- 单选框 -->
```

## Vue3响应式原理

使用Proxy的原因：

- Object.defineProperty()无法监控数组的下标变化，数组下标添加元素不能实时响应。Proxy可以拦截对象的所有操作，监听数组的变化、动态添加属性。
- Object.defineProperty()对每个属性单独劫持，属性较多时影响性能。Proxy可以劫持整个对象，并返回一个新对象。

问题：

- Proxy只会代理对象的第一层,通过reactive实现

```js
function reactive(obj) {
  if (typeof obj !== 'object' || obj === null) { //判断是否为object
    return obj;
  }

  return new Proxy(obj, {
    get(target, key, receiver) {
      const result = Reflect.get(target, key, receiver);
      track(target, key); // 收集依赖
      return reactive(result); // 递归代理嵌套对象
    },
    set(target, key, value, receiver) {
      const oldValue = target[key];
      const result = Reflect.set(target, key, value, receiver);
      if (oldValue !== value) {
        trigger(target, key); // 触发更新
      }
      return result;
    },
    deleteProperty(target, key) {
      const result = Reflect.deleteProperty(target, key);
      if (result) {
        trigger(target, key); // 触发更新
      }
      return result;
    },
  });
}

```

## vue2、vue3渲染器的diff算法(看源码补充)

**diff算法：**

- 先同级比较，再比较子节点
- 先判断一方有子节点另一方没有的情况
- 节点移除
- 比较都有子节点的情况
- 递归比较子节点

vue2：

- 双端比较，同时从新旧children的两端开始进行比较，节点相同，则更新，不同则比较子节点。时间复杂度O(n)

vue3:

- 基于Map数据结构，可以更快地进行比较。查找节点时间复杂度O(1)

## hash模式和history模式的实现原理（补充）

hash值的的变化，**不会**导致浏览器向服务器发出请求。浏览器不发出请求，就不会刷新页面。前端路由器通过监听**hashchange**监听hash发生了哪些变化，根据变化实现更新。

history模式的实现主要是依据pushState和replaceState两个api，监听**url**来实现页面更新。URL路径变化会触发浏览器向服务器发送请求。

区别：

- hash模式的url有“**#**”，“**#**”后面部分表示应用程序的路由路径；history没有
- **刷新页面时**hash模式可以正常加载页面。history没有处理的话，会返回404，它需要后端支持
- **兼容性上**，hash可以支持低版本浏览器和IE

## vue-router路由钩子函数是什么，执行顺序？

全局守卫、路由守卫、组件守卫

- beforeEach:路由切换之前触发。**检测用户是否登录。**
- beforeResolve：路由切换**完成前**触发。获取数据，确保所有数据都已经加载。
- afterEach:路由切换后，**执行一些全局操作**
- beforeEnter:路由进入前，可以检测路由参数是否有效
- beforeRouteUpdate：路由更新前，**可以处理参数变化时需要更新的数据**，只会在当前组件复用时触发
- beforeRouteLeave: 在路由离开之前触发

完整的导航解析流程：

1. 导航触发

2. 在失活的组件里调用 **beforeRouterLeave** 守卫√

3. 调用全局的**beforeEach**守卫√

4. 在重用的组件之前调用**beforeRouteUpdate**，同样是旧有的

5. 路由配置里面调用**beforeEnter**

6. 解析异步路由组件

7. 被激活的组件里调用**beforeRouteEnter**√

8. 调用全局的**beforeResolve**√

9. 导航被确认

10. 调用全局**afterEach√**

11. 调用 beforeRouterEnter 守卫中传给 **next** 的回调函数，创建好的组件实例会作为回

    调函数的参数传入

## vue-router动态路由是什么？

需要把某种模式匹配到的所有路由，映射到一个组件。可以使用动态路径参数实现。**路径参数用冒号：表示**,可以有多个路径参数，会映射到$route.params上的对应字段

组件复用导致路由参数失效怎么办？因为不会触发生命周期函数

- 通过watch监听路由参数，再发起请求

```js
1 watch：{
2 	"router":function(){
3 		this.getData(this.$router.params.xxx)
4 	}
5 }
```

- 用key来阻止复用

```
router-view :key="$route.fullPath"
```

## 对vuex的理解

vuex是专门为vue开发的状态管理器，采用集中式存储管理应用中所有组件的状态。

组件创建或者销毁时，组件会被初始化，导致data也会被销毁。

主要模块：

- State：定义了应用状态的数据结构，可以初始化默认状态
- Getter:允许组件从store中获取数据，mapGetters辅助函数将state中的getter映射为**局部计算属性**
- Mutation：同步更改store
- Action：用于提交mutation，可以异步进行
- Module：允许将单一的Store拆分为多个store

## Vuex页面刷新丢失数据怎么解决？

需要做vuex**数据持久化**，一般使用本地存储的方案。保存在cookie或者localStorage中

vuex-persist持久化插件

## vue中使用了哪些设计模式？

- **观察者模式**：响应式数据原理
- **单例模式**：整个程序只有一个vue的根组件、vuex的store、vue-router的每个路由
- **发布订阅模式**：事件机制，多了消息代理和事件通道
- 装饰器模式：@装饰器的用法
- 策略模式：对象的行为在不同的场景有不同的实现方案
- 工厂模式：传入参数即可创建实例，虚拟
- **代理模式**：vue3的proxy

## Vue性能优化（重点）

- 对象层级**不要过深**，否则影响性能、可读性和可维护性
- **不需要响应式的数据**不要放在data中，可以使用Object.freeze()冻结
- v-if和v-show区分场景使用，v-if用于**不经常切换的场景**，v-show适用于经常切换的场景
- v-for 遍历必须加key，最好是id值，**避免同时使用v-if**
- computed和watch区分场景使用；computed适用于**计算成本较高**的场景，例如**数组排序、过滤**等；适用于需要在数据变化时**执行异步操作或者开销较大的操作**时。
- 大数据列表和表格性能优化，虚拟列表，虚拟表格，**渲染可视区域内的数据**。
- 防止内部泄露，组件销毁后把全局变量销毁，例如解除对**window和document**的引用
- 图片懒加载，用户浏览到可视区域时再加载图片
- 路由懒加载，**动态导入**const UserDetails = () => import('./views/UserDetails.vue')
- 第三方插件按需加载，tree-shaking机制
- 适当采用keep-alive缓存组件
- 防抖、节流的运用
- 服务端渲染SSR和预渲染

## nextTick作用是什么？实现原理？

vue更新DOM是异步更新的，数据变化，DOM更新不会马上完成。

nextTick能够在**下一次DOM更新循环结束后执行回调函数**，以确保DOM已经被更新。

实现原理：宏任务和微任务(优先级较高)

- **Promise**：将函数延迟到函数调用栈最末端，**微任务**
- **MutationObserver**：监听DOM节点变动，所有DOM变动完成之后，执行回调函数。
- 以上都不行则采用**SetTimeout**把函数延迟到DOM更新之后再使用。**宏任务**

执行时机：

- 宏任务包括：script整体代码、setTimeout、setInterval、setImmediate、IO操作、UI渲染等
- 微任务包括：Promise.then()、Promise.catch()、Promise.finally()、MutationObserver等等

## keep-alive使用场景和原理

- keep-alive是vue的内置组件，用于缓存内部组件的实例。**避免创建组件带来的开销，保留组件状态。**
- keep-alive具有**include**和**exclude**属性，控制哪些组件进入缓存，设置max属性，当缓存实例超过该数时vue会移除最久没有使用的组件缓存。
- 具体的实现上，内部维护了一个**key数组**和**一个缓存对象**，key会自动生成。
- keep-alive内部所有的嵌套组件都具有两个生命周期钩子函数，**activated**在首次挂载和从缓存中获取时触发，**deactivated**在从DOM移除进入缓存，以及组件卸载时触发

```js
<!-- 以英文逗号分隔的字符串 -->
<KeepAlive include="a,b">
  <component :is="view" />
</KeepAlive>

<!-- 正则表达式 (需使用 `v-bind`) -->
<KeepAlive :include="/a|b/">
  <component :is="view" />
</KeepAlive>

<!-- 数组 (需使用 `v-bind`) -->
<KeepAlive :include="['a', 'b']">
  <component :is="view" />
</KeepAlive>
// a,b为组件名称 

```

## Vue.set方法原理

两种情况下修改Vue不会触发视图更新：

1. 实例创建后，给实例添加新的属性
2. 直接更改数组下标修改数组的值

Vue.set用于给响应式对象添加新的属性，以便能够响应式更新视图。对新的属性进行响应式跟踪，触发对象的dep收集到的watcher去更新

```
// 两种方式
Vue.set(object, propertyName, value)
this.$set(this.obj, 'key', value)
```

