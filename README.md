# rpi-qt
关于树莓派和qt的所有！

qt在树莓派上的编译脚本，修改版
树莓派制作通用镜像的脚本，修改版
qt在树莓派上链接mssql数据库指南。

http://youness.net/raspberry-pi/bluetooth-headset-raspberry-pi

apt-get update
– apt-get upgrade
– wget https://freedesktop.org/software/pulseaudio/releases/pulseaudio-6.0.tar.xz
– tar xvf pulseaudio-6.0.tar.xz
– apt-get -y install intltool libtool libcap-dev libjson0-dev libsndfile1-dev libudev-dev libsbc-dev libbluetooth-dev libx11-xcb-dev libasound2-dev libsystemd-dev libsamplerate0-dev libspeexdsp-dev libdbus-1-dev
– cd pulseaudio-6.0 && ./configure –disable-x11 –disable-bluez4 && make -j 8 && make install && ldconfig
– apt-get install pulseaudio-module-bluetooth


    [root@Byrd byrd]# free -m  
                 total       used       free     shared    buffers     cached  
    Mem:           512        108        403          0          0         28  
    -/+ buffers/cache:         79        432  
    Swap:            0          0          0  
    [root@Byrd ~]# mkdir /opt/images/  
    [root@Byrd ~]# rm -rf /opt/images/swap  
    [root@Byrd ~]# dd if=/dev/zero of=/opt/images/swap bs=1024 count=2048000  
    2048000+0 records in  
    2048000+0 records out  
    2097152000 bytes (2.1 GB) copied, 82.7509 s, 25.3 MB/s  
    [root@Byrd ~]# mkswap /opt/images/swap  
    mkswap: /opt/images/swap: warning: don't erase bootbits sectors  
            on whole disk. Use -f to force.  
    Setting up swapspace version 1, size = 2047996 KiB  
    no label, UUID=59daeabb-d0c5-46b6-bf52-465e6b05eb0b  
    [root@hz mnt]# swapon /opt/images/swap  
    [root@hz mnt]# free -m  
                 total       used       free     shared    buffers     cached  
    Mem:           488        481          7          0          6        417  
    -/+ buffers/cache:         57        431  
    Swap:          999          0        999  
