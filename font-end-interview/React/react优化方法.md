1. 避免不必要的渲染
> shouldComponentUpdate() 

2.  Fragment
>Fragment可以避免不必要的dom节点。

3. 事件回调不使用匿名函数或者bind
>因为匿名函数的写法会在每次调用render时候都创建新的函数，而bind表达式也会在每次调用时候创建一个新的函数

4. 不要用内联样式
>因为React在解析JSX时候需要将style对象解析成css style字符串。更推荐将样式写在CSS中。

5. 不要在render中setState
>如果在render方法进行setState，可能导致循环地进行diff工作。

6. 优化条件渲染
>让条件分支中只包含需要改动的元素，不包含不需要改动的元素，防止diff子节点和更新节点时候增加不必要的操作，消耗性能。

● 懒加载（lazy）使用lazy实现按需加载组件和按需加载路由。
● 服务端渲染，使用服务端渲染提升首屏渲染性能。
● 缓存缓存cdn cdn缓存提升React资源的加载速度。
● 使用 prerender-spa-plugin 渲染首屏，基本原理是启动一个服务，用pupetter离屏渲染。这个原理和服务端渲染类似，但对代码改造更小，不过要求服务器有node环境。