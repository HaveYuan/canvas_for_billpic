# canvas_for_billpic
小程序使用canvas画分享海报


由于小程序的限制，我们不能很方便地在微信内直接分享小程序到朋友圈，所以普遍的做法是生成一张带有小程序分享码的分享海报，再将海报保存到手机相册，有两种方法可以生成分享海报，第一种是让后台生成然后返回图片链接，这一种方法比较简单，只需要传后台所需要的参数就行了，今天给大家介绍的是第二种方法，用canvas生成分享海报。

## 效果
![](https://mmbiz.qpic.cn/mmbiz_gif/B8lDVIjhMnnnvRDiaShKcxlBJtSXfgG6VoTGOQKyC75vAO1rCp0EcZgWbCPFWNaYgoNxIEmjvKMG42wmNZg8Gmw/640?wx_fmt=gif)

## 主要步骤
1. 把海报样式用标签先写好，方便画图时可以比对
1. 用canvas进行画图，canvas要注意定好宽高
1. canvas利用wx.canvasToTempFilePath这个api将canvas转化为图片
1. 将转化好的图片链接放入image标签里
1. 再利用wx.saveImageToPhotosAlbum保存图片

## 坑点
1. 用canvas进行画图的时候要注意画出来的图的大小一定要是你用标签写好那个样式的两倍大小，比如你的海报大小是400*600的大小，那你用canvas画的时候大小就要是800*1200，宽高可以写在样式里，如果你画出来的图跟你海报图是一样的大小的话生成的图片是会很模糊的，所以才需要放大两倍。



2. 画图的时候要注意尺寸的转化，如果你是用rpx做单位的话，就要对单位进行转化，因为canvas提供的方法都是经px为单位的，所以这一点要注意一下，px转rpx的公式是w/750*z*2，w是手机屏幕宽度screenWidth，可以通过wx.getSystemInfo获取，z是你需要画图的单位，2就是乘以两倍大小。



3. 图片来源问题，因为canvas不支持网络图片画图，所以你的图片要么是固定的，如果不是固定的，那就要用wx.downloadFile下载后得到一个临时路径才行



4. 小程序码问题，小程序需要后台请求接口后返回一个二进制的图片，因为二进制图片canvas也是不支持的，所以也是要用wx.downloadFile下载后得到一个临时路径，或者可以叫后台直接返回一个小程序码的路径给你



5. 这里保存的时候是有个授权提醒的，如果拒绝的话再次点击就没有反应了，所以这里我做了一个判断是否有授权的，如果没有就弹窗提醒，确认的话会打开设置页面，确认授权后再次返回就行了，这里有个坑注意下，就是之前拒绝后再进入设置页面确认授权返回页面时保存图片会不成功，官方还没解决，我是加了个setTimeOut处理的，详情可以看这里 https://developers.weixin.qq.com/community/develop/doc/000c46600780f0fa68d7eac345a400

## 代码实现
![](https://mmbiz.qpic.cn/mmbiz_png/B8lDVIjhMnnnvRDiaShKcxlBJtSXfgG6VsGpDyeI86Ez6jOUY8xUPpYlH806cFibgdjq0G7rXyOvuyxwovZDmSvg/640?wx_fmt=png)

这里图片我先用的是网上的链接，实际项目中是后台返回的数据，这个可以自行处理，这里只是为了演示方便，生成临时路径的方法我这里是分别定义了一个方法，其实可以合成一个方法的，只是生成小程序码时如果要传入参数要注意一下。

绘图方法是drawImg，这里截一部分，详细的可以看代码

## 不足
由于在实际项目中返回的图片宽高是不固定的，但是canvas画出来的又需要固定宽高，所以分享图会有图片变形的问题，使用drawImage里的参数也不能解决，如果各位有比较好的方案可以一起讨论一下。

