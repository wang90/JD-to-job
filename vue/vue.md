### vue 与 react 的区别
#### 监听数据变化的实现原理不同
- Vue 通过 getter/setter 以及一些函数的劫持，能精确知道数据变化，不需要特别的优化就能达到很好的性能
- React 默认是通过比较引用的方式进行的，如果不优化（PureComponent/shouldComponentUpdate）可能导致大量不必要的VDOM的重新渲染
#### 数据流的不同
![code](../images/vue&react数据流.png)
#### HoC 和 mixins
- Vue 中我们组合不同功能的方式是通过 mixin
- React中我们通过 HoC (高阶组件）
#### 组件通信的区别
![code](../images/vue&react组件通信.png)
#### 模板渲染方式的不同
- React 是通过JSX渲染模板
- Vue是通过一种拓展的HTML语法进行渲染
#### Vuex 和 Redux 的区别
- 在 Vuex 中，$store 被直接注入到了组件实例中，因此可以比较灵活的使用：
  - 使用 dispatch 和 commit 提交更新
  - 通过 mapState 或者直接通过 this.$store 来读取数据
- 在 Redux 中，我们每一个组件都需要显示的用 connect 把需要的 props 和 dispatch 连接起来。

### Vue中的key
首先key的主要作用高效的更新我们的虚拟dom
可以通过源码的方式去解释，在patch的过程中会执行patchVnode,patchVnode的过程中会执行updateChildren的方法会更新所有的两个新旧子元素，那么在这个过程中通过这个key可以精准的判断，我当前在循环这两个元素节点是不是同一个节点，如果没有加key永远都是一个相同的节点，能做的操作就是强硬的去更新，那么在这个过程中，我们没有办法避免这些频繁的更新操作，所以会额外多做很多dom操作，不加key的性能就会很差，如果加上key,可以通过利用内部的优化的算法操作，例如猜测首位结构的相似性，那么由于大部分情况下元素都不会发生大量的位置上的变化，所以会很高效结束这个循环，会减少大量的更新过程，操作就会非常高效。
