
## H5定制了`web Workers`来解决因为大计算的情况阻塞ui机制渲染的问题

## 项目持续集成
1.通过git上的webhooks可以实现

## web workers
1.浏览器新增的api可以单独开启线程执行任务，不会影响主线程渲染，当子线程执行完毕会通知主线程

## 跨域相关
1.浏览器与服务端之间的跨域行为  端口 协议 域名 有一个不同就会产生跨域
2.`XMLHttpRequest`对象请求才有跨域行为<br>
3.`img.src` 也能发起get请求 多用于埋点<br>
4.`scripts src` 请求资源 不会有跨域行为<br>
5.服务端设置CORS允许跨域的域名<br>
6.`http-proxy-middleware`代理访问跨域的域名 （webpack的devServer实现原理就是express起的一个小服务器）

## 预检请求
一般限制请求有两种方式<br>
1.浏览器本身限制发起跨域请求<br>
2.跨域请求正常发起，但是返回的结果被浏览器拦截了<br>
3一般浏览器都是第二种方式发起的请求 虽然返回的结果被拦截了 但是 还是会操作服务端的数据 这样会有一定的风险为了规避这个风险
出现了预检请求 就是先发`options`通过之后 才会继续向下走<br>
4.但是并不是所有请求都会预检 产生预检请求是有一定规则的 比如<br>
5.使用`ge`t和`post`常见的两种传输方式的时候会因为`Content-Type`传的参数决定是否要预检请求post的 `Content-Type`是以下值的时候不会预检请求其他值都会<br>
* `text/plain`
* `multipart/form-data`
* `application/x-www-form-urlencoded`

6.使用 `put delete`的时候都会发起预检请求<br>
7.还有人为的设置`cors`首部安全字段的时候会发起预检请求

## Http强缓存策略

Http 1.0<br>
1.使用强缓存需要在服务端设置的expires 时间和本地的时间进行 比较如果本地时间大于expires设置的时间说明过期了去请求新的资源否则就是没过期直接走缓存不需要请求新资源这样的缺点是本地的时间是可以随意更改的所以导致比较时间不准确

```javascript
{res.setHeader('Expires', new Date(Date.now() + 10 * 1000).toUTCString())}
```

Http1.1<br>
1.使用cache-control当catche-control和expires都存在的时候cache-control优先级更高 可以设置max-age过期时间比如设置10分钟那么在这10分钟之内都是有效的会走缓存的资源不需要依赖本地浏览器判断还可以设置以下表格展示的值
```javascript
res.setHeader('Cache-Control', 'max-age=20')
```

Http1.1<br>
1.协商缓存策略`etag`优先级高于`last-modified`<br>
2.使用`last-modified`和`if-modified-since`两个属性第一次请求的时候服务端会响应一个`last-modified`到客户端之后在请求的时候客户端会将这个时间用`if-modified-since`带到服务端做判断，服务端判断如果不需要更新资源直接返回304状态码，反之需要更新资源返回200状态码并且返回新的资源<br>
3.通过etag和if-none-match来判断是否过期原理上和上面说的last-modified相似只是他们比较的是关键内容hash


## 浏览器主要组件有以下几个
1.用户界面包括地址栏回退前进按钮等
2.浏览器引擎在用户界面和引擎之间传递指令
3.呈现引擎主要负责显示页面内容如果此时请求的内容是html他就负责解析html和css内容并将内容展示到页面
4.网络主要负责网络调用比如http请求
5.js解释器用来解析js和执行js代码
6.数据存储这是持久层

## 渲染层逻辑 
1.将html变成html呈现树将css变成css呈现树之后进入布局阶段
2.布局阶段是为每一个节点分配在页面上的坐标之后进行绘制
3.绘制阶段遍历呈现树有用户界面渲染出来
4.这里渲染是渐进式渲染的 也就是说遍历完一部分呈线树就会展示一部分页面
这是webkit内核的渲染逻辑 Gecko内核 布局阶段叫做重排 直是术语 不一样实现方式都相似 firfox用的是Gecko内核 而谷歌和Safari用的是webkit内核