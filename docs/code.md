## 经常使用的代码片段集锦


###  [添加淘宝镜像](http://npm.taobao.org/)
```shell
// 1, 修改 下载仓库为淘宝镜像
　 npm config set registry http://registry.npm.taobao.org/

   npm config get registry  //查看当前npm的registry

// 2, 如果要发布自己的镜像需要修改回来

　 npm config set registry https://registry.npmjs.org/

//3, 安装cnpm
　npm install -g cnpm --registry=https://registry.npm.taobao.org

```

### 获取摄像头视频音频流
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


### 一分钟搭建静态服务器

```javascript
const serve = require('koa-static');
const Koa = require('koa');
const app = new Koa();

// $ GET /package.json
app.use(serve('.')); 
app.listen(8000);

console.log('listening on port 8000');
```

#### 统计页面加载
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