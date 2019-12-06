# 经常使用的代码片段集锦


## 防抖 节流
在进行窗口的resize、scroll，输入框内容校验等操作时，如果事件处理函数调用的频率无限制，会加重浏览器的负担，导致用户体验非常糟糕。此时我们可以采用debounce（防抖）和throttle（节流）的方式来减少调用频率，同时又不影响实际效果。 
函数防抖
函数防抖（debounce）：当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，如果设定的时间到来之前，又一次触发了事件，就重新开始延时。 
```javascript
/ 防抖
function debounce(fn, wait) {    
    var timeout = null;    
    return function() {        
        if(timeout !== null)   clearTimeout(timeout);        
        timeout = setTimeout(fn, wait);    
    }
}
// 处理函数
function handle() {    
    console.log(Math.random()); 
}
// 滚动事件
window.addEventListener('scroll', debounce(handle, 1000));
```
当持续触发scroll事件时，事件处理函数handle只在停止滚动1000毫秒之后才会调用一次，也就是说在持续触发scroll事件的过程中，事件处理函数handle一直没有执行

函数节流
函数节流（throttle）：当持续触发事件时，保证一定时间段内只调用一次事件处理函数。节流通俗解释就比如我们水龙头放水，阀门一打开，水哗哗的往下流，秉着勤俭节约的优良传统美德，我们要把水龙头关小点，最好是如我们心意按照一定规律在某个时间间隔内一滴一滴的往下滴
`节流throttle代码（时间戳）：`
```javascript
var throttle = function(func, delay) {            
　　var prev = Date.now();            
　　return function() {                
　　　　var context = this;                
　　　　var args = arguments;                
　　　　var now = Date.now();                
　　　　if (now - prev >= delay) {                    
　　　　　　func.apply(context, args);                    
　　　　　　prev = Date.now();                
　　　　}            
　　}        
}        
function handle() {            
　　console.log(Math.random());        
}        
window.addEventListener('scroll', throttle(handle, 1000));
```
`节流throttle代码（定时器）：`
```javascript
// 节流throttle代码（定时器）：
var throttle = function(func, delay) {            
    var timer = null;            
    return function() {                
        var context = this;               
        var args = arguments;                
        if (!timer) {                    
            timer = setTimeout(function() {                        
                func.apply(context, args);                        
                timer = null;                    
            }, delay);                
        }            
    }        
}        
function handle() {            
    console.log(Math.random());        
}        
window.addEventListener('scroll', throttle(handle, 1000));

```
当触发事件的时候，我们设置一个定时器，再次触发事件的时候，如果定时器存在，就不执行，直到delay时间后，定时器执行执行函数，并且清空定时器，这样就可以设置下个定时器。当第一次触发事件时，不会立即执行函数，而是在delay秒后才执行。而后再怎么频繁触发事件，也都是每delay时间才执行一次。当最后一次停止触发后，由于定时器的delay延迟，可能还会执行一次函数。
节流中用时间戳或定时器都是可以的。更精确地，可以用时间戳+定时器，当第一次触发事件时马上执行事件处理函数，最后一次触发事件后也还会执行一次事件处理函数。

`节流throttle代码（时间戳+定时器）：`
```javascript
// 节流throttle代码（时间戳+定时器）：
var throttle = function(func, delay) {     
    var timer = null;     
    var startTime = Date.now();     
    return function() {             
        var curTime = Date.now();             
        var remaining = delay - (curTime - startTime);             
        var context = this;             
        var args = arguments;             
        clearTimeout(timer);              
        if (remaining <= 0) {                    
            func.apply(context, args);                    
            startTime = Date.now();              
        } else {                    
            timer = setTimeout(func, remaining);              
        }      
    }
}
function handle() {      
    console.log(Math.random());
} 
window.addEventListener('scroll', throttle(handle, 1000));
```
在节流函数内部使用开始时间startTime、当前时间curTime与delay来计算剩余时间remaining，当remaining<=0时表示该执行事件处理函数了（保证了第一次触发事件就能立即执行事件处理函数和每隔delay时间执行一次事件处理函数）。如果还没到时间的话就设定在remaining时间后再触发 （保证了最后一次触发事件后还能再执行一次事件处理函数）。当然在remaining这段时间中如果又一次触发事件，那么会取消当前的计时器，并重新计算一个remaining来判断当前状态。
### 总结
函数防抖：将几次操作合并为一此操作进行。原理是维护一个计时器，规定在delay时间后触发函数，但是在delay时间内再次触发的话，就会取消之前的计时器而重新设置。这样一来，只有最后一次操作能被触发。
函数节流：使得一定时间内只触发一次函数。原理是通过判断是否到达一定时间来触发函数。
区别： 函数节流不管事件触发有多频繁，都会保证在规定时间内一定会执行一次真正的事件处理函数，而函数防抖只是在最后一次事件后才触发一次函数。 比如在页面的无限加载场景下，我们需要用户在滚动页面时，每隔一段时间发一次 Ajax 请求，而不是在用户停下滚动页面操作时才去请求数据。这样的场景，就适合用节流技术来实现。

##  [添加淘宝镜像](http://npm.taobao.org/)
```shell
// 1, 修改 下载仓库为淘宝镜像
　 npm config set registry http://registry.npm.taobao.org/

   npm config get registry  //查看当前npm的registry

// 2, 如果要发布自己的镜像需要修改回来

　 npm config set registry https://registry.npmjs.org/

//3, 安装cnpm
　npm install -g cnpm --registry=https://registry.npm.taobao.org

```

## h5获取地理信息
```javascript 
        // H5 获取当前位置经纬度
        var loc_lon = '', loc_lat = ''; // 经度,纬度
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(function (position) {
                console.log('回调执行====开始====', position)
                loc_lon = position.coords.longitude;
                loc_lat = position.coords.latitude;
                console.log('回调执行==结束==h5经度：' + loc_lon + '  h5纬度：' + loc_lat);
            });
        } else {
            console.log("您的设备不支持定位功能");
        }
```


## 获取摄像头视频音频流
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>获取摄像头</title>
    <style>
        .contianer {
            text-align: center;
        }
    </style>
</head>
<body>
    <div class="contianer">
        <video id="displayF"  height="400px"  autoplay></video>
    </div>
    <script>
        let videoF = document.getElementById('displayF');
        navigator.mediaDevices.getUserMedia({
            video: true,
            audio: true
        }).then(async function(stream) {
            // videoF.src = window.URL.createObjectURL(stream); 
            videoF.srcObject = stream;
        }); 
    </script>
</body>
</html>

```


## 一分钟搭建静态服务器

```javascript
const serve = require('koa-static');
const Koa = require('koa');
const app = new Koa();

// $ GET /package.json
app.use(serve('.')); 
app.listen(8000);

console.log('listening on port 8000');
```

## git修改文件夹/文件名大小写敏感问题解决
### 问题描述 
git项目中出现了相同名字的、大小写不同的文件夹，是因为Windows环境下git配置ignorecase默认为true，不区分大小写，而Linux环境区分。
如果本地分支在Windows，远程分支在Linux，那么当你把一个文件夹的小写改为大写，commit是不会体现这个变化，这样大写的文件夹就提交到了Linux服务器上，服务器会认为这是不同文件夹，因而出现了2份一样的文件夹，而里面的文件，可能一样，也可能不一样。
如何解决Git的大小不敏感问题呢？
方案一：设置Git大小写敏感：
```shell
//查看core.ignorecase 的值
git config core.ignorecase
//设置 大小写敏感
git config core.ignorecase false
```
方案二：先删除文件，再添加进去（需要先备份文件夹）：
```shell
git rm ; git add  ;  git commit -m "rename file"
```


## 统计页面加载
```javascript
    window.logInfo = {};  //统计页面加载时间
    window.logInfo.openTime = performance.timing.navigationStart;
    window.logInfo.whiteScreenTime = +new Date() - window.logInfo.openTime;
    document.addEventListener('DOMContentLoaded',function (event) {
        window.logInfo.readyTime = +new Date() - window.logInfo.openTime;
    });
    
    window.onload = function () {
        window.logInfo.allloadTime = +new Date() - window.logInfo.openTime;
        window.logInfo.nowTime = new Date().getTime();
        var timname = {
            whiteScreenTime: '白屏时间',
            readyTime: '用户可操作时间',
            allloadTime: '总下载时间',
            mobile: '使用设备',
            nowTime: '时间',
        };
        var logStr = '';
        for (var i in timname) {
            console.warn(timname[i] + ':' + window.logInfo[i] + 'ms');
            if (i === 'mobile') {
                logStr += '&' + i + '=' + window.logInfo[i];
            } else {
                logStr += '&' + i + '=' + window.logInfo[i];
            }
        }
        // (new Image()).src = '/action?' + logStr; 上传信息
    }; 
    
    // 统计设备信息      
    window.logInfo.mobile = mobileType();
    function mobileType() {
        var u = navigator.userAgent, app = navigator.appVersion;
        var type =  {// 移动终端浏览器版本信息
            ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
            iPad: u.indexOf('iPad') > -1, //是否iPad
            android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或者uc浏览器
            iPhone: u.indexOf('iPhone') > -1 || u.indexOf('Mac') > -1, //是否为iPhone或者QQHD浏览器
            trident: u.indexOf('Trident') > -1, //IE内核
            presto: u.indexOf('Presto') > -1, //opera内核
            webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
            gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
            mobile: !!u.match(/AppleWebKit.*Mobile/i) || !!u.match(/MIDP|SymbianOS|NOKIA|SAMSUNG|LG|NEC|TCL|Alcatel|BIRD|DBTEL|Dopod|PHILIPS|HAIER|LENOVO|MOT-|Nokia|SonyEricsson|SIE-|Amoi|ZTE/), //是否为移动终端
            webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
        };
        var lists = Object.keys(type);
        for(var i = 0; i < lists.length; i++) {
            if(type[lists[i]]) {
            return lists[i];
            }
        }  
    }

    var defaults = {
        msg:'',  // 错误的具体信息
        url:'',  // 错误所在的url
        line:'', // 错误所在的行
        col:'',  // 错误所在的列
        nowTime: '',// 时间
    };
                
    window.onerror = function(msg,url,line,col,error) {
        
        col = col || (window.event && window.event.errorCharacter) || 0;

        defaults.url = url;
        defaults.line = line;
        defaults.col =  col;
        defaults.nowTime = new Date().getTime();

        if (error && error.stack){
            // 如果浏览器有堆栈信息，直接使用
            defaults.msg = error.stack.toString();

        } else if (arguments.callee){
            // 尝试通过callee拿堆栈信息
            var ext = [];
            var fn = arguments.callee.caller;
            var floor = 3;  
            while (fn && (--floor>0)) {
                ext.push(fn.toString());
                if (fn  === fn.caller) {
                    break;
                }
                fn = fn.caller;
            }
            ext = ext.join(",");
            defaults.msg = error.stack.toString();
        }
        var str = ''
        for(var i in defaults) {
            // console.log(i,defaults[i]);
            if(defaults[i] === null || defaults[i] === undefined) {
                defaults[i] = 'null'; 
            }
            str += '&'+ i + '=' + defaults[i].toString();
        }
        srt = str.replace('&', '').replace('\n','').replace(/\s/g, '');
        console.log(srt);
        // (new Image()).src = '/error?' + srt;  上传错误
    }
```