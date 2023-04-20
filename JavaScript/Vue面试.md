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

## vue2、vue3渲染器的diff算法



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
