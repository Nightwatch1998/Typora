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