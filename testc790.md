sleep 8
v4l2-ctl --set-edid=file=1080P50EDID.txt --fix-edid-checksums
v4l2-ctl --set-dv-bt-timings query
gst-launch-1.0 -vvv v4l2src ! "video/x-raw,framerate=30/1,format=UYVY,colorimetry=(string)bt601" ! videoconvert ! autovideosink alsasrc device=hw:4 ! audioconvert ! avenc_aac bitrate=48000 ! aacparse ! avdec_aac! audioconvert ! queue ! alsasink device=hw:3,0


方法如下：
1. 进入/home/pi/.config 路径
cd /home/pi/.config
2. 找到autostart 路径，没有就创建一个
mkdir autostart
3. 然后进入autostart 路径
cd autostart
4. 在autostart 中创建一个.desktop 尾缀文件，例如test.desktop
touch test.desktop
5. 该文件中输入：
 [Desktop Entry]
 Name=PChost
 Comment=Python Program
 Exec=lxterminal -e sudo bash /home/pi/Desktop/testc780.sh
 Icon=/home/pi/python_games/4row_black.png
 Terminal=false
 MultipleArgs=false
 Type=Application
 Categories=Application;Development;
 StartupNotify=true

其中Exec那行就是关键。这行是打开了lxterminal，并让命令行执行了 sudo python3 /home/pi/Documents/Python/rinf.py。我这个程序用的点灯的物联网模块，要求root权限，所以加了sudo.
6、重启，连上VNC，可以看到命令行界面已经打开，程序也在里面执行，print的信息也能正常显示了。
