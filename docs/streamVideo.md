``` javascript
asyncvideoStream() {
    letlocalStream = awaitnavigator.mediaDevices.getUserMedia({
        video: true
    })
    letvideo = document.querySelector('video')
    video.srcObject = localStream
    video.onloadedmetadata = function(e) {
        video.play()
    }
    letmediaRecorder = newMediaRecorder(localStream);
    mediaRecorder.ondataavailable = function(blob) {
        console.log(blob.data)
    }
    mediaRecorder.start(1000)
}
```

找到方法了，getUserMedia返回的stream需要用MediaRecorder包装一下。
监听ondataavailable事件，然后执行start方法。这样就可以根据固定的时间间隔取得视频流了。
