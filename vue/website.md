### 简单说下vue

#### 官网解释
vue 是一套用于用户界面的渐进式框架；
vue被设计为可以自底向上逐层应用；
vue 的核心库只关注视图层，不仅易于上手，还便于第三方库或者其他项目整合；
另一方面，与现代化工具链以及各种支持类库相结合使用，vue也完全能够为复杂的单页面应用提供驱动

#### 什么是渐进式
渐进式可以理解为一步一步的意思，可以根据vue和vue全家桶理解为，vue本身的声明式渲染，组件系统，vue-router,vuex以及vue-cli工具等等，这些东西都有，但是你可以不用vue-router,vuex的状态；
声明式渲染和组件系统是vue的核心库，而vue-router,vuex，vue-cli是为了方便vue开发的解决方案，这些方案相对独立，可以应用在项目上，也可以不用

#### 什么是自底向上逐层应用
由基层开始做起，把基础的东西写好，在逐层向上添加功能和效果

#### Vue中的MVVM
##### 什么是MVVM
MVVM是Model-View-ViewModel的缩写，它是一种给予前端开发的架构模式，其核心是提供对View和Model的双向数据绑定，使得Model的状态改变可以自动传递给View,既为双向数据绑定
##### Vue实现的VM
Vue.js就是借鉴了MVVM的风格进行双向数据绑定Javascript库，专注于View层，它的核心就是MVVM中的VM，主要是为了链接View和Model,保证数据的一只性，Vue会通过DOM listeners 来监听和改变Model层的数据，反之当Model层的数据发生改变时，也可以通过data bingings来监听并改变View层展示，从而实现双向数据绑定的功能

#### 声明式渲染
Vue.js的核心式一个允许采用简洁的模版愈发来声明式的将数据渲染进DOM的系统
```````
<div id="app">
  {{ message }}
</div>
```````
```````
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```````
```````
Hello Vue!
```````
##### vue响应式如何实现的？
原理图   
![code](../images/vue-data.png)
- 监听数据变化，知道变量在何时更改的
  - vue2.0 使用object.defineProperty
    - 将data中属性遍历一遍转换为getter/setter,并在getter的函数中将使用函数的上下文进行收集，我们称之为依赖收集的过程；在setter函数中修改这个变量的时候，会触发通知依赖更新的过程
  - vue3.0 使用Proxy
    - 对Data进行一次代理，在代理过程中实现我们的依赖收集和依赖通知的操作

##### Vue的更新队列
原因：主要是出于对模版渲染的性能优化
###### 暴露出来的api nextTick
必须在模版更新或子组件更新之后处理