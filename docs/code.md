## 经常使用的代码片段集锦



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