#install ROS#
2016/10/24 23:59:07 
##install ROS kinetic on ubuntu 16.04##
 - Configure your Ubuntu repositories

Modify the software repositories configuration file /etc/apt/sources.list.

Allow "restricted" "universe" and "multiverse".

![figure1](http://i.imgur.com/EWqR6ih.png)

- Setup your sources.list

Setup your computer to accept software from packages.ros.org. ROS Kinetic ONLY supports Wily (Ubuntu 15.10), Xenial (Ubuntu 16.04) and Jessie (Debian 8) for debian packages.

`sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`

 - Set up your keys

`sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116`

 - Installation

Make sure your Debian package index is up-to-date.

`sudo apt-get update`

Desktop-Full Install: (Recommended) : ROS, rqt, rviz, robot-generic libraries, 2D/3D simulators, navigation and 2D/3D perception.

`sudo apt-get install ros-kinetic-desktop-full`

 - Initialize rosdep

Before you can use ROS, you will need to initialize rosdep. rosdep enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS.
    
    sudo rosdep init
    rosdep update

 - Environment setup

It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell is launched:

	echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
	source ~/.bashrc

 - Getting rosinstall

[Rosinstall](http://wiki.ros.org/rosinstall) is a frequently used command-line tool in ROS that is distributed separately. It enables you to easily download many source trees for ROS packages with one command.

To install this tool on Ubuntu, run:

`sudo apt-get install python-rosinstall`

 - Build farm status

The packages that you installed were built by the [ROS build farm](http://build.ros.org/). You can check the status of individual packages [here](http://repositories.ros.org/status_page/ros_kinetic_default.html).

#Tutorials #

##Installing and Configuring Your ROS Environment##
####1.Managing Your Environment####

If you are ever having problems finding or using your ROS packages make sure that you have your environment properly setup. A good way to check is to ensure that environment variables like ROS_ROOT and ROS_PACKAGE_PATH are set:

`printenv | grep ROS`

![figure3](http://i.imgur.com/VvixaNk.png)

If you just installed ROS from apt on Ubuntu then you will have setup.*sh files in '/opt/ros/Kinetic/', and you could source them like so:

`source /opt/ros/kinetic/setup.bash`

####2.Create a ROS Workspace####

Let's create a catkin workspace:

	mkdir -p ~/catkin_ws/src
	cd ~/catkin_ws/src
	catkin_init_workspace
Even though the workspace is empty (there are no packages in the 'src' folder, just a single CMakeLists.txt link) you can still "build" the workspace.

	cd ~/catkin_ws/
	catkin_make
![figure2](http://i.imgur.com/J20ZNwR.png)
The catkin_make command is a convenience tool for working with catkin workspaces. If you look in your current directory you should now have a 'build' and 'devel' folder. Inside the 'devel' folder you can see that there are now several setup.*sh files. Sourcing any of these files will overlay this workspace on top of your environment. To understand more about this see the general catkin documentation: catkin. Before continuing source your new setup.*sh file:

`source devel/setup.bash`

To make sure your workspace is properly overlayed by the setup script, make sure ROS_PACKAGE_PATH environment variable includes the directory you're in.

`echo $ROS_PACKAGE_PATH`

![figure3](http://i.imgur.com/VvixaNk.png)

Now that your environment is setup.
