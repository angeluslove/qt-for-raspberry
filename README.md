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
