## React
1.真实dam节点属性太多 影响计算性能<br>
2.`jsx`通过`babel-loader`  转化成虚拟dom对象。<br>
3.`React.createElement ` 根据传入的参数返回一个虚拟dom对象<br>
4.`React.cloneElement`  根据传入的参数 克隆一个element  返回 一个 虚拟dom<br>
5.经过babel编译会把`jsx`编译成React,createElement()函数执行<br>
6.把穿进去的虚拟dom节点转为真是dom节点<br>
7.并且在函数里面判断是否是首次渲染 首次渲染直接就更新 不会进入批量更新函数 <br>
8.`diff`的时候判断`type` 和`key` 都不一样直接 删掉 在更新新的<br>
9.同层比较 key和type相等 直接复用<br>
10.如果当前`diff`没有绑定key 就会默认使用当前层级的下标<br>
11.用链表来维护的`diff`计算的位置实际改变链表的指向就可以<br>
12.`useRef `在每一次hooks渲染中都会返回同一个ref对象 并且变更ref 并不会引发组件再次渲染<br>
13.t`his.forceUpdate()` 强制更新render 直接跳过shouldComponentUpdate比较 参数是更新完之后的callback<br>
14.`useCallback` 缓存一个函数<br>
15.useMemo, `useCallback` 可以使参数不会因为其他不相关的参数而发生变化  第二个参数数组里面可以传相关依赖 <br>
16.`react fiber` 是单链表结构只能获取下一个节点 不能获取上一个节点


## React.render 首次是怎么渲染dom的流程是什么
1.首先调用`render`的时候会先触发`legacyRenderSubtreeIntoContainer`函数在这个函数中会根据`_reactRootContainer`变量判断是否是首次渲染如果是第一次就正常进入渲染流程 <br>
2.之后会调用`legacy_renderSubtreeIntoContainer`会根据传入的节点例如 div 节点 或者 是functionComponent 以及classComponent 进行遍历操作 生成divDom标签进行节点插入最后就是进入`scheduleRootUpdate`进行dom更新 <br>
3.在插入root根节点之前会先清空一次root下的东西 所以你在index.html根目录root节点下写东西是没什么作用的这样的目的就是为了保证根目录下只有一个节点 <br>
4.如果是第二次进入的时候 会有根据变量`_reactRootContaine`判断是否已经进入过了如果渲染过一次了就直接走`legacy_renderSubtreeIntoContainer`进行调度更新


## React 机型diff计算的时候有几种节点更新替换的方式 
第一种首先是如果是有两个类型不同的节点直接会删掉更新 要是想复用可能会进行大量的遍历成本会更高<br>
第二种是跨层级比较也是直接删除掉在创建新的节点因为这个查询遍历起来时间复杂度更高以上两种方式都是不会就地复用节点的<br>
第三种情况就是在同级层次下key和type如果一样就可以就地复用




## hooks
### useLayoutEffect 是同步的useEffect是异步

1.useReducer和useState的区别是 前者适合用于处理复杂的逻辑状态，因为可以把状态reducer抽取到reducer中独立处理也方便复用，后者一般用于简单的状态<br>
2.useEffect和useLayoutEffect的区别是 后者的执行优先级是高于前者一般是dom更新完成之后同步执行，前者是在组件渲染到屏幕中以后延迟执行的 目的就是不会影响页面ui的渲染<br>
3.useContext、contextType、consumer的区别是。第一个是在hooks中使用的 第二个是在class类组件中使用的 第三个是两个中都可以使用


## React-router-dom
1.三种渲染方式 children ->component ->render渲染优先级<br>
2.Router外层包上Switch之后 每个路由就会独占一页<br>
### 不包Switch的情况下
1.children会和其他路由并存 不管path是否匹配<br>
2.render只有匹配才会渲染<br>
3.children和render只能接收回调函数<br>
4.component如果传入回调函数其他state更新就会频繁渲染


## redux-saga 和redux-thunk的区别
1.sage内部实现方式就是generator生成器内部会抛出put提交，call监听异步，fork直接执行，takeawy监听saga函数等方法<br>
2.thunk实际就是扩展一下dispatch的能力可以支持接收一个函数，因为在不用thunk之前只能接收对象所以thunk内部就是执行的普通函数


## react 17.0.1不需要在页面引入React
1.react 17.0以前 需要使用babel-transform-react-jsx编译jsx<br>
2.17.0使用的是automatic<br>
3.如果updateContainer 外面包裹一层unbatcheUpdates 就会隔绝批量更新操作<br>
4.fiber节点 原声标签 stateNode存储是dom节点 函数组件是null 类组件的存储的是类组件的实力因为他需要获取到里面的生命周期 state之类的<br>
5.$$typeof保证上线的安全性<br>
6.excutionContext标记如果是0就会立马更新 是0表示没有任务了可以直接更新了


## 自定义hooks
1.逻辑复杂 hooks复用逻辑 自定义hooks useEffect可以写多个<br>
2.useState返回数组 是因为 不能让返回的变量固定。<br>
3.useCallbacks缓存的是一个函数 在用于父子组件传回调函数的时候用的多<br>
4.useMemon缓存的是一个计算的值 根据依赖是执行 

## react diff
1.跨层级的、父节点不同 子节点相同的 都不会复用成本太高实际出现几率不高 直接干掉新建<br>
2.同层并且key 和type一样可以复用



### vue和react的diff算法，都是忽略跨级比较，只做同级比较。vue diff时调动patch函数，参数是vnode和oldVnode，分别代表新旧节点。


## Vue
1.vue比对节点，当节点元素类型相同，但是className不同，认为是不同类型元素，删除重建，而react会认为是同类型节点，只是修改节点属性 2. vue的列表比对，采用从两端到中间的比对方式，而react则采用从左到右依次比对的方式。当一个集合，只是把最后一个节点移动到了第一个，react会把前面的节点依次移动，而vue只会把最后一个节点移动到第一个。总体上，vue的对比方式更高效 


## Vue.use()内部做了什么 
1.内部执行`install`将插件挂载到全局
## 为什么要把router放到new Vue({})里面
1.以后再`vue`的组件中要使用`this.$router`所以要把router挂载到里面
## router-view做了什么

## current change => render() => vdom
2.监控change变化触发render返回一个虚拟dom<br>
3.`Vue.util.defineReactive()`设置一个对象的响应式<br>
4.`vue-router`需要vue的响应式的支持才能在hash改变的时候触发 组件内部的render函数更新<br>
5.响应式的原理说白了就是能感知数据的变化 触发使用组件的渲染函数重新执行

## methods computed 区别
我的理解是页面更新的 时候 如果计算属性依赖的值没有更新 那么计算属性里面的 函数就不会重新执行 但是 methods里放的普通方法 只要页面更新 无所谓依赖不依赖 都会执行


## vue页面初始化的时候为什么可以不掉用$mount?
1.在`_init`函数中做了兼容,如果调用vue构造函数的时候传了el就可以不用传因为默认内部会隐式调用`$mount`

## Vue3新增优化策略
1.静态节点提升<br>
2.精确找到需要更新的位置<br>
3.缓存事件处理程序<br>
4.区块 `block` 查找<br>
5.`vnode`上`有shapeFlag`标记为一个什么样的组件<br>
6.`patchFlag`标记动态内容的类型 为了在patch的过程中速度更快

## Vue3的优点
1.整体偏向纯函数风格<br>
2.以前挂载的静态方法现在都挂在到了实例上（`use、mixin、component、directive、nexttick、mount`等）[filter已经被废弃]<br>
3.更好的支持跨平台根据传入的配置项支持不同的平台（渲染器重写一下）<br>
4.createApp创建当前的应用程序防止有多余的参数<br>
4.1.例如 应用中有两个程序 以前的方法是在use上挂在方法 里面的程序不管是用没用到都会在里面产生方法而通过createApp挂载只会在当前程序中可以使用 很好的隔离了没有用到的方法属性等（使tree shaking更好的起作用）


## VueDiff流程

比较规则:
    1.根据key来判断 <br>
    2.判断节点type判断<br>
    3.节点上面相关属性判断<br>

1.两组新旧节点首先会首和首比较。<br>
2.两组新旧节点尾和尾在进行比较<br>
3.旧节点的首再和新节点的尾比较,比较规则同上但是会移动位置 会把当前的新节点放在老节点数组的尾部 (因为当前比较的新节点,在新节点数组中就在尾部)<br>
4.旧节点的尾再和新节点的首比较,比较规则同上也会移动位置 移动位置规则和第三条规则相反<br>
5.如果以上规则都不可以那么就只能老老实实的进行递归遍历了深度优先遍历同级进行比较<br>
6.如果当前元素有子节点会首先调用updateChildren进行子节点遍历

## Vue2比较Vue3

我觉得比较两个框架来说可以从他们设计的角度、优点、缺点来说<br>
Vue本身是一个轻量级的渐进式框架 ，会提供一系列指令方便我们的操作，但是同时这也限制住了我们个人的想法和操作所以 大多数人都认为vue是一个入门相对简单的框架<br>

### Vue本身有什么优点呢 ？
1.支持vnode ，vnode的好处可以根据不同的平台最后产出平台所需要的代码，因为在vue平台本身操作的就是vnode一个js对象而已你怎么计算操作都可以，只需要在输出的时候、输出不同的平台的代码就好了<br>
2.进行diff计算，diff计算能快速的计算出新的vnode和老的vnode不同的地方而快速的进行patch更新节点<br>
3.本身都带有指令 v-if、v-modal还有自定义指令等更方便我们快速开发<br>
这些都是vue2和vue3框架本身共存的一些优点<br>

### Vue本身的缺点
我觉得缺点可能就是这个框架限制了开发者的一些自身的想法。

### 一、更多操作 我觉得可以相对于两个框架优缺比较来说

1.vue2本身在做数据初始化的时候会创建大量的dep和Watcher进行页面中响应式依赖的管理，因为只有这样当后期更新的时候才能知道具体更新哪里，
但是这无疑在内存中会耗费很大的空间，而vue3的这次的迭代更新完美的解决了这个问题，vue在数据初始化的时候会进行effect副作用收集、简单的说就是给当前的响应式数据配置一个effect副作用，当以后数据依赖发生变化的时候这个effect就会执行<br>

2.vue2的数据响应式采用的是Object.defineProperty方法，好处是相对于Vue3的Proxy兼容性要强很多Vue3已经完全抛弃了IE,但是缺点是将数据变为响应式的过程相对于Proxy会复杂很多因为每次都需要遍历一遍数据中的所有值，而且扩展性也没有Proxy强，举个例子：将一个对象包装成响应式数据之后，想往当前对象中在加入响应式数据Proxy可以直接赋值加入就像我们平常给对象用.赋值一样而Object.defineProperty方法需要在调一遍才可以，这也就是说为什么vue2要有set和delete等方法<br>

3.vue3在开发者使用的层面上新推出了compisitionApi使用setUp进行数据的处理，这样的好处是可以将一个功能的内聚性写的特别强特别健壮，因为你不需要像在vue2中写一个功能把所有东西放在一个页面中跳来跳去这样肯定会乱一些日后维护起来肯定没有前者好维护<br>
4.在vue3中优化的速度有了大大的提升主要是因为在模板编译器中进行了很多的预处理所以在最后patch的时候就可以根据我们的预处理直接进行更新而减少大量的计算，主要有一下几种方式的处理

```javascript
{
    1.静态节点提升
    2.精确找到需要更新的位置
    3.缓存事件处理程序
    4.区块 block 查找
    5.vnode上有shapeFlag标记为一个什么样的组件
    6.patchFlag标记动态内容的类型 为了在patch的过程中速度更快
}

```

### api的废弃和新增
1.Vue3废弃了filter、beforCreate、created等api和声明周期，新增了setUp方法


### 异步处理的兼容性
1.vue2中的异步处理从promise开始向下兼容直到setTimeout<br>
2.vue3中直接就是用promise不考虑别的（就当做当前所处环境完全支持promise）<br>

### Vue3采用model-rollup架构这样的好处就是里面的reactive、runtime、compile都可以单独分离出来使用

### vue2和vue3在diff上的计算也是略有不同因为有上面的优化处理，所以在vue3中只比较两个数的头部和尾部 不会在像Vue2中进行收尾比较如果都不符合在进行递归遍历，最后 比较完成 新节点还有剩余就批量插入，老节点还有剩余就批量删除