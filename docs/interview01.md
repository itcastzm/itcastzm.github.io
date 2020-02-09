# 初中级前端面试题目汇总和答案解析(转)
## 1. 介绍一下ES6的新特性
[核心特性]
- const和let
- 模板字符串
- 箭头函数
- 函数的参数默认值
- Spread / Rest 操作符
- 二进制和八进制字面量(通过在数字前面添加0o或0O即可将其转为八进制值,二进制使用0b或者0B)
- 对象和数组解构
- ES6中的类(class)
- Promise
- Set()和Map()数据结构
- Modules（模块, 如import, export）
- for..of 循环

## 介绍Promise以及Promise的几种状态
[参考答案]
介绍: Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。状态: pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

## 谈谈你对闭包的理解及其优缺点
闭包就是能够读取其他函数内部变量的函数. 本质上,闭包是将函数内部和函数外部连接起来的桥梁.
- 优点
    逻辑连续，当闭包作为另一个函数调用参数时，避免脱离当前逻辑而单独编写额外逻辑。
    延长局部变量的生命周期, 更具有封装性, 保护局部变量。
- 缺点
    容易造成内存溢出
    闭包会在父函数外部，改变父函数内部变量的值,所以可能会导致改变父函数的变量
## React的生命周期
[参考答案]
- 初始化阶段 defaultProps -> constructor -> componentWillMount() -> render() -> componentDidMount()
- 运行中阶段 componentWillReceiveProps() -> shouldComponentUpdate() -> componentWillUpdate() -> componentDidUpdate()
- 销毁阶段 componentWillUnmount()
## React中componentWillReceiveProps的触发条件是什么
[参考答案] 
componentWillReceiveProps会在接收到新的props的时候调用

## vue中v-if和v-show的区别
[参考答案]
- v-show不管条件是真还是假，第一次渲染的时候标签都会添加到DOM中。切换的时候，通过display: none;样式来显示隐藏元素,对性能影响不是很大。
- v-if在首次渲染的时候，如果条件为假，不会在页面渲染该元素。当条件为真时，开始局部编译，动态的向DOM元素里面添加元素。当条件从真变为假的时候，开始局部编译，卸载这些元素，也就是删除。对性能有一定影响

## 小程序里面开页面最多多少
[参考答案] 
小程序原生页面存在层级限制，超过一定层数就会无法打开新页面。一开始这个限制为不超过5层，目前是不超过10层。

##  取数组的最大值（ES5、ES6）
[参考答案]
```javascript 
// es5
Math.max.apply(null, arr);
// es6
Math.max(...arr);
```

## http并发请求资源数上限
[参考答案] 
HTTP客户端一般对同一个服务器的并发连接个数都是有限制的, 最大为6条。

## 如何优化网站的SEO
[参考答案]
1. 网站结构布局优化：尽量简单, 提倡扁平化结构. 一般而言，建立的网站结构层次越少，越容易被“蜘蛛”抓取，也就容易被收录。 
2. img标签必须添加“alt”和“title”属性，告诉搜索引擎导航的定位，做到即使图片未能正常显示时，用户也能看到提示文字。
3. 把重要内容HTML代码放在最前搜索引擎抓取HTML内容是从上到下，利用这一特点，可以让主要代码优先读取，广告等不重要代码放在下边。
4. 控制页面的大小，减少http请求，提高网站的加载速度。
5. 合理的设计title、description和keywords
    - title标题：只强调重点即可，尽量把重要的关键词放在前面，关键词不要重复出现，尽量做到每个页面的title标题中不要设置相同的内容。
    - meta keywords页面/网站的关键字。
    - meta description网页描述，需要高度概括网页内容，切记不能太长，过分堆砌关键词，每个页面也要有所不同。
6. 语义化书写HTML代码，符合W3C标准尽量让代码语义化，在适当的位置使用适当的标签，用正确的标签做正确的事。让阅读源码者和“蜘蛛”都一目了然。
7. a标签：页面链接，要加 “title” 属性说明，链接到其他网站则需要加上 el="nofollow" 属性, 告诉 “蜘蛛” 不要爬，因为一旦“蜘蛛”爬了外部链接之后，就不会再回来了。
8. 图标使用IconFont替换
9. 使用CDN网络缓存，加快用户访问速度，减轻服务器压力
10. 启用GZIP压缩，浏览速度变快，搜索引擎的蜘蛛抓取信息量也会增大
11. SSR技术
12. 预渲染技术

## 介绍下HTTP状态码, 403、301、302分别代表什么
[参考答案]
当浏览者访问一个网页时，浏览者的浏览器会向网页所在服务器发出请求。当浏览器接收并显示网页前，此网页所在的服务器会返回一个包含HTTP状态码的信息头（server header）用以响应浏览器的请求。
- 301 (永久移动) 请求的网页已永久移动到新位置。服务器返回此响应(对 GET 或 HEAD 请求的响应)时，会自动将请求者转到新位置
- 302 (临时移动) 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求
- 403 (禁止) 服务器拒绝请求

## RESTful常用的方法和介绍
[参考答案] 
rest请求方法有4种，包括get,post,put,delete.分别对应获取资源，添加资源，更新资源及删除资源.

## React16.3对生命周期的改变
[参考答案] 
React16.3+废弃的三个生命周期函数
•componentWillMount•componentWillReceiveProps•componentWillUpdate
取而代之的是两个新的生命周期函数
•static getDerivedStateFromProps(nextProps, prevState) 取代componentWillMount、componentWillReceiveProps和componentWillUpdate•getSnapshotBeforeUpdate(prevProps, prevState) 取代componentWillUpdate

```javascripts
class ScrollingList extends React.Component {
  constructor(props) {
    super(props);
    this.listRef = React.createRef();
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    // 返回的数据将作为componentDidUpdate第三个参数
    return {
      stateA: 'xxx'
    };
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    if (snapshot !== null) {
      console.log(snapshot.stateA)
    }
  }

  render() {}
}
```
## 介绍React高阶组件
高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。具体而言，高阶组件是参数为组件，返回值为新组件的函数.其本身是纯函数，没有副作用。
```javascript 
function logProps(WrappedComponent) {
  return class extends React.Component {
    componentWillReceiveProps(nextProps) {
      console.log('Current props: ', this.props);
      console.log('Next props: ', nextProps);
    }
    render() {
      // 将 input 组件包装在容器中，而不对其进行修改。Good!
      return <WrappedComponent {...this.props} />;
    }
  }
}
```

## 缓存相关的HTTP请求头
[参考答案]

| headers字段     | 解释     | 案例     |
| --------------------  |  -------------------------------- |  ------------------------------- |
| Expires   | 服务端告诉浏览器，先把这个文件缓存起来，在这个过期时间之前，该文件都不会变化 | Expires: Mon, 1 Aug 2016 22:43:02 GMT     |
| Cache-Control  | 由于Expires给定的是绝对时间，而客户端的系统时间可以由用户任意修改, Cache-Control为相对时间 | Cache-Control: max-age=80     |
| Last-Modified(response header) / If-Modified-Since (request header) | 服务端收到请求后会对比目前文件的最后修改时间和该请求头的信息，如果没有修改，那就直接返回304给浏览器，而不返回实际资源。如果有变化了，就返回200，并且带上新的资源内容 | If-Modified-Since:Mon, 01 Aug 2016 13:48:44 GMT  |
| Etag / If-None-Match   |  第一次请求后响应头中包含了Etag，作为时间标签,下一次请求时会把原来的Etag标签带上（在请求头中变成了If-None-Match）作为校验标准，若这个文件如果发生了改变，则Etag也会改变。服务器对比浏览器请求头中的的If-None-Match：如果相同就返回304，而不返回实际资源如果不同，就返回200和新的资源。          |  |


## 如何优化用户体验
[参考答案]
- 页面渲染前使用骨架屏或者加载动画,避免大块白屏
- 使用预渲染或者ssr技术提高首屏加载时间
- 动画使用css3硬件加速,避免用户操作动画卡顿
- 计算密集型业务使用web worker或者js分片处理,避免js线程阻塞
- 页面状态监控,给用户提供反馈机制
- 静态资源走CDN缓存或者oss服务,提高用户访问速度
- 避免用户操作报错,提供404页面或则错误提示页面

## 对PWA的了解
>[参考答案]
progressive web app：渐进式网页应用.可以将 Web 和 App 各自的优势融合在一起：渐进式、可响应、可离线、实现类似 App 的交互、即时更新、安全、可以被搜索引擎检索、可推送、可安装、可链接。其核心技术包括 App Manifest、Service Worker、Web Push，等等。
## 介绍下跨域
>[参考答案]
同源策略/SOP（Same origin policy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSFR等攻击, 同源策略要求两个通讯地址的协议、域名、端口号必须相同，否则两个地址的通讯将被浏览器视为不安全的，并被阻挡下来. 要突破SOP的限制,我们可以使用如下方式:
- CORS 同域安全策略CORS是一种跨域资源请求机制，它要求当前域在响应报头添加Access-Control-Allow-Origin标签，从而允许指定域的站点访问当前域上的资源
```javascript
res.setHeader("Access-Control-Allow-Origin","*");
// 不过CORS默认只支持GET/POST这两种http请求类型，如果要开启PUT/DELETE之类的方式，需要在服务端在添加一个"Access-Control-Allow-Methods"报头标签：
 res.setHeader(
    "Access-Control-Allow-Methods",
    "PUT, GET, POST, DELETE, HEAD, PATCH"
  )
```
- HTML5 postMessage 可以使用 postMessage 方法和 onmessage 事件来实现不同域之间的通信，其中postMessage用于实时向接收信息的页面发送消息
- HTML5 WebSocket WebSocket protocol 是HTML5一种新的协议。它实现了浏览器与服务器全双工通信，同时允许跨域通讯，是server push技术的一种很棒的实现
- JSONP 是JSON的一种“使用模式”，主要是利用script标签不受同源策略限制的特性，向跨域的服务器请求并返回一段JSON数据
- iframe形式 通过设置相同的document.domain, 或者不同域通过window.name传递数据
- 服务器代理
## Access-Control-Allow-Origin在服务端哪里配置
[参考答案] 
response header响应头

##  csrf跨站攻击怎么解决
[参考答案]
CSRF, 跨站请求伪造,它可以在用户毫不知情的情况下以用户名义伪造请求发送给受攻击站点，从而对用户或者网站造成攻击. 预防措施如下:
- 服务器端验证HTTP Referer字段, Referer记录了该HTTP请求的来源地址
- 在请求地址中添加token并验证 
- 在HTTP头中自定义属性并验证

## 用js写一个数组扁平化函数
[参考答案]
```javascript 

// reduce
function flatten(arr = []) {  
return arr.reduce((result, item) => {
return result.concat(Array.isArray(item) ? flatten(item) : item)
}, [])
}

// (toString | join) & split(利用数组的toString或者join,将数组转化为字符串)
function flatten(arr = []) {
return arr.toString().split(',').map(item => Number(item))
}
```

## Promise和Callback有什么区别
[参考答案]
相比于callback，Promise 具有更易读的代码组织形式（链式结构调用），更好的异常处理方式（在调用 Promise 的末尾添加上一个catch方法捕获异常即可），以及异步操作并行处理的能力（Promise.all()Promise.race()等）。callback最大的问题就是我们通常说的回调地狱,一旦业务逻辑复杂了,我们不得不使用大量的嵌套回调代码,可维护性很低.

##  如何实现高度自适应
[参考答案]
- 使用绝对定位, 设置top,bottom属性•使用flex布局•float+ height:100%

## cookie, session, storage的区别和联系
[参考答案]
- cookie存储于浏览器端，而session存储于服务端
- cookie的安全性相比于session较弱,cookie容易被第三方劫持,考虑到安全应当使用session
- session保存在服务器上,当访问增多时，会占用服务器的资源
- cookie存储容量有限制，单个cookie保存数据不能超过4k，且很多浏览器限制一个站点最多保存20个cookie。对于session,默认大小一般是1M
- cookie、sessionStorage、localStorage,都保存在浏览器端，且受同源策略影响
- cookie数据始终在同源的http请求中携带，而Storage不会再请求中携带，仅在本地存储
- 存储大小上, cookie一般是4k，Storage可以达到5M-10M
- 数据存储时间上：sessionStorage仅仅是会话级别的存储，它只在当前浏览器关闭前有效，不能持久保持；localStorage始终有效，即使窗口或浏览器关闭也一直有效，除非用户手动删除；cookie只在设定的 过期时间之前有效
- 作用域上：sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage和 cookie在所有同源窗口是共享的
- Storage支持事件通知机制，可以将数据更新的通知发送给监听者。并且它提供增删查api使用更方便

## 对web安全的理解
[参考答案]
- CSRF 攻击和防范
跨站请求伪造,可以理解为攻击者盗用了用户的身份，以用户的名义发送恶意请求，造成用户隐私及财产损失 过程: 1.登录受信任网站并在本地生成cookie; 2.在不登出 网站 的情况下访问危险网站 防范: 关键操作只接受POST请求, 使用验证码, 检测Referer, 使用token(或者JWT)
- XSS 攻击和防范
全称Cross-site script，跨站脚本攻击，是Web程序中常见的漏洞。原理是攻击者向有XSS漏洞的网站中输入恶意的脚本，当其它用户浏览该网站时候，该脚本会自动执行，从而达到攻击的目的(盗取Cookie，破坏页面结构，重定向到钓鱼网站等)。区分: 分为持久型XSS和非持久性XSS. 持久型XSS是将攻击的脚本植入到服务器，从而导致每个访问的用户都会遭到此XSS脚本的攻击。非持久型XSS是将恶意脚本包装在页面的URL参数中，通过URL链接骗取用户访问，从而进行攻击. 防范: 对用户输入进行HTML转义, 对敏感信息进行过滤
- SQL 注入与防范
通过把SQL命令插入到表单中并提交或页面请求的参数中，最终使得服务器执行恶意的SQL命令. 防范: 对用户的输入进行校验或限制长度；对特殊字符进行转换, 不要使用动态拼装SQL，为每个应用使用单独的权限有限的数据库连接。对隐私信息进行加密
- DDOS 攻击
分布式拒绝服务(DDoS:Distributed Denial of Service)攻击指借助于客户/服务器技术，将多个计算机联合起来作为攻击平台，对一个或多个目标发动DDoS攻击，从而成倍地提高拒绝服务攻击的威力。

##  用js实现数组随机取数，每次返回的值都不一样 
```javascript 
function getUniqueItems(arr, num) {
  let temp = [];
  for (let index in arr) {
      temp.push(arr[index]);
  }
  let res = [];
  for (let i = 0; i<num; i++) {
      if (temp.length>0) {
          let arrIndex = Math.floor(Math.random()*temp.length);
          res[i] = temp[arrIndex];
          temp.splice(arrIndex, 1);
      } else {
        break;
      }
  }
  return res;
}
```
## 页面上有1万个button如何绑定事件
[参考答案] 
事件委托, 冒泡触发
## base64为什么能提升性能以及它的缺点是什么 
[参考答案] 
优点:
- 无额外请求
- 适用于很小或者很简单的图片
- 可像单独图片一样使用，比如背景图片等
- 没有跨域问题，不需要考虑缓存、文件头或者cookies问题 
缺点:
- CSS 文件体积的增大, 造成CRP(关键渲染路径)阻塞
- 页面解析CSS生成的CSSOM时间增加

## 介绍webp图片文件格式
[参考答案]
WebP是一种支持有损压缩和无损压缩的图片文件格式，根据Google的测试，无损压缩后的WebP比PNG 文件少了45％的文件大小，即使这些PNG文件经过其他压缩工具压缩之后，WebP 还是可以减少28％的文件大小。
- 优点
•更小的文件尺寸•更高的质量——与其他相同大小不同格式的压缩图像比较
- 缺点
•编码和解码速度比较慢,存在一定兼容性

## react-router里的Link标签和a标签有什么区别
参考答案]
从渲染的DOM来看，这两者都是链接，都是a标签，区别是：Link是react-router里实现路由跳转的链接，配合Route使用，react-router拦截了其默认的链接跳转行为，区别于传统的页面跳转，Link 的“跳转”行为只会触发相匹配的Route对应的页面内容更新，而不会刷新整个页面。a标签是html原生的超链接，用于跳转到href指向的另一个页面或者锚点元素,跳转新页面会刷新页面。
##  介绍一下函数柯里化,并写一个柯里化函数
[参考答案]
柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术。柯里化函数能够让我们：
1. 在多个函数调用中依次收集参数，不用在一个函数调用中收集所有参数。
2. 当收集到足够的参数时，返回函数执行结果。

## 介绍一下从输入URL到页面加载全过程 
[参考答案]
- 浏览器的地址栏输入URL并按下回车。
- 浏览器查找当前URL是否存在缓存，并比较缓存是否过期。
- DNS解析URL对应的IP。
- 根据IP建立TCP连接（三次握手）。
- HTTP发起请求。
- 服务器处理请求，浏览器接收HTTP响应。
- 渲染页面，构建DOM树。
- 关闭TCP连接（四次挥手）。

## 说说bind、call、apply的区别
[参考答案]
call和apply的共同点
- 都能够改变函数执行时的上下文，将一个对象的方法交给另一个对象来执行，并且是立即执行的
- 调用call和apply的对象，必须是一个函数Function
call和apply的区别
- apply的第二个参数，必须是数组或者类数组，它会被转换成类数组，传入函数中，并且会被映射到函数对应的参数上, 而call从第二个参数开始，可以接收任意个参数
bind
- bind()方法创建一个新的函数，与apply和call比较类似，也能改变函数体内的this指向。不同的是，bind方法的返回值是函数，并且需要稍后调用，才会执行。而apply和call 则是立即调用

## ES6中的map和原生的对象有什么区别
[参考答案]
object和Map存储的都是键值对组合。区别：object的键的类型是字符串；map的键的类型可以是任意类型；另外，object获取键值使用Object.keys（返回数组）Map获取键值使用map变量.keys() (返回迭代器)。

## 说说H5手机端的适配的几种方案 
[参考答案]
1. js实现一
```javascript 
(function (doc, win) {
  var docEl = doc.documentElement,
      resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
      recalc = function () {
          var clientWidth = docEl.clientWidth;
          var fontSize = 20;
          docEl.style.fontSize = fontSize + 'px';
          var docStyles = getComputedStyle(docEl);
          var realFontSize = parseFloat(docStyles.fontSize);
          var scale = realFontSize / fontSize;
          console.log("realFontSize: " + realFontSize + ", scale: " + scale);
          fontSize = clientWidth / 750 * 20;
          if(isIphoneX()) fontSize = 19;
          fontSize = fontSize / scale;
          docEl.style.fontSize = fontSize + 'px';
      };
  // Abort if browser does not support addEventListener
  if (!doc.addEventListener) return;
  win.addEventListener(resizeEvt, recalc, false);
  doc.addEventListener('DOMContentLoaded', recalc, false);

  // iphoneX判断
  function isIphoneX(){
      return /iphone/gi.test(navigator.userAgent) && (screen.height == 812 && screen.width == 375)
  }

})(document, window);
```
2. js实现二
```javascript 
(function(base) {
var _base = base|| 75;
var ua = navigator.userAgent;
var matches = ua.match(/Android[\S\s]+AppleWebkit\/(\d{3})/i);
var isIos = navigator.appVersion.match(/(iphone|ipad|ipod)/gi);
var dpr = window.devicePixelRatio || 1;
if(!isIos && !(matches && matches[1] > 534)) {
// 如果非iOS, 非Android4.3以上, dpr设为1;
        dpr = 1;
}
var scale = 1/ dpr;
var metaEl = document.querySelector('meta[name="viewport"]');
if(!metaEl) {
        metaEl = document.createElement('meta');
        metaEl.setAttribute('name', 'viewport');
        window.document.head.appendChild(metaEl);
}
    metaEl.setAttribute('content', 'width=device-width,user-scalable=no,initial-scale='+ scale + ',maximum-scale='+ scale + ',minimum-scale='+ scale);

    document.documentElement.style.fontSize = document.documentElement.clientWidth / (750/ _base) + 'px';
})(75);
```
3. css @media媒介查询(苏宁易购实现方式)
4. 手淘的lib-flexible实现方式

##  用js如何去除url中的#号 
[参考答案]
- 情景一: 单纯将hash路由改变成history路由即可去除hash的#号,此时需要服务器做路由重定向,比如nginx, node重定向
- 情景二: 单纯去除#
```javascript
function dropHash(url) {
  let i = url.indexOf('#')
  return i > -1 ? url.replace(/#/g, '') : url
}
```
## Redux状态管理器和变量挂载到window中有什么区别
[参考答案]
redux通过制定一套严格的规范，提供一种规范式的api和使用方式来处理状态, 适合大型项目和多人协同开发的项目，虽然代码编写有些繁复，但整体数据流向清楚，便于问题跟踪和后期维护,并且支持状态回溯.而window的变量管理比较混乱,维护不当还会造成变量污染,不适合中大型项目的开发.

## webpack和gulp的优缺点
| | | |
| --|-- |-- |
| |gulp | webpack|
| 定位| 基于任务流的自动化打包工具 | 模块化打包工具 |
| 优点| 易于学习和理解, 适合多页面应用开发 | 可以模块化的打包任何资源,适配任何模块系统,适合SPA单页应用的开发|
| 缺点| 不太适合单页或者自定义模块的开发| 学习成本低,配置复杂,通过babel编译后的js代码打包后体积过大|

## 说说jsonp为什么不支持post方法 [参考答案]
浏览器的同源策略限制从一个源加载的文档或脚本与来自另一个源的资源进行交互,jsonp跨域本质上是通过动态script标签, 本质上也是对静态资源的访问,所以只能是get请求

## 说说栈和堆的区别, 垃圾回收时栈和堆的区别以及栈和堆具体怎么存储
[[参考答案]
1. 从定义和存取方式上说:
- 栈stack为自动分配的内存空间, 它由系统自动释放, 特点是"LIFO，即后进先出（Last in, first out）"。数据存储时只能从顶部逐个存入，取出时也需从顶部逐个取出,js的基本数据类型(Undefined、Null、Boolean、Number和String). 基本类型在内存中占据空间小、大小固定 ，他们的值保存在栈空间，按值访问
- 堆heap是动态分配的内存，大小不定也不会自动释放. 特点是"无序"的key-value"键值对"存储方式. 比如js的对象,数组. 引用类型占据空间大、大小不固定, 栈内存中存放地址指向堆(heap)内存中的对象。是按引用访问的
2. 从js数据的存取过程上说:
栈内存中存放的是对象的访问地址，在堆内存中为这个值分配空间。这个值大小不固定，因此不能把它们保存到栈内存中。但内存地址大小的固定的，因此可以将内存地址保存在栈内存中。这样，当查询引用类型的变量时，先从栈中读取内存地址，然后再通过地址找到堆中的值。

3. 栈内存和堆内存与垃圾回收机制的联系和清除方式:
- 垃圾回收机制: JavaScript中有自动垃圾回收机制，会通过标记清除的算法识别哪些变量对象不再使用，对其进行销毁。开发者也可在代码中手动设置变量值为null（xxx = null）进行清除，让引用链断开，以便下一次垃圾回收时有效回收。其次, 函数执行完成后，函数局部环境声明的变量不再需要时，就会被垃圾回收销毁（理想的情况下，闭包会阻止这一过程）。全局环境只有页面退出时才会出栈，解除变量引用。所以工程师们应尽量避免在全局环境中创建全局变量，如需使用，一定要在不需要时手动标记清除，将内存释放。
- 栈内存和堆内存通常与垃圾回收机制有关。之所以会区分栈内存和堆内存,目的是使程序运行时占用的内存最小。当某个方法执行时，都会建立自己的内存栈，在这个方法内定义的变量将会逐个放入栈内存里，随着方法的执行结束，这个方法的内存栈也将自然销毁了。因此，所有在方法中定义的变量都是放在栈内存中的；当我们创建一个对象时，对象会被保存到运行时数据区中，以便反复利用（因为对象的创建内存开销较大），这个运行时数据区就是堆内存。堆内存中的对象不会随方法的结束而销毁，即使方法结束后，这个对象还可能被另一个引用变量所引用，则这个对象依然不会被销毁，只有当一个对象没有任何引用变量引用它时，系统的垃圾回收机制才会在循环收集的过程中回收。

## 谈谈你对发布-订阅和观察者模式的区别
[参考答案]
1. 从定义上:
- 观察者模式: 在软件设计中是一个对象，维护一个依赖列表，当任何状态发生改变自动通知它们。
- 发布-订阅设计模式: 在发布-订阅模式，消息的发送方，叫做发布者，消息不会直接发送给特定的接收者，叫做订阅者。
2. 区别:
- 在观察者模式中，观察者知道被观察者，被观察者一直保持对观察者进行记录。在发布订阅模式中，发布者和订阅者不知道对方的存在, 它们只有通过消息代理进行通信
- 在发布订阅模式中，组件是松散耦合的，正好和观察者模式相反
- 观察者模式大多数时候是同步的，比如当事件触发，被观察者就会去调用观察者的方法。而发布-订阅模式大多数时候是异步的（使用消息队列）
- 观察者模式需要在单个应用程序地址空间中实现，而发布-订阅更像交叉应用模式

## 介绍排序算法和快排原理 
[参考答案]
排序算法有:
冒泡排序, 希尔排序, 快速排序, 插入排序, 归并排序, 堆排序, 桶排序等.
快速排序原理:
通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

## 说说node文件查找的优先级
[参考答案]
从文件模块缓存中加载 > 从原生模块加载 > 从文件加载

## WebView和原生是如何通信的
[参考答案]
- 使用Android原生的JavascriptInterface来进行js和java的通信
- UrlRouter(通过内部实现的框架去拦截前端写的url，如果符合UrlRouter的协议的话，就执行相应的逻辑)
- WebView 中的 prompt/console/alert 拦截，通常使用 prompt避免副作用
- API注入，通过Native获取JavaScript环境上下文，并直接在上面添加方法，使js可以直接调用

## pm2怎么做进程管理，如何解决进程奔溃问题
[参考答案]
- 通过pm2 start去开启一个进程, pm2 stop去停止某个进程, pm2 list去查看进程列表, pm2 monit查看每个进程的cpu使用情况, pm2 restart重启指定应用等
- 进程奔溃可以用过设置自动重启或者限制应用运行内存max_memory_restart(最大内存限制数，超出自动重启)

## 直接给一个数组项赋值，Vue 能检测到变化吗,为什么？
[参考答案] 
vue中的数组的监听不是通过Object.defineProperty来实现的，是通过对'push', 'pop','shift','unshift','splice', 'sort','reverse'等改变数组本身的方法来通知监听的,所以直接给数组某一项赋值无法监听到变化,解决方案如下:
- 用vue的set方法改变数组或者对象
- 用改变数组本身的方法如splice, pop, shift等
- 用深拷贝,解构运算符

## 手写 bind、call 和 apply
```javascript 

Function.prototype.bind = function(context, ...bindArgs) {
  // func 为调用 bind 的原函数
  const func = this;
  context = context || window;

  if (typeof func !== 'function') {
    throw new TypeError('Bind must be called on a function');
  }
  // bind 返回一个绑定 this 的函数
  return function(...callArgs) {
    let args = bindArgs.concat(callArgs);
    if (this instanceof func) {
      // 意味着是通过 new 调用的 而 new 的优先级高于 bind
      return new func(...args);
    }
    return func.call(context, ...args);
  }
}

// 通过隐式绑定实现
Function.prototype.call = function(context, ...args) {
  context = context || window;
  context.func = this;

  if (typeof context.func !== 'function') {
    throw new TypeError('call must be called on a function');
  }

  let res = context.func(...args);
  delete context.func;
  return res;
}

Function.prototype.apply = function(context, args) {
  context = context || window;
  context.func = this;

  if (typeof context.func !== 'function') {
    throw new TypeError('apply must be called on a function');
  }

  let res = context.func(...args);
  delete context.func;
  return res;
}
```

## 实现一个继承
```javascript

// 参考 You Dont Know JavaScript 上卷
// 基类
function Base() {
}
// 派生类
function Derived() {
    Base.call(this);
}
// 将派生类的原型的原型链挂在基类的原型上
Object.setPrototypeOf(Derived.prototype, Base.prototype);
```

## 实现一个 new
```javascript 

// 手动实现一个 new 关键字的功能的函数 _new(fun, args) --> new fun(args)
function _new(fun, ...args) {
    if (typeof fun !== 'function') {
        return new Error('参数必须是一个函数');
    }
    let obj = Object.create(fun.prototype);
    let res = fun.call(obj, ...args);
    if (res !== null && (typeof res === 'object' || typeof res === 'function')) {
        return res;
    }
    return obj;
}
```

## 实现一个 instanceof
```javascript 
// a instanceof b
function _instanceof(a, b) {
    while (a) {
        if (a.__proto__ === b.prototype) return true;
        a = a.__proto__;
    }
    return false;
}
```

## 手写 jsonp 的实现
```javascript
// foo 函数将会被调用 传入后台返回的数据
function foo(data) {
    console.log('通过jsonp获取后台数据:', data);
    document.getElementById('data').innerHTML = data;
}
/**
 * 通过手动创建一个 script 标签发送一个 get 请求
 * 并利用浏览器对 <script> 不进行跨域限制的特性绕过跨域问题
 */
(function jsonp() {
    let head = document.getElementsByTagName('head')[0]; // 获取head元素 把js放里面
    let js = document.createElement('script');
    js.src = 'http://domain:port/testJSONP?a=1&b=2&callback=foo'; // 设置请求地址
    head.appendChild(js); // 这一步会发送请求
})();

// 后台代码
// 因为是通过 script 标签调用的 后台返回的相当于一个 js 文件
// 根据前端传入的 callback 的函数名直接调用该函数
// 返回的是 'foo(3)'
function testJSONP(callback, a, b) {
  return `${callback}(${a + b})`;
}
```

## ajax 的实现
``` javascript 

// Asynchronous Javascript And XML
function ajax(options) {
  // 选项
  var method = options.method || 'GET',
      params = options.params,
      data = options.data,
      url = options.url + (params ? '?' + Object.keys(params).map(key => key + '=' + params[key]).join('&') : ''),
      async = options.async === false ? false : true,
      success = options.success,
      headers = options.headers;

  var request;
  if (window.XMLHttpRequest) {
    request = new XMLHttpRequest();
  } else {
    request = new ActiveXObject('Microsoft.XMLHTTP');
  }

  request.onreadystatechange = function() {
    /**
    readyState:
      0: 请求未初始化
      1: 服务器连接已建立
      2: 请求已接收
      3: 请求处理中
      4: 请求已完成，且响应已就绪

    status: HTTP 状态码
    **/
    if (request.readyState === 4 && request.status === 200) {
      success && success(request.responseText);
    }
  }

  request.open(method, url, async);
  if (headers) {
    Object.keys(headers).forEach(key => request.setRequestHeader(key, headers[key]));
  }
  method === 'GET' ? request.send() : request.send(data);
}
// e.g.
ajax({
  method: 'GET',
  url: '...',
  success: function(res) {
    console.log('success', res);
  },
  async: true,
  params: {
    p: 'test',
    t: 666
  },
  headers: {
    'Content-Type': 'application/json'
  }
})
```

## reduce 的实现
```javascript 

function reduce(arr, callback, initial) {
    let i = 0;
    let acc = initial === undefined ? arr[i++] : initial;
    for (; i < arr.length; i++) {
        acc = callback(acc, arr[i], i, arr);
    }
    return acc;
}
```

## 实现 generator 的自动执行器
要求是 yield 后面只能是 Promise 或 Thunk 函数，详见 es6.ruanyifeng.com/#docs/generator
```javascript 
function run(gen) {
  let g = gen();

  function next(data) {
    let result = g.next(data);
    if (result.done) return result.value;
    if (result.value instanceof Promise) {
      result.value.then(data => next(data));
    } else {
      result.value(next);
    }
  }

  return next();
}

// ======== e.g. ==========

function func(data, cb) {
  console.log(data);
  cb();
}

function *gen() {
  let a = yield Promise.resolve(1);
  console.log(a);
  let b = yield Promise.resolve(2);
  console.log(b);
  yield func.bind(null, a + b);
}
run(gen);
/**
output:
1
2
3
**/
```

## 节流
老生常谈了，感觉没必要写太复杂
```javascript 

/**
 * 节流函数 限制函数在指定时间段只能被调用一次
 * 用法 比如防止用户连续执行一个耗时操作 对操作按钮点击函数进行节流处理
 */
function throttle(func, wait) {
  let timer = null;
  return function(...args) {
    if (!timer) {
      func(...args);
      timer = setTimeout(() => {
        timer = null;
      }, wait);
    }
  }
}
```

## 防抖

```javascript
/**
 * 函数调用后不会被立即执行 之后连续 wait 时间段没有调用才会执行
 * 用法 如处理用户输入
 */
function debounce(func, wait) {
  let timer = null;

  return function(...args) {
    if (timer) clearTimeout(timer); // 如果在定时器未执行期间又被调用 该定时器将被清除 并重新等待 wait 秒
    timer = setTimeout(() => {
      func(...args);
    }, wait);
  }
}
```

## 
手写 Promise
简单实现，基本功能都有了。
```javascript 
const PENDING = 1;
const FULFILLED = 2;
const REJECTED = 3;

function MyPromise(executor) {
    let self = this;
    this.resolveQueue = [];
    this.rejectQueue = [];
    this.state = PENDING;
    this.val = undefined;
    function resolve(val) {
        if (self.state === PENDING) {
            setTimeout(() => {
                self.state = FULFILLED;
                self.val = val;
                self.resolveQueue.forEach(cb => cb(val));
            });
        }
    }
    function reject(err) {
        if (self.state === PENDING) {
            setTimeout(() => {
                self.state = REJECTED;
                self.val = err;
                self.rejectQueue.forEach(cb => cb(err));
            });
        }
    }
    try {
        // 回调是异步执行 函数是同步执行
        executor(resolve, reject);
    } catch(err) {
        reject(err);
    }
}

MyPromise.prototype.then = function(onResolve, onReject) {
    let self = this;
    // 不传值的话默认是一个返回原值的函数
    onResolve = typeof onResolve === 'function' ? onResolve : (v => v);
    onReject = typeof onReject === 'function' ? onReject : (e => { throw e });
    if (self.state === FULFILLED) {
        return new MyPromise(function(resolve, reject) {
            setTimeout(() => {
                try {
                    let x = onResolve(self.val);
                    if (x instanceof MyPromise) {
                        x.then(resolve);
                    } else {
                        resolve(x);
                    }
                } catch(e) {
                    reject(e);
                }
            });
        });
    }

    if (self.state === REJECTED) {
        return new MyPromise(function(resolve, reject) {
            setTimeout(() => {
                try {
                    let x = onReject(self.val);
                    if (x instanceof MyPromise) {
                        x.then(resolve);
                    } else {
                        resolve(x);
                    }
                } catch(e) {
                    reject(e);
                }
            });
        });
    }

    if (self.state === PENDING) {
        return new MyPromise(function(resolve, reject) {
            self.resolveQueue.push((val) => {
                try {
                    let x = onResolve(val);
                    if (x instanceof MyPromise) {
                        x.then(resolve);
                    } else {
                        resolve(x);
                    }
                } catch(e) {
                    reject(e);
                }
            });
            self.rejectQueue.push((val) => {
                try {
                    let x = onReject(val);
                    if (x instanceof MyPromise) {
                        x.then(resolve);
                    } else {
                        resolve(x);
                    }
                } catch(e) {
                    reject(e);
                }
            });
        });
    }
}

MyPromise.prototype.catch = function(onReject) {
    return this.then(null, onReject);
}

MyPromise.all = function(promises) {
    return new MyPromise(function(resolve, reject) {
        let cnt = 0;
        let result = [];
        for (let i = 0; i < promises.length; i++) {
            promises[i].then(res => {
                result[i] = res;
                if (++cnt === promises.length) resolve(result);
            }, err => {
                reject(err);
            })
        }
    });
}

MyPromise.race = function(promises) {
    return new MyPromise(function(resolve, reject) {
        for (let i = 0; i < promises.length; i++) {
            promises[i].then(resolve, reject);
        }
    });
}

MyPromise.resolve = function(val) {
    return new MyPromise(function(resolve, reject) {
        resolve(val);
    });
}

MyPromise.reject = function(err) {
    return new MyPromise(function(resolve, reject) {
        reject(err);
    })
}
```

## 实现一个路由 - Hash
实现原理就是监听 url 的哈希值变化了
```html

<!DOCTYPE html>
<html>
<head>
  <title>hash 路由</title>
</head>
<body>
  <header>
    <a href="#home">首页</a>
    <a href="#center">个人中心页</a>
    <a href="#help">帮助页</a>
  </header>
  <section id="content"></section>
  <script>
    window.addEventListener('hashchange', (e) => {
      let content = document.getElementById('content');
      content.innerText = location.hash;
    })
</script>
</body>
</html>
```
## 路由实现 - history
```html
<!DOCTYPE html>
<html>
<head>
  <title>history 路由</title>
</head>
<body>
  <header>
    <a onclick="changeRoute(this)" data-path="home">首页</a>
    <a onclick="changeRoute(this)" data-path="center">个人中心页</a>
    <a onclick="changeRoute(this)" data-path="help">帮助页</a>
  </header>
  <section id="content"></section>
  <script>
    function changeRoute(route) {
      let path = route.dataset.path;
      /**
       * window.history.pushState(state, title, url)
       * state：一个与添加的记录相关联的状态对象，主要用于popstate事件。该事件触发时，该对象会传入回调函数。
       *        也就是说，浏览器会将这个对象序列化以后保留在本地，重新载入这个页面的时候，可以拿到这个对象。
       *        如果不需要这个对象，此处可以填 null。
       * title：新页面的标题。但是，现在所有浏览器都忽视这个参数，所以这里可以填空字符串。
       * url：新的网址，必须与当前页面处在同一个域。浏览器的地址栏将显示这个网址。
       */
      changePage(path);
      history.pushState({ content: path }, null, path);
    }
    /**
     * 调用 history.pushState() 或者 history.replaceState() 不会触发 popstate 事件。
     * 点击后退、前进按钮、或者在 js 中调用 history.back()、history.forward()、history.go() 方法会触发
     */
    window.addEventListener('popstate', (e) => {
      let content = e.state && e.state.content;
      changePage(content);
    });

    function changePage(pageContent) {
      let content = document.getElementById('content');
      content.innerText = pageContent;
    }
</script>
</body>
</html>
```


## 用css画一个立方体。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .container {
            position: relative;
            width: 300px;
            height: 300px;
            margin: 120px auto;
            transform-style: preserve-3d;
            transform: rotateX(-33.5deg) rotateY(45deg);
            transform-origin: 150px 150px;
            animation: rotate 3s infinite;
        }
        .container .page {
            position: absolute;
            width: 200px;
            height: 200px;
            text-align: center;
            line-height: 200px;
            color: #fff;
        }
        .container .page:first-child {
            background-color: rgba(0,0,0,.2);
        }
        .container .page:nth-child(2) {
            transform: rotateX(90deg);
            transform-origin: 0 0;
            transition: transform 3s;
            background-color: rgba(179, 15, 64, 0.6);
        }

        .container .page:nth-child(3) {
            transform: translateZ(200px);
            background-color: rgba(22, 160, 137, 0.7);
        }

        .container .page:nth-child(4) {
            transform: rotateX(-90deg);
            transform-origin: -200px 200px;
            background-color: rgba(210, 212, 56, 0.2);
        }
        .container .page:nth-child(5) {
            transform: rotateY(-90deg);
            transform-origin: 0 0;
            background-color: rgba(201, 23, 23, 0.6);
        }
        .container .page:nth-child(6) {
            transform: rotateY(-90deg) translateZ(-200px);
            transform-origin: 0 200px;
            background-color: rgba(16, 149, 182, 0.2);
        }

        @keyframes rotate {
            0% {
                transform: rotateX(-33.5deg) rotateY(45deg);
            }
            100% {
                transform: rotateX(-33.5deg) rotateY(405deg);
            }
        }
</style>
</head>
<body>
    <div class="container">
        <div class="page">A</div>
        <div class="page">B</div>
        <div class="page">C</div>
        <div class="page">D</div>
        <div class="page">E</div>
        <div class="page">F</div>
    </div>
</body>
</html>
```

## css 两栏布局
要求：垂直两栏，左边固定右边自适应
```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
    .outer {
        height: 100px;
        margin-bottom: 10px;
    }
    .left {
        background: tomato;
        height: 100px;
    }
    .right {
        background: gold;
        height: 100px;
    }
    /* 浮动 */
    .outer1 .left {
        width: 200px;
        float: left;
    }
    .outer1 .right {
        width: auto;
        margin-left: 200px;
    }
    /* flex */
    .outer2 {
        display: flex;
    }
    .outer2 .left {
        flex-grow: 0;
        flex-shrink: 0;
        flex-basis: 200px;
    }
    .outer2 .right {
        flex: auto; /* 1 1 auto */
    }
    /* position */
    .outer3 {
        position: relative;
    }
    .outer3 .left {
        position: absolute;
        width: 200px;
    }
    .outer3 .right {
        margin-left: 200px;
    }
    /* position again */
    .outer4 {
        position: relative;
    }
    .outer4 .left {
        width: 200px;
    }
    .outer4 .right {
        position: absolute;
        top: 0;
        left: 200px;
        right: 0;
    }
</style>
</head>
<!-- 左右两栏，左边固定，右边自适应 -->
<body>
    <div class="outer outer1">
        <div class="left">1-left</div>
        <div class="right">1-right</div>
    </div>
    <div class="outer outer2">
        <div class="left">2-left</div>
        <div class="right">2-right</div>
    </div>
    <div class="outer outer3">
        <div class="left">3-left</div>
        <div class="right">3-right</div>
    </div>
    <div class="outer outer4">
        <div class="left">4-left</div>
        <div class="right">4-right</div>
    </div>
</body>
</html>
```

##  css三栏布局
要求：垂直三栏布局，左右两栏宽度固定，中间自适应
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .outer, .left, .middle, .right {
            height: 100px;
            margin-bottom: 5px;
        }
        .left {
            background: tomato;
        }
        .middle {
            background: lightgreen;
        }
        .right {
            background: gold;
        }
        /* 左右分别设置绝对定位 中间设置外边距 */
        .outer1 {
            position: relative;
        }
        .outer1 .left {
            position: absolute;
            width: 100px;
        }
        .outer1 .middle {
            margin: 0 200px 0 100px;
        }
        .outer1 .right {
            position: absolute;
            width: 200px;
            top: 0;
            right: 0;
        }
        /* flex 布局 */
        .outer2 {
            display: flex;
        }
        .outer2 .left {
            flex: 0 0 100px;
        }
        .outer2 .middle {
            flex: auto;
        }
        .outer2 .right {
            flex: 0 0 200px;
        }
        /* 浮动布局 但是 html 中 middle要放到最后 */
        .outer3 .left {
            float: left;
            width: 100px;
        }
        .outer3 .right {
            float: right;
            width: 200px;
        }
        .outer3 .middle {
            margin: 0 200px 0 100px;
        }
</style>
</head>
<!-- 三栏布局 左右固定 中间自适应 -->
<body>
    <div class="outer outer1">
        <div class="left">1-left</div>
        <div class="middle">1-middle</div>
        <div class="right">1-right</div>
    </div>
    <div class="outer outer2">
        <div class="left">2-left</div>
        <div class="middle">2-middle</div>
        <div class="right">2-right</div>
    </div>
    <div class="outer outer3">
        <div class="left">3-left</div>
        <div class="right">3-right</div>
        <div class="middle">3-middle</div>
    </div>
</body>
</html>
```

## 圣杯布局 和 双飞翼布局
和三栏布局要求相同，不过中间列要写在前面保证优先渲染。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .outer, .left, .middle, .right {
            height: 100px;
            margin-bottom: 5px;
        }
        .left {
            background: tomato;
        }
        .middle {
            background: lightgreen;
        }
        .right {
            background: gold;
        }
        /* 圣杯布局 通过浮动和负边距实现 */
        .outer1 {
            padding: 0 200px 0 100px;
        }
        .outer1 .middle {
            width: 100%;
            float: left;
        }
        .outer1 .left {
            width: 100px;
            float: left;
            margin-left: -100%;
            position: relative;
            left: -100px;
        }
        .outer1 .right {
            width: 200px;
            float: left;
            margin-left: -200px;
            position: relative;
            left: 200px;
        }
        /* 双飞翼布局 */
        .outer2 .middle-wrapper {
            width: 100%;
            float: left;
        }
        .outer2 .middle {
            margin: 0 200px 0 100px;
        }
        .outer2 .left {
            width: 100px;
            float: left;
            margin-left: -100%;
        }
        .outer2 .right {
            width: 200px;
            float: left;
            margin-left: -200px;
        }
</style>
</head>
<!-- 三栏布局 左右固定 中间自适应 -->
<body>
    <!-- 圣杯布局 middle 最先 -->
    <div class="outer outer1">
        <div class="middle">圣杯-middle</div>
        <div class="left">圣杯-left</div>
        <div class="right">圣杯-right</div>
    </div>
    <!-- 双飞翼布局 middle 最先 多一层 div -->
    <div class="outer outer2">
        <div class="middle-wrapper">
            <div class="middle">双飞翼布局-middle</div>
        </div>
        <div class="left">双飞翼布局-left</div>
        <div class="right">双飞翼布局-right</div>
    </div>
</body>
</html>
```

## css 三角形
实现一个三角形
常见题目，通过 border 实现
```html

<!DOCTYPE html>
<html>
<head>
  <title>三角形</title>
  <style type="text/css">
    .box1, .box2, .box3, .box4 {
      height: 0px;
      width: 0px;
      float: left;
      border-style: solid;
      margin: 10px;
    }
    .box1 { /* 等腰直角 */
      border-width: 100px;
      border-color: tomato transparent transparent transparent;
    }
    .box2 { /* 等边 */
      border-width: 100px 173px;
      border-color: transparent tomato transparent transparent;
    }
    .box3 { /* 等腰 */
      border-width: 100px 80px;
      border-color: transparent transparent tomato transparent;
    }
    .box4 { /* 其他 */
      border-width: 100px 90px 80px 70px;
      border-color: transparent transparent transparent tomato;
    }
</style>
</head>
<body>
  <div class="box1"></div>
  <div class="box2"></div>
  <div class="box3"></div>
  <div class="box4"></div>
</body>
</html>

```

##  css 正方形
使用 css 实现一个宽高自适应的正方形
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
    <style>
      /* 都是像对于屏幕宽度的比例 */
      .square1 {
        width: 10%;
        height: 10vw;
        background: red;
      }
      /* margin/padding 百分比是相对父元素 width 的 */
      .square2 {
        width: 20%;
        height: 0;
        padding-top: 20%;
        background: orange;
      }
      /* 通过子元素 margin */
      .square3 {
        width: 30%;
        overflow: hidden; /* 触发 BFC */
        background: yellow;
      }
      .square3::after {
        content: '';
        display: block;
        margin-top: 100%; /* 高度相对于 square3 的 width */
      }
</style>
  </head>
  <!-- 画一个正方形 -->
  <body>
    <div class="square1"></div>
    <div class="square2"></div>
    <div class="square3"></div>
  </body>
</html>
```

## css 扇形
实现一个 1/4 圆、任意弧度的扇形
有多种实现方法，这里选几种简单方法（我看得懂的）实现。
```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
    /* 通过 border 和 border-radius 实现 1/4 圆 */
    .sector1 {
        height: 0;
        width: 0;
        border: 100px solid;
        border-radius: 50%;
        border-color: turquoise tomato tan thistle;
    }
    /* 类似三角形的做法加上父元素 overflow: hidden; 也可以实现任意弧度圆 */
    .sector2 {
        height: 100px;
        width: 200px;
        border-radius: 100px 100px 0 0;
        overflow: hidden;
    }
    .sector2::after {
        content: '';
        display: block;
        height: 0;
        width: 0;
        border-style: solid;
        border-width: 100px 58px 0;
        border-color: tomato transparent;
        transform: translate(42px,0);
    }
    /* 通过子元素 rotateZ 和父元素 overflow: hidden 实现任意弧度扇形（此处是60°） */
    .sector3 {
        height: 100px;
        width: 100px;
        border-top-right-radius: 100px;
        overflow: hidden;
        /* background: gold; */
    }
    .sector3::after {
        content: '';
        display: block;
        height: 100px;
        width: 100px;
        background: tomato;
        transform: rotateZ(-30deg);
        transform-origin: left bottom;
    }
    /* 通过 skewY 实现一个60°的扇形 */
    .sector4 {
        height: 100px;
        width: 100px;
        border-top-right-radius: 100px;
        overflow: hidden;
    }
    .sector4::after {
        content: '';
        display: block;
        height: 100px;
        width: 100px;
        background: tomato;
        transform: skewY(-30deg);
        transform-origin: left bottom;
    }
    /* 通过渐变设置60°扇形 */
    .sector5 {
        height: 200px;
        width: 200px;
        background: tomato;
        border-radius: 50%;
        background-image: linear-gradient(150deg, transparent 50%, #fff 50%),
        linear-gradient(90deg, #fff 50%, transparent 50%);
    }
</style>
</head>
<body>
    <div style="display: flex; justify-content: space-around;">
        <div class="sector1"></div>
        <div class="sector2"></div>
        <div class="sector3"></div>
        <div class="sector4"></div>
        <div class="sector5"></div>
    </div>
</body>
</html>
```

## 水平垂直居中
实现子元素的水平垂直居中
```html
<!DOCTYPE html>
<html>
<head>
  <title>水平垂直居中</title>
  <style type="text/css">
    .outer {
      height: 200px;
      width: 200px;
      background: tomato;
      margin: 10px;
      float: left;
      position: relative;
    }
    .inner {
      height: 100px;
      width: 100px;
      background: black;
    }
    /*
     * 通过 position 和 margin 居中
     * 缺点：需要知道 inner 的长宽
     */
    .inner1 {
      position: absolute;
      top: 50%;
      left: 50%;
      margin-top: -50px;
      margin-left: -50px;
    }
    /*
     * 通过 position 和 margin 居中 (2
     */
    .inner2 {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      margin: auto;
    }
    /*
     * 通过 flex 进行居中
     */
    .outer3 {
      display: flex;
      justify-content: center;
      align-items: center;
    }
    /**
     * 通过 position 和 transform 居中
     */
    .inner4 {
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      position: absolute;
    }
</style>
</head>
<body>
  <div class="outer outer1">
    <div class="inner inner1"></div>
  </div>
  <div class="outer outer2">
    <div class="inner inner2"></div>
  </div>
  <div class="outer outer3">
    <div class="inner inner3"></div>
  </div>
  <div class="outer outer4">
    <div class="inner inner4"></div>
  </div>
</body>
</html>
```
## 清除浮动
要求：清除浮动
可以通过 clear:both 或 BFC 实现
```html

<!DOCTYPE html>
<html>
<head>
  <title>清除浮动</title>
  <style type="text/css">
    .outer {
      width: 200px;
      background: tomato;
      margin: 10px;
      position: relative;
    }
    .inner {
      height: 100px;
      width: 100px;
      background: pink;
      margin: 10px;
      float: left;
    }
    /* 伪元素 */
    .outer1::after {
      content: '';
      display: block;
      clear: both;
    }
    /* 创建 BFC */
    .outer2 {
      overflow: hidden;
    }
</style>
</head>
<body>
  <div class="outer outer1">
    <div class="inner"></div>
  </div>
  <div class="outer outer2">
    <div class="inner"></div>
  </div>
</body>
</html>
```

## 弹出框
使用 CSS 写一个弹出框效果
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .bg {
            height: 666px;
            width: 100%;
            font-size: 60px;
            text-align: center;
        }
        .dialog {
            z-index: 999;
            position: fixed;
            top: 0;
            right: 0;
            bottom: 0;
            left: 0;
            background: rgba(0, 0, 0, 0.5);
        }
        .dialog .content {
            min-height: 300px;
            width: 600px;
            background: #fff;
            border-radius: 5px;
            border: 1px solid #ebeef5;
            box-shadow: 0 2px 12px 0 rgba(0,0,0,.1);
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
</style>
</head>
<body>
    <div class="bg">
        页面内容
    </div>
    <div class="dialog">
        <div class="content">
            弹出框
        </div>
    </div>
</body>
</html>
```

## 导航栏
要求：一个 div 内部放很多水平 div ，并可以横向滚动。
```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=div, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        body,html {
            margin: 0;
            padding: 0;
        }
        /* flex 实现 */
        /* .nav {
            display: flex;
            height: 30px;
            border: 1px solid #000;
            padding: 3px;
            overflow-x: auto;
        }
        .nav::-webkit-scrollbar {
            display: none;
        }
        .item {
            flex: 0 0 200px;
            height: 30px;
            margin-right: 5px;
            background: gray;
        } */
        /* inline-block 和 white-space: nowrap; 实现 */
        .nav {
            height: 30px;
            padding: 3px;
            border: 1px solid #000;
            overflow-x: auto;
            white-space: nowrap;
        }
        .nav::-webkit-scrollbar {
            display: none;
        }
        .item {
            display: inline-block;
            width: 200px;
            height: 30px;
            margin-right: 5px;
            background: gray;
        }
</style>
</head>
<!-- 水平滚动导航栏 -->
<body>
    <div class="nav">
        <div class="item">item1</div>
        <div class="item">item2</div>
        <div class="item">item3</div>
        <div class="item">item4</div>
        <div class="item">item5</div>
        <div class="item">item6</div>
        <div class="item">item7</div>
        <div class="item">item8</div>
        <div class="item">item9</div>
    </div>
</body>
</html>
```


