# macos 平台，采集视频流

## 1. 用Gst命令行采集视频流

要求： 

使用制定摄像头

30帧每秒

分辨率1280x720

用mjpg编码

gst-launch-1.0 -v autovideosrc ! videoconvert ! video/x-raw,framerate=30/1,width=1280,height=720 ! jpegenc ! rtpjpegpay ! udpsink host=127.0.0.1 port=5000

这里不能使用127.0.0.1，需要用外网地址，否则无法接收

## 2. 视频接收

用gst命令行接收视频流

直接播放

gst-launch-1.0 -v udpsrc port=5000 caps="application/x-rtp,media=(string)video,clock-rate=(int)90000,encoding-name=(string)JPEG" ! rtpjpegdepay ! jpegdec ! autovideosink