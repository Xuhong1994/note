搭建环境
=
==Bazel、CUDA、CuDNN使用与Tensorflow对应的版本==
– Tensorflow1.2 -> bazel0.5.2/CUDA8.0.61.2/CuDNN6.0.20
==1.安装ubuntu14.04==
Tips:
a.用U盘安装时，要断网，否则会更新东西
b.安装完成后，会出现restart now，点击restart now后，记得拔掉U盘，否则会再次进入到安装界面
c.重启完毕，插上网线，同时将源选择成Ｍain Server

==2.安装显卡驱动==
a.Ctrl+alt+F1 进入命令行
b.``sudo service lightdm stop`` 关掉图形界面
c.进入安装包目录下，执行如下命令：
``sudo chmod u+x *.sh *.run``
``sudo ./NVIDIA-Linux-x86_64-384.59.run``

==3.安装cuda==
``sudo ./cuda_8.0.61.2_linux.run``
Tips:
不要选择安装显卡驱动
选项如图：
![](/home/xh/图片/选区_013.png) 

==4.安装cudnn==
a.解压
b.将对应的.h和.cpp文件分别拷贝到系统目录下
``sudo cp *.h /usr/local/cuda/include/``
``sudo cp  libcudnn* /usr/local/cuda/lib64/``
c.``sudo chmod  a＋r  /usr/local/cuda/include/*.h     `` ``/usr/local/cuda/lib64/libcudnn*``
d.添加cuda的库目录
``sudo vi /etc/profile ``
``export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH``
``source /etc/profile``


==5.安装Bazel==
a.``sudo apt-get install pkg-config zip g++ zlib1g-dev unzip``
b.``./bazel-0.5.2-installer-linux-x86_64.sh --user``
c.配置Bazel环境，见图
``sudo vi /etc/profile``
在尾部加入
``
export  PATH=/home/dlserver/bin:$PATH
``
退出，再``source /etc/profile``
![](/home/xh/图片/选区_006.png) 

==6.安装python2.7依赖==
![](/home/xh/图片/选区_007.png) 

==7.安装tensorflow依赖==
![](/home/xh/图片/选区_008.png) 

==8.安装git==
``
sudo apt get install git
``

==9.安装tensorflow==
a.``git clone https://github.com/tensorflow/tensorflow ``
b.``cd tensorflow``
 ``git checkout r1.２``checkout到固定版本号
 c.``cd tensorflow``
 `` ./configure``
 选项如图：
 ![](/home/xh/图片/选区_009.png) 
 d.``bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package ``
 e.``bazel-bin/tensorflow/tools/pip_package/build_pip_package　～/ttt``
 f.``sudo pip install ～/ttt/tensorflow-1.2.1-py2-none-any.whl``
 

==10.测试==
Tips:
安装完成后记得重启再进行测试
a.``vim a.py``
b.
![](/home/xh/图片/选区_010.png) 
c.python a.py
此时会出现以下问题：
![](/home/xh/图片/选区_011.png) 
检查发现是环境变量的问题　
``sudo vi /etc/profile ``
``export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH``
``source /etc/profile``

