## map 和 weakMap区别
1.针对集合增删改查都有<br>
2.map的键可以是任何类型weakMap的键只能是对象<br>
3.weakMap的键在引用消失后会自动清空不需要垃圾回收机制操心所以同时也不能枚举（因为你不知道什么时候就会消失）<br>
4.weakMap的使用场景 为element DOM添加属性 在DOM清除的时候weakMap自动清空


## Es6新增的for of和for in的区别
1.首先for of 是遍历值的for in 是遍历下标的 <br>
2.for of正常是不支持遍历普通对象的 for in可以遍历<br>
3.for of背后是对iterator的调用所以说如果就算是对象内部如果有iterator属性也是可以遍历的

## Commonjs和Es6的区别
1.es6模块中顶层this指向undefined，commonjs中顶层this指向当前模块 <br>
2.es6输出的是模块的引用并且是编译时加载。commonjs输出的是值的复制 并且是运行是加载 <br>
3.commonjs模块就是一个脚本文件，require命令第一行加载该脚本时就会执行整个脚本，并且在内存中生成一个对象 <br>
3.commonjs模块无论加载多少次都只会在第一次加载时运行一次，以后加载时就返回第一次运行的结果，除非手动清除缓存

## 变量的赋值过程
1.会执行两个动作，首先编译器会在当前作用域中声明一个变量（如果之前没有声明过），然后在运行时引擎会在作用域中查找该变量，如果能找到就会对他赋值<br>
2.例如 var a = 2 实际上JavaScript引擎将var a 和a=2 看做两个单独的声明，第一个是编译阶段执行的任务第二个是执行阶段的任务


## eval和with
1.eval()方法可以接受一个字符串 在非严格模式下可以用来改变当前的一个词法作用域环境 <br>
2.with声明实际上是根据你传递给它的对象凭空创建了一个全新的词法作用域，会将这个词法作用域添加到当前所处的函数作用域中 

## this篇
1.this并不指向函数自身也不指向函数的词法作用域，实际上是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里调用 <br>
2.call，bind如果传了字符串类型、布尔类型、数字类型来当做this绑定 这个原始值会被转化为它的对象像是 也就是 new String() new Boolean() new Number() <br>
3.对于默认绑定来说，决定this的绑定对象并不是调用位置是否处于严格模式，而是取决于函数体是否在严格模式下，如果函数体在严格模式下那么this就会绑定到undefined，否则this会绑定到全局对象上 <br>
4.Object.create(null)创建的对象如果不指定this绑定 是没有自己原型对象的 创建出来的对象 比 {}还空

## typeof null为啥返回object ？
1.这本身是语言层面的一个bug 一直没被解决的  主要原因是 在js中不同的对象在底层都表示为二进制，而二进制前三位都是0的话会被判定为object类型，null的二进制表示全是0，自然前三位也是0 所以执行typeof会返回object

## 怎么检测对象上是否有某个属性？
1.hasOwnProperty方法只会检测自身属性 使用in操作符检测自身和原型对象 

## 怎么计算首屏渲染时间和白屏时间？
1.window.performance 里面有一个参数是timing <br>
2.在这个参数重可以通过里面给出的字段来判断首屏加载时间和首屏渲染时间包括dns解析和http连接时间通过这些参数就可以大概率的了解到整个页面的性能<br> 
3.好多第三方的监控工具的实现原理也是这个方法 例如 听云 诸葛 哨兵

## 错误日志怎么监控
1.window.onerror监控错误日志可以在这里进行上报

## H5新增的api
1.requestAnimationFrame是一个优化js动画的api <br>
2.Page visibility api可以让开发人员知道用户什么时候正在看着页面呢，而什么时候页面是隐藏的 <br>
3.Geolocation api 在得到用户许可的情况下，可以确定用户所在的位置，在移动web应用中，这个api非常重要 <br>
4.file api 可以读取文件内容 用于显示 处理和上传与h5的拖放功能结合很容易就能制造出拖放上传功能 <br>
5.web Timing 给出了页面加载和渲染过程的很多信息，对性能优化很有价值 <br>
6.web workers 可以运行异步的js代码避免阻塞用户界面，在执行复杂的计算和数据处理的时候，这个api非常有用，要不然这些任务轻则会占用很长时间重则会导致用户无法与页面交互

## class 类中super指向哪里？
1.super指向的是父类的原型对象

## js浮点精度计算问题怎么计算
1.可以使用 Math.abs配合es6提供的Number.EPSILO比较两个数是否相等 <br>
2.或者将浮点数变为整数进行计算

## 判断一个数字是否为整数
1.可以使用Number.isInteger判断一个数字是否为整数

## Object.prorotype.toString.call根据什么判断的类型
1.所有typeof 返回的Object类型内部都有一个Class内部属性这个属性表明了当前的类型 我们通过Object.prorotype.toString.call查看的就是这个类型