准备工作 
开发环境：pc: Ubuntu 16.04 (64 bit)    pi3 : Raspbian Jessie 2017-06-21 (这个不使用lite原因是缺少opengl库)

//(这些是需要你在pc上进行的)---------------------------------------------------------------------------
1：安装必要的软件包
    sudo apt-get install build-essential sshpass git python pkg-config re2c gperf bison flex ninja python ruby gcc-multilib g++-multilib
2：创建 raspi文件夹
    mkdir ~/raspi
3：下载rpi-buildqt 的脚本包，这里提供了修改后的脚本包
    修改内容为: 把原来需要大量下载的代码去掉了,添加了一些编译模块，因为国内下载的速度能让你怀疑人生
    这里你可以直接使用修改的版本，也可以去主页下载原版！
    https://github.com/Kukkimonsuta/rpi-buildqt
4： 编辑环境
    在~/raspi/下解压 rpi-buildqt包
    编辑rpi-buildqt/scripts/env.sh文件
    修改 # device info 下边的设置
    export RPIDEV_DEVICE_VERSION=pi3            # pi1 pi2 pi3 (only tested pi3)  你的设备类型
    export RPIDEV_DEVICE_ADDRESS=192.168.5.109  # ip of device                   设备IP
    export RPIDEV_DEVICE_PORT=22                # ssh port                       ssh链接的端口
    export RPIDEV_DEVICE_USER=pi                # username                       用户名
    export RPIDEV_DEVICE_PW=123456              # password                       密码

    最基础的这几项是需要修改的，必须修改对，后边脚本会自动引用，同步和反同步都需要 
    其他的根据你自己的需要修改，一般是不需要更改的！
//(这步需要在你的树莓派设置)---------------------------------------------------------------------------
5:  
    sudo raspi-config              选择扩展项目，配置你的内存 给显卡 256M （具体操作可以百度，很简单）

    关于安装opengles2库的安装(如果你使用的是lite版的话)
    sudo apt-get install libgles2-mesa-dev

    //更新下 egl/gles 链接 这步支持复制粘帖就行！
    sudo rm /usr/lib/arm-linux-gnueabihf/libEGL.so* /usr/lib/arm-linux-gnueabihf/libGLESv2.so*
    
    sudo ln -s /opt/vc/lib/libEGL.so /usr/lib/arm-linux-gnueabihf/libEGL.so.1.0.0
    sudo ln -s /opt/vc/lib/libGLESv2.so /usr/lib/arm-linux-gnueabihf/libGLESv2.so.2.0.0
    
    sudo ln -s libEGL.so.1.0.0 /usr/lib/arm-linux-gnueabihf/libEGL.so.1
    sudo ln -s libGLESv2.so.2.0.0 /usr/lib/arm-linux-gnueabihf/libGLESv2.so.2
    
    sudo ln -s libEGL.so.1.0.0 /usr/lib/arm-linux-gnueabihf/libEGL.so
    sudo ln -s libGLESv2.so.2.0.0 /usr/lib/arm-linux-gnueabihf/libGLESv2.so
    
    //安装支持的软件和库文件
    sudo apt-get update
    sudo apt-get install rsync  
    //蓝牙音箱连接
    sudo apt-get install pulseaudio pulseaudio-module-bluetooth libbluetooth-dev alsa-tools
    pacmd set-default-sink bluez_sink.xx_xx_xx_xx_xx_xx   //输出音频到特定蓝牙地址中  	

    bluez libbluetooth-dev
    
    //qtbase         需要的库
    sudo apt-get install libboost1.58-all-dev libudev-dev libinput-dev libts-dev libmtdev-dev libjpeg-dev libfontconfig1-dev libssl-dev libdbus-1-dev libglib2.0-dev libxkbcommon-dev
    
    sudo apt-get install libgles2-mesa-dev libgbm-dev
    sudo apt-get install libpq-dev libmariadbclient-dev
    
    // qtmultimedia  需要的库
    sudo apt-get install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly gstreamer1.0-plugins-bad libgstreamer-plugins-bad1.0-dev gstreamer1.0-pulseaudio gstreamer1.0-tools gstreamer1.0-alsa libasound2-dev pulseaudio libpulse-dev


    //qtwebengine    需要的库
    sudo apt-get install libvpx-dev libsrtp0-dev libsnappy-dev libnss3-dev

    //piomxtextures  需要的库
    sudo apt-get install libssh-dev libsmbclient-dev libv4l-dev libbz2-dev
    
    //附加语音合成库
    sudo apt-get install  libspeechd-dev speech-dispatcher
    
    到这里树莓派的任务就结束了，但需要继续开机联网，后边需要同步和反同步

//这里的步骤是附加的，因为去掉了脚本下载部分所以需要我们自己去下载-------------------------------------------
1： 下载tools编译工具包 这个也可以去git下载，下载zip包还是很快的，就不提供了，大几百M
    https://github.com/raspberrypi/tools
    注意： 下载解压后记得修改文件夹为tools,因为一般下载zip包解压会带后缀 master,这里需要去掉
    结构就像这样    
    ~/raspi/tools

3： 下载qt的源代码 直接去官方下载
     download/qt.io  
     我下载的是最新版的 qt-everywhere-opensource-src-5.9.1 
     里边已经有各个模块的包了，不需要重新下载
     注意：
     解压后文件夹名称修改为src 并放在~/raspi/下 (解压时间比较长，等完全解压再改名)
     结构就像这样
     ~/raspi/src/qtbase
     ~/raspi/src/piomxtextures
     ~/raspi/src/....  其他qt模块

上边这些准备工作做好后就可以执行 脚本进行安装
//----------------------------------------------------------------------
cd  ~/raspi/rpi-buildqt/scripts
chmod +x 0_init.sh
    
./0_init.sh                           //设置权限的
./1_tools.sh                          //下载编译器和固定编译器的，我把下载代码注释了，前边已经自己下载配置了
./2_sysroot.sh                        //同步树莓派的库和头文件的，树莓派一定要联网可以ssh访问到才行
./3.0_download_qtbase.sh              //下在qtbase源文件的，下载被我注释了，前边已经自己下载配置了
./3.1_build_qtbase.sh                 //编译qtbase，这步也就10分钟就OK了，我的机器比较烂，inter g2020的办公cpu
./4.0_download_modules.sh             //下载其他qt模块,下载被我注释了，前边已经自己下载配置了
./4.1_build_modules.sh                //编译上一步下载的模块，这步其他模块都会很快，唯独qtwebengine,我编译了整整4个小时
./5.0_download_piomxtextures.sh       //下载第三方的模块，这步我没注释，主要是文件不大，很快
./5.1_build_piomxtextures.sh          //编译这块也需要下载一些文件，我没有注释，主要是下载FFmpeg-n3.2.4 LightLogger
                                        需要下载30M左右的包，速度大概是10K左右，痛苦的等待！大概半个小时就编译完成
./6_copy_to_device.sh                 //最后一步了，反同步到树莓派上，记得必须可以访问树莓派

最后可以测试下 ssh你的树莓派 然后执行 ： ~/piomxtextures_pocplayer /opt/vc/src/hello_pi/hello_video/test.h264
成功的话就可以看到 一段视频了！

