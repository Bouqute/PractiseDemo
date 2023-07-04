ES6


## 什么是react
react 网页UI框架，通过组件化的方式解决视图层面开发复用的问题，本质是一个组件化框架，它的核心设计思路有三点，分别是声明式/组件化与通用性。
1. 声明式的优势在于直观与组合
2. 组件化的优势在于视图的拆分与模块复用，可以更容易做到高内聚低耦合。
3. 通用性 一次学习，随处编写。

## 为什么使用jsx
解释JSX
官方解释：是一个js的语法拓展或者说是类似于XML的ECMAScript的语法拓展

核心概念
是CreateElement 的语法糖

方案对比
模版弱关注度分离。引入概念多
模版字符串 结构描述复杂 语法提示差
JXON 语法提示差


React JSX to JX

babel编译流程
parse: 通过 parser 把源代码转换成 AST
transform：通过遍历 AST，调用各种插件对 AST 进行转换，包括一些语法转换，代码优化等，最终生成新的 AST
generate: 把转换后的 AST 打印成目标代码，并生成 sourcemap



React 的 JSX 会被 Babel 的 @babel/plugin-transform-react-jsx 插件转换成 React.createElement 方法调用，这个插件的核心就是通过 visitor 函数来遍历 AST，然后对不同类型的节点进行处理，比如 JSXElement，JSXFragment 等，最后将 JSX 转换成 React.createElement 方法调用，得到一个由 createElement fn 组成的 AST，最后再在 generate 阶段完成 AST 到 JS 的转换


## 生命周期


#### 挂载阶段
1. constructor()
>在 React 组件挂载之前，会调用它的构造函数

2. getDeriverStateProps()
>getDerivedStateFromProps 会在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用，它应返回一个对象来更新 state.(这里是静态方法，代表你不能访问 this，使得 getDerivedStateFromProps 这个函数强迫变成一个纯函数，逻辑也相对简单，就没那么多错误了)

3. Render()
>渲染函数 render() 被调用

4. componentDidMount() 组建完成
>会在组件挂载后（插入 DOM 树中)立即调用。依赖于 DOM 节点的初始化应该放在这里。如需通过网络请求获取数据，此处是实例化请求的好地方
getDerivedStateFromProps

#### 更新阶段
1. static getDerivedStateFromProps()
2. shouldComponentUpdate()
 >根据 shouldComponentUpdate() 的返回值，判断 React 组件是否受当前 state 或 props 更改的影响。默认父组件的 state 或 prop 更新时，无论子组件的 state、prop 是否更新，都会触发子组件的更新，这会形成很多没必要的 render，浪费很多性能，使用 shouldComponentUpdate 可以优化掉不必要的更新
3. render()
4. getSnapshotBeforeUpdate()
>getSnapshotBeforeUpdate() 在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给 componentDidUpdate()
5. componentDidUpdate()
componentDidUpdate() 会在更新后会被立即调用。

#### 卸载阶段
1. componentWillUnmount()
    >会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除 timer 等。

#### 错误处理
1. static getDerivedStateFromError()
    >此生命周期会在后代组件抛出错误后被调用。 它将抛出的错误作为参数，并返回一个值以更新 state
2. componentDidCatch()
    >此生命周期在后代组件抛出错误后被调用.    



### 什么情况下会重新渲染

#### 函数组建
函数组建任何情况下都会重新渲染。

#### React.Components
不实现shouldComponentUpdate函数，有两种情况出发重新渲染
1. 当state发生变化时
2. 当父级组建的Props传入时

#### React.PureComponent
默认实现了shouldComponentUpdate函数，仅在props与state进行浅比较后。确认有变更时候才会出发重新渲染

### react 请求应该放在哪里

对于异步请求，应放在 componentDidMount 中操作从时间顺序看，除componentDidMount还可以有以下选择:
constructor:可放，从设计言不推荐，主要用于初始化 state 与函数绑定，不承载业务逻辑且随着类属性流行，constructor 已很少用
componentWillMount:已被标记废弃，在新的异步渲染架构下会触发多次渲染，易引发 Bug，不利未来 React 升级后的代码维护


# 类组建与函数组建
共同点： 使用方式/表达效果完全一致

不同点：
1. 心智模型：类使用OOP 函数组建使用 FP
2. 使用场景 生命周期
3. 独有功能 生命周期
4. 设计模式 继承
5. 未来趋势 Hooks

# 如何设计React组建




1.React 中 keys 的作用是什么？ 
>用来追踪哪些列表的元素被修改，被添加或者是被删除的辅助标示。 在开发过程中我们需要保证某个元素的key在其同级元素中具有唯一性。 在react的diff算法中react会借助元素的key来判断该元素是最新创建的还是被移动而来的，从而减少不必要的元素渲染。



2.React 中 refs 的作用是什么？ 
>允许我们访问DOM 节点或在render 方法中创建的React 元素

3.React 中有三种构建组件的方式
>函数式创建
通过React.createClass 方法创建
继承React.Component 创建   
4.调用 setState 之后发生了什么？
> React 会重新渲染组件，并根据新的状态重新计算组件的视图


5.react diff 原理（常考，大厂必考）
> 概念： 需要遍历整棵树的节点然后进行比较，是一个深度递归的过程
> react diff 优化策略？
> 1. DOM节点跨层级的操作不做优化，因为很少这么做，这是针对的tree层级的策略；
> 2. 对于同一个类的组件，会生成相似的树形结构，对于不同类的组件，生成不同的树形结构，这是针对conponent层级的策略；
> 对于同一级的子节点，拥有同层唯一的key值，来做删除、插入、移动的操作，这是针对element层级的策略；


6.为什么建议传递给 setState 的参数是一个 callback 而不是一个对象？
> 因为props和state可能会异步更新，也就是说，对相同的变量进行处理的时候，会将这多次处理合并为一个，这个是批处理；而如果传入函数，那么会进行链式调用，这个函数会被react加入到一个执行队列中，函数中的代码会依次执行。


7.除了在构造函数中绑定 this，还有其它方式吗？
> 1. 函数定义的时候使用箭头函数
> 2. 函数调用是使用bind绑定this 


8.setState第二个参数的作用？
>第二个参数是一个回调函数，在setState的异步操作结束并且组件已经重新渲染的时候执行。也就是说，我们可以通过这个回调来拿到更新的state的值。

9.(在构造函数中)调用 super(props) 的目的是什么 ?
> 子类没有this，必须要有super，初始化 this，可以绑定事件到 this 上
> constructor(props) 方法中 传入的参数 props 为对象，所以 super(props) 的作用就是在父类的构造函数中给 props 赋值一个对象 this.props = props 这样就能在它的下面定义你要用到的属性了

10.简述 flux 思想    
> 一套基于dispatcher的前端应用架构模式
> 1. 用户访问view
>2. view发出用户的Action
>3. dispatcher收到Action，要求Store进行响应的更新
>4. Store更新后，发出一个"change"事件
>5. view收到"change"事件后，更新页面

11.在 React 当中 Element 和 Component 有何区别？    
>Component 就是指class component 或者function component，也就是我们经常写的react 组件。 Element 其实指的是Component 的返回值，就是一个javascript 的对象（虚拟dom 对象？）。 也就是我们经常说的dom 的描述对象。
>

12.描述事件在 React 中的处理方式。
> 使用方法：
> 使用方法跟原生的DOM很类似，只不过使用驼峰写法来命名事件类型
> 直接讲函数的声明当成事件句柄
> 将react这套事件机制成为`SyntheticEvent`,叫 **合成事件**
> react事件传播方式为冒泡，如果需要换成事件捕获方式需要在事件后面加上一个`Capture`
>在合成事件中，所有事件都绑定在`document`元素上 react17版本之前，后面使用root元素。
> 需要阻止事件向上传播，使用`e.stopPropagation()`方法阻止事件流向上传播，但是并不难真正的阻止在原生Dom元素上的传播，只是对合成事件中的事件流有效！
>使用事件合成的目的：
> 1. 进行浏览器兼容，实现更好的跨平台；React提供的合成事件可用来抹平不同浏览器事件对象之间的差异，将不同平台事件模拟合成事件。
> 2. 避免垃圾回收；React事件对象不会被释放掉，而是存放进一个数组中，当事件触发，就从这个数组中弹出，避免频繁地去创建和销毁（垃圾回收）。
> 3. 方便事件统一管理和事务机制。
>**React合成事件原理**

>1. 事件绑定
>事件插件（EventPlugin）的模块产生关联的，每个插件只处理对应的合成事件
>2. 事件触发
>当任意事件触发都会执行dispatchEvent函数

13.createElement 和 cloneElement 有什么区别？    


14.如何告诉 React 它应该编译生产环境版本？    
15.Controlled Component 与 Uncontrolled Component 之间的区别是什么？



16.React Hook优点？为什么要用 hook？

> 最大的优点： 更容易复用代码
> 1. 每调用useHook一次都会生成一份独立的状态，这个没有什么黑魔法，函数每次调用都会开辟一份独立的内存空间。
> 2. 组件树层级变浅。在原本的代码中，我们经常使用 HOC/render props 等方式来复用组件的状态，增强功能等，无疑增加了组件树层数及渲染，而在 React Hooks 中，这些功能都可以通过强大的自定义的 Hooks 来实现。
> 不用再去考虑 this 的指向问题。在类组件中，你必须去理解 JavaScript 中 this 的工作方式

高阶组件缺点：
>1. 来源不清晰,高阶组件是通过增强组件的props（赋予一个新的属性或者方法到组件的props属性）， 实现起来比较隐式。你难以区分这个props是来自哪个高阶组件。
>2. 高阶组件需要实例化一个父组件来实现，不管是在代码量还是性能上，都不如hooks。
>3. 依赖不清晰：高阶组件对入参的依赖是隐式的，入参发生在看不到的上层的高阶组件里面。
>4. 命名冲突：高阶组件太多时，容易发生命名冲突。



# useState工作的过程?
>1. 首次调用函数组件，执行useState，React获取到当前正在遍历的节点，创建_state对象挂载到节点上，根据调用useState的顺序，在数组中加入状态，并根据传入的初始值初始化状态。
> 2. 当调用状态的set方法时候，React修改_state上面的状态，并更新相应的组件
> 3. 后面再调用函数组件时候，useState找到对应的节点的_state，并根据useState的调用顺序，找到修改后的状态返回，这样组件就可以拿到正确的状态