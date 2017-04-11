# opencv
## opencv的各版本搭建工作
## 一、在centos6.5上离线搭建opencv2.4.13

* 1.搭建opencv2.4.13所需环境总结：
  *官网给出的环境是：

	* GCC 4.4.x or later
	
	* CMake 2.6 or higher
	
	* Git
	
	* GTK+2.x or higher, including headers (libgtk2.0-dev)
	
	* pkg-config
	
	* Python 2.6 or later and Numpy 1.5 or later with developer packages  (python-dev, python-numpy)
	
	* ffmpeg or libav development packages: libavcodec-dev, libavformat-dev,libswscale-dev
	
	* [optional] libtbb2 libtbb-dev
	
	* [optional] libdc1394 2.x
	
	* [optional] libjpeg-dev, libpng-dev, libtiff-dev, libjasper-dev,libdc1394-22-dev
	

* 简单点讲就是：
0.pkg-config 1.gcc  2.gcc-c++ 3.cmake 4.gtk+* 5.ffmpeg  6.python-dev 7.numpy 

 其中pkg-config一般系统都已经安装好

* （1）：安装gcc gcc-c++

可联网情况下在ubantu上安装编译器gcc gcc-c++只需一条命令：sudo apt-get install build-essential

在redhat上只能用源码进行编译，详情见 《gcc与g++编译安装文档》 若想联网安装需要注册红帽网络或者换用其他发行版的源

centos联网可用yum install gcc gcc-c++安装，离线情况使用源码进行编译 （网上说需要3小时，亲测30-45分钟）

* （2）：安装cmake

联网：ubantu：sudo apt-get install cmake

　　 centos：yum install cmake
      
不联网：使用源码进行编译安装，详情见《cmake编译安装文档》

另一种：在redhat上离线安装软件可以通过在其他发行版（比如centos）安装 yumdownloader（yum只下载不安装工具）

将使用 yum install * 的过程中所依赖的rpm包下载下来，再对照yum的安装过程一次进行rpm包的安装与更新
	
* （3）：安装gtk+*

由于使用源码编译比较烦琐，编译其依赖库的过程中所遇到的问题甚多，解决相对麻烦耗时，最后甚至做无用功，

所以我才用的是yumdownloader 将gtk安装所需的依赖包下载下来依次安装即可解决问题，查看gtk相关软件的命令：

pkg-config --list-all gtk


* （4）：安装ffmpeg

安装ffmpeg的原因是其中包含libavcodec-dev, libavformat-dev,libswscale-dev三个库文件

安装方法在centos和redhat上只能通过源码进行编译，详情见《ffmpeg编译安装文档》



* （5）：安装python-dev

由于linux自带python，当时我以为python-dev中包含其他工具，所以就通过yumdownloader方法重新更新了一下，现在看来实在没有必要。

但是在redhat上python版本是4.1.2，需要进行版本更新，使用源码进行编译，详情见《python源码编译安装更新》


* （6）：安装numpy

官方网站给出的numpy安装时可选项 optional，然而不安装numpy进行编译opencv后python调用cv2这个模块问题甚多

所以若使用python则必须安装

ubantu：sudo apt-get install numpy或源码编译

centos：yum install numpy或源码编译

redhat：使用源码进行编译


源码编译见《numpy编译安装文档》


* （7）：安装编译opencv

安装上面所需软件后，就可以动手编译安装opencv了

源码编译及脚本详见《opencv编译安装文档》



##二、在centos6.5上离线搭建opencv3.2.0


##1.官网给出的编译环境基本上与编译opencv2.4.13相同


Required Packages

    * GCC 4.4.x or later
    
    * CMake 2.8.7 or higher
    
    * Git
    
    * GTK+2.x or higher, including headers (libgtk2.0-dev)
    
    * pkg-config
    
    * Python 2.6 or later and Numpy 1.5 or later with developer packages (python-dev, python-numpy)
    
    * ffmpeg or libav development packages: libavcodec-dev, libavformat-dev, libswscale-dev
    
    * [optional] libtbb2 libtbb-dev
    
    * [optional] libdc1394 2.x
    
    * [optional] libjpeg-dev, libpng-dev, libtiff-dev, libjasper-dev, libdc1394-22-dev
    
    * [optional] CUDA Toolkit 6.5 or higher
    
    * [compiler] sudo apt-get install build-essential

    * [required] sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

    * [optional] sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev  	   		    libdc1394-22-dev


## 2.步骤：

* （1）安装gcc g++

* （2）安装cmake

* （3）安装gtk+2.x

* （4）安装ffmpeg

* （5）安装python

* （6）安装python-numpy

* （7）安装可选的依赖项

* （8）编译安装opencv3.2.0


## 注：

* 第（5）(6)步注意事项

因为在centos6.5上系统默认装的python版本为2.6.6，使用此版本编译opencv3.2.0会出现一个问题，即在 “make”到百分之九十几的时候会出现语法错误之类：

[ 96%] Generating pyopencv_generated_include.h, pyopencv_generated_funcs.h, pyopencv_generated_types.h, pyopencv_generated_type_reg.h, 


pyopencv_generated_ns_reg.h

Traceback (most recent call last):

File "/app/opencv-3.2.0/modules/python/python3/..//src2/gen2.py", line 4, in

import hdr_parser, sys, re, os

File "/app/opencv-3.2.0/modules/python/src2/hdr_parser.py", line 855

has_mat = len(list(filter(lambda x: x[0] in {"Mat", "vector_Mat"}, args))) > 0

^
SyntaxError: invalid syntax

make[2]: *** [modules/python3/pyopencv_generated_include.h] Error 1

make[1]: *** [modules/python3/CMakeFiles/opencv_python3.dir/all] Error 2

make: *** [all] Error 2

这可能是因为python2.6某些语法不支持最新版本opencv不兼容导致的，（另外听说python2.6可以进行编译，但至少我暂时不会）

解决办法：升级python至2.7，然后进行编译安装。但是在linux升级python会导致很多问题，像yum、pip、easy_install等不能用，需要重新进行配置

另外升级完python到2.7后，安装numpy时一定要使用源码编译，并且调用最新的python2.7解释器执行：

/(python2.7安装路径下)/bin/python2.7 setup.py install


*第（8）步注意事项：先按照官网步骤执行：

cd ~/opencv

mkdir build

cd build

官网上的下一步命令是这个：cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..  

但是你编译完后发现并没有产生cv2.so这个模块，原因就是因为升级python导致的，使用yum 安装到系统的软件在系统中的路径设置，

环境变量设置的相对全面，而采用源码编译安装可能就略显.....

源码编译安装python2.7后，cmake 并不能很好的识别到python相关库的位置，所以需要就行手动添加：

cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_GPHOTO2=OFF -D PYTHON2_INCLUDE_DIR=/usr/local/python27/include/python2.7-D
PYTHON2_LIBRARY=/usr/local/python27/lib/libpython2.7.so -D PYTHON2_NUMPY_INCLUDE_DIRS=/usr/local/python27/lib/python2.7/site-packages
/numpy/core/include -D PYTHON2_PACKAGES_PATH=lib/python2.7/site-packages -D OPENCV_EXTRA_MODULES_PATH=/usr/local/src/opencv_contrib-
3.2.0/modules -D CMAKE_INSTALL_PREFIX=/usr/local .. 

其中-D 的意思就是说后面要定义一些参数

-D WITH_GPHOTO2=OFF  可能编译过程会发生一些关于gphoto2的报错，使用 WITH_GPHOTO2=OFF 放弃该模块的编译

-D PYTHON2_INCLUDE_DIR=/usr/local/python27/include/python2.7  手动定义PYTHON2_INCLUDE_DIR的位置

-D PYTHON2_LIBRARY=/usr/local/python27/lib/libpython2.7.so    手动定义python2.7共享链接库的位置

-D PYTHON2_NUMPY_INCLUDE_DIRS=/usr/local/python27/lib/python2.7/site-packages/numpy/core/include  手动定义numpy中include的位置

-D PYTHON2_PACKAGES_PATH=lib/python2.7/site-packages    手动定义python包路径的位置

-D OPENCV_EXTRA_MODULES_PATH=/usr/local/src/opencv_contrib-3.2.0/modules  定义opencv额外模块的位置 （OpenCV从2.x到3.x是一个很大的转变，

对于很多功能不完善、性能不稳定的模块，都被添加到了extra_modules（扩展模块）里面了）


cmake成功后即可进行make，make install

安装完成后在进行配置：
```shell
echo "/usr/local/lib">> /etc/ld.so.conf

sudo /sbin/ldconfig

echo "PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig

export PKG_CONFIG_PATH" >> /etc/bashrc

source /etc/bashrc

echo "updating databases........"

sudo updatedb

echo "PYTHONPATH=/usr/local/lib/python2.7/site-packages

export PYTHONPATH">> ~/.bashrc

source ~/.bashrc  #大功告成
```



此时可以打开python:
```shell
[root@localhost Desktop]# python

Python 2.7.13 (default, Mar 31 2017, 19:34:43) 

[GCC 4.8.2] on linux2

Type "help", "copyright", "credits" or "license" for more information.

>>> import cv2

>>> cv2.__version__

'3.2.0'

>>> 
```


问：1.找不到cv2？
	Traceback (most recent call last):
	
 	 File "<stdin>", line 1, in <module>
	 
	ImportError: No module named cv2.cv

答：

前提：安装了python-numpy并且编译完成后产生了cv2.so这个库

解决办法：sudo vi  ~/.bashrc

在末尾加上：

export PYTHONPATH=/usr/local/lib/python2.6/site-packages:$PYTHONPATH

然后加入缓存或者关机重启

echo "export PYTHONPATH=/usr/local/lib/python2.6/site-packages:$PYTHONPATH" >> ~/.bashrc
