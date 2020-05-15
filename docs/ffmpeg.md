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