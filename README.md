
关于qt 5.12.6 在树莓派pi4 上的编译！

环境： win10     :  子系统 ubuntu18.4  
           pi4 2G    :  2019-09-26-raspbian-buster-lite.img （命令行，无图形界面）
           Qt          :  qt-everywhere-src-5.12.6.tar.xz


注意： 尽量使用较新的qt版本，同样的配置qt5.12.2在第一步就无法通过eglfs测试，所以请尽量使用qt5.12.4以后的版本。

全部文件说明：
pi3_mssql      ： 文件是关于 如何在 树莓派上 用 qt 程序连接mssql数据库的。
rpi_back_sh   ： 文件是关于如何备份一个树莓派做成镜像的脚本
qt_pi4_make ： 文件是交叉编译qt5.12.6在树莓派4上，并开启硬件加速的教程



