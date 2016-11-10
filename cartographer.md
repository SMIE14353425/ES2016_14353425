#Install Cartographer#

####1.安装所有依赖项####
由于我们先前安装的是kinetic，所以下述是kinetic，不同安装对应不同

`sudo apt-get install -y google-mock libboost-all-dev  libeigen3-dev libgflags-dev libgoogle-glog-dev liblua5.2-dev libprotobuf-dev  libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler python-sphinx  ros-kinetic-tf2-eigen libatlas-base-dev libsuitesparse-dev liblapack-dev`

####2.首先安装ceres solver，选择的版本是1.11,路径随意####
执行下列代码即可安装完成

	git clone https://github.com/hitcm/ceres-solver-1.11.0.git
	cd ceres-solver-1.11.0
	mkdir build
	cd build
	cmake ..
	make -j
	sudo make install

####3.然后安装 cartographer,路径随意####
执行下列代码即可安装完成，若执行ninja时卡顿，尝试增大虚拟机内存

	git clone https://github.com/hitcm/cartographer.git
	cd cartographer
	mkdir build
	cd build
	cmake .. -G Ninja
	ninja
	ninja test
	sudo ninja install

####4.安装cartographer_ros####
在此之前我们需要建立我们自己工作区catkin_ws，上述已有部分叙述。ROC工作目录详细操作如下：
	
	mkdir ~/catkin_ws
	cd ~/catkin_ws
	mkdir src
	cd src
	git clone https://github.com/hitcm/cartographer_ros.git
	cd ..
	catkin_make
	source devel/setup.sh
	roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag
	roslaunch cartographer_ros demo_backpack_3d.launch bag_filename:=${HOME}/Downloads/cartographer_3d_deutsches_museum.bag
我们需要提前下载数据到Downloads文件夹下

2d数据，大概500M，[下载](https://storage.googleapis.com/cartographer-public-data/bags/backpack_2d/cartographer_paper_deutsches_museum.bag)

3d数据，8G左右，[下载](https://storage.googleapis.com/cartographer-public-data/bags/backpack_3d/cartographer_3d_deutsches_museum.bag)

最后跑的程序截图如下：
![figure1](http://i.imgur.com/p4Ze95o.png)
![figure2](http://i.imgur.com/DzfJJQs.png)
![figure3](http://i.imgur.com/KwOZ8uR.png)

#实验感想#
安装ros时是简单的，但是到安装cartographer时就比较复杂了。而且也比较容易崩，所以先把虚拟机备份了，并且调大了虚拟机的内存，调到了2G（推荐最大4G）。一开始，由于没翻墙，只能参考[参考博客](http://www.cnblogs.com/hitcm/p/5939507.html#commentform)，一开始被自己坑了一下，代码都copy错，“-”“—”不一样。然后是ROS工作目录，本来按照教程自己弄了一次，再弄这个就有问题了，弄了很久，都有问题——同博主提出的问题，但是用尽了解决办法都没有用，包括博客上的办法，后来遇到另一个问题，猜测是中文系统的锅，改回了英文也没用。后来，删了工作目录重新来一遍就OK了。（感觉太懒）由于3D数据包太大就没下来跑了。