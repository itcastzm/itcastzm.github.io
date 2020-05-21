# ffmpeg
## 官网
http://ffmpeg.org

## 视频格式转换
### 把用iPhone拍的.MOV文件转成.avi文件
```shell
ffmpeg -i D:\Media\IMG_0873.MOV   D:\Media\output.avi
```
意思是，把D:\Media目录下的源文件IMG_0873.MOV（视频：h.264，音频：aac）转换成output.avi（编码格式自动选择为：视频mpeg4，音频mp3），目标文件仍然保存到D:\Media目录下。

问题来了：我想自己指定编码格式，怎么办呢？一种方法是，通过目标文件的扩展名（.flv、.mpg、.mp4、.wmv等）来控制，比如：
```shell
ffmpeg -i D:\Media\IMG_0873.MOV D:\Media\output2.flv
```
另一种方法是通过-c:v参数来控制，比如我想输出的视频格式是H.265（警告：编码时间会比较长哦）。命令行如下：
```shell
ffmpeg -i D:\Media\IMG_0873.MOV -c:v libx265 D:\Media\output265.avi
```
注：可以先用ffmpeg -encoders命令查看一下所有可选的编码格式。

不再深究了，我们继续。我发现源文件的图像帧尺寸是1920x 1080，我不需要这么大——能有720 x 480就够了。于是，就要用上-s参数了。为了保证图像缩放后的质量，最好加上码流参数-b:v。如下：
```shell
ffmpeg -i D:\Media\IMG_0873.MOV -s 720x480 -b:v 1500k D:\Media\output2.avi
```
还可以更简单一点，使用-target参数匹配行业标准，参数值可以是vcd、svcd、dvd、dv、dv50等，可能还需要加上电视制式作为前缀（pal-、ntsc-或film-）。如下：
```shel
ffmpeg -i D:\Media\IMG_0873.MOV -target pal-dvd D:\Media\output2dvd.avi
```
又来一个问题：我发现用手机拍的视频中，有些是颠倒的，我想让它顺时针旋转90度。这时候，可以使用-vf参数加入一个过滤器，如下：
```shell
ffmpeg -i D:\Media\IMG_0873.MOV -vf "rotate=90*PI/180" D:\Media\output3.avi
```
注：如果想逆时针旋转90度，90前面加个负号就可以了。

如果我只需要从源视频里截取一小段，怎么办呢？比如从第2秒的地方开始，往后截取10秒钟。命令行可以这样：
```shel
ffmpeg -ss 2 -t 10 -i D:\Media\IMG_0873.MOV D:\Media\output4.avi
```
注：这种情况下，-ss和-t参数必须放在-i前面，表示是限定后面跟着的输入文件的。

应用场景2：视频合成

我发现，用手机拍的视频有时候背景噪音比较大。怎么把噪音去掉，换成一段美妙的音乐呢？使用FFmpeg也能轻易做到。

第一步：把源文件里的音频去掉，生成一个临时文件tmp.mov
```shel
ffmpeg -i D:\Media\IMG_0873.MOV -vcodec copy -an D:\Media\tmp.mov
```
注：-vcodeccopy的意思是对源视频不解码，直接拷贝到目标文件；-an的意思是将源文件里的音频丢弃。

第二步：把这个无声的视频文件（tmp.mov）与一个音乐文件（music.mp3）合成，最终生成output.mov
```shel
ffmpeg -i D:\Media\tmp.mov -ss 30 -t 52 -i D:\Media\music.mp3 -vcodec copy D:\Media\output5.avi
```
为了保证良好的合成效果，音乐时长必须匹配视频时长。这里我们事先知道视频时长为52秒，于是截取music.mp3文件的第30秒往后的52秒与视频合成。另外，为了保证音频时长截取的准确性，我们这里没有使用-acodec copy，而是让音频重新转码。

还有一种情况：我们希望在一段视频上叠加一张图片。可以简单实现如下：
```shell
ffmpeg -i D:\Media\IMG_0873.MOV -i D:\Media\logo.png -filter_complex 'overlay' D:\Media\output6.avi
```
应用场景3：视频播放

格式转换或合成之后，我们需要试着播放一下。播放器的选择很多。这里顺手用ffplay工具也行：
``` shell
ffplay -i D:\Media\output6.avi
```

应用场景4：获取视频信息

有时候，我只是想看看这个视频文件的格式信息。可以用ffprobe工具：
```shel
ffprobe -i D:\Media\IMG_0873.MOV
```

其他应用

FFmpeg的功能非常强大。关键是要理解各种参数的意义，并且巧妙搭配。必要的话，就把在线文档完整读一遍吧：http://www.ffmpeg.org/ffmpeg.html


## 转换音频

### 将 mp3 ape 转换成 awv
```shell
ffmpeg -i input.mp3  -f  wav  output.wav

ffmpeg -i input.ape  -f  wav  output.wav
```

## FFmpeg获取DirectShow设备数据（摄像头，录屏）
### 列设备
```shell
ffmpeg -list_devices true -f dshow -i dummy
```
命令执行后输出的结果如下（注：中文的设备会出现乱码的情况）。列表显示设备的名称很重要，输入的时候都是使用“-f dshow -i video="{设备名}"”的方式。

下文的测试中，使用其中的两个视频输入："Integrated Camera"和"screen-capture-recorder"。

 注：音频设备出现乱码，这个问题的解决方法会随后提到。

### 获取摄像头数据（保存为本地文件或者发送实时流）
编码为H.264，保存为本地文件
下面这条命令，实现了从摄像头读取数据并编码为H.264，最后保存成mycamera.mkv。
```shell
ffmpeg -f dshow -i video="Integrated Camera" -vcodec libx264 mycamera.mkv
```

### 直接播放摄像头的数据
使用ffplay可以直接播放摄像头的数据，命令如下：
```shell
ffplay -f dshow -i video="Integrated Camera"
```
注：除了使用DirectShow作为输入外，使用VFW也可以读取到摄像头的数据，例如下述命令可以播放摄像头数据：
```shell
ffplay -f vfwcap -i 0
```
此外，可以使用FFmpeg的list_options查看设备的选项：
```shell
ffmpeg -list_options true -f dshow -i video="Integrated Camera"
```

可以通过输出信息设置摄像头的参数。

例如，设置摄像头分辨率为1280x720
```shell
ffplay -s 1280x720 -f dshow -i video="Integrated Camera"
```
设置分辨率为424x240
```shell
ffplay -s 424x240 -f dshow -i video="Integrated Camera"
```

### 编码为H.264，发布UDP
下面这条命令，实现了：获取摄像头数据->编码为H.264->封装为UDP并发送至组播地址。
```shell
ffmpeg -f dshow -i video="Integrated Camera" -vcodec libx264 -preset:v ultrafast -tune:v zerolatency -f h264 udp://233.233.233.223:6666
```
注1：考虑到提高libx264的编码速度，添加了-preset:v ultrafast和-tune:v zerolatency两个选项。
注2：高分辨率的情况下，使用UDP可能出现丢包的情况。为了避免这种情况，可以添加–s 参数（例如-s 320x240）调小分辨率。
### 编码为H.264，发布RTP
下面这条命令，实现了：获取摄像头数据->编码为H.264->封装为RTP并发送至组播地址。
```shell
ffmpeg -f dshow -i video="Integrated Camera" -vcodec libx264 -preset:v ultrafast -tune:v zerolatency -f rtp rtp://233.233.233.223:6666>test.sdp
```
注1：考虑到提高libx264的编码速度，添加了-preset:v ultrafast和-tune:v zerolatency两个选项。
注2：结尾添加“>test.sdp”可以在发布的同时生成sdp文件。该文件可以用于该视频流的播放。
### 编码为H.264，发布RTMP
下面这条命令，实现了：获取摄像头数据->编码为H.264->并发送至RTMP服务器。
```shell
ffmpeg -f dshow -i video="Integrated Camera" -vcodec libx264 -preset:v ultrafast -tune:v zerolatency -f flv rtmp://localhost/oflaDemo/livestream
```
### 编码为MPEG2，发布UDP
与编码为H.264类似，指明-vcodec即可。
```shell
ffmpeg -f dshow -i video="Integrated Camera" -vcodec mpeg2video -f mpeg2video udp://233.233.233.223:6666
```
播放MPEG2的UDP流如下。指明-vcodec为mpeg2video即可
```shell
ffplay -vcodec mpeg2video udp://233.233.233.223:6666
```
###  屏幕录制（Windows平台下保存为本地文件或者发送实时流）
Linux下使用FFmpeg进行屏幕录制相对比较方便，可以使用x11grab，使用如下的命令：
```shell
ffmpeg -f x11grab -s 1600x900 -r 50 -vcodec libx264 –preset:v ultrafast –tune:v zerolatency -crf 18 -f mpegts udp://localhost:1234
```
详细时使用方式可以参考这篇文章：DesktopStreaming With FFmpeg for Lower Latency

Linux录屏在这里不再赘述。在Windows平台下屏幕录像则要稍微复杂一些。在Windows平台下，使用-dshow取代x11grab。一句话介绍：注册录屏dshow滤镜（例如screen-capture-recorder），然后通过dshow获取录屏图像然后编码处理。

因此，在使用FFmpeg屏幕录像之前，需要先安装dshow滤镜。在这里推荐一个软件：screen capture recorder。安装这个软件之后，就可以通过FFmpeg屏幕录像了。
screen capture recorder项目主页：

http://sourceforge.net/projects/screencapturer/

下载地址：

http://sourceforge.net/projects/screencapturer/files

下载完后，一路“Next”即可安装完毕。注意，需要Java运行环境（Java Runtime Environment），如果没有的话下载一个就行。

screen capture recorder本身就可以录屏，不过这里我们使用FFmpeg进行录屏。

### 编码为H.264，保存为本地文件
下面的命令可以将屏幕录制后编码为H.264并保存为本地文件
```shell
ffmpeg -f dshow -i video="screen-capture-recorder" -r 5 -vcodec libx264 -preset:v ultrafast -tune:v zerolatency MyDesktop.mkv
```
注：“-r 5”的意思是把帧率设置成5。         

最后得到的效果如下图。(略)

此外，也可以录声音，声音输入可以分成两种：一种是真人说话的声音，通过话筒输入；一种是虚拟的声音，即录屏的时候电脑耳机里的声音。下面两条命令可以分别录制话筒的声音和电脑耳机里的声音。

录屏，伴随话筒输入的声音
```shell
ffmpeg -f dshow -i video="screen-capture-recorder" -f dshow -i audio="鍐呰楹﹀厠椋?(Conexant 20672 SmartAudi" -r 5 -vcodec libx264 -preset:v ultrafast -tune:v zerolatency -acodec libmp3lame MyDesktop.mkv
```
上述命令有问题：audio那里有乱码，把乱码ANSI转UTF-8之后，开始测试不行，后来发现是自己疏忽大意，乱码部分转码后为“内装麦克风 ”，然后接可以正常使用了。因此，命令应该如下图所示：
```shell
ffmpeg -f dshow -i video="screen-capture-recorder" -f dshow -i audio="内装麦克风 (Conexant 20672 SmartAudi" -r 5 -vcodec libx264 -preset:v ultrafast -tune:v zerolatency -acodec libmp3lame MyDesktop.mkv 
```
注：
如果不熟悉ANSI转码UTF-8的话，还有一种更简单的方式查看设备的名称。即不使用FFmpeg查看系统DirectShow输入设备的名称，而使用DirectShow SDK自带的工具GraphEdit（或者网上下一个GraphStudioNext）查看输入名称。

打开GraphEdit选择“图像->插入滤镜” 


### 另一种屏幕录制的方式
最近发现FFmpeg还有一个专门用于Windows下屏幕录制的设备：gdigrab。
gdigrab是基于GDI的抓屏设备，可以用于抓取屏幕的特定区域。在这里记录一下gdigrab的用法。
gdigrab通过设定不同的输入URL，支持两种方式的屏幕抓取：
（1）“desktop”：抓取整张桌面。或者抓取桌面中的一个特定的区域。
（2）“title={窗口名称}”：抓取屏幕中特定的一个窗口。
下面举几个例子。
最简单的抓屏： 
```shell
ffmpeg -f gdigrab -i desktop out.mpg
```
从屏幕的（10,20）点处开始，抓取640x480的屏幕，设定帧率为5
```shell
ffmpeg -f gdigrab -framerate 5 -offset_x 10 -offset_y 20 -video_size 640x480 -i desktop out.mpg
```



## 参考
雷霄骅（天妒英才）
https://blog.csdn.net/leixiaohua1020