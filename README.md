# README #
## Description ##
The **Distributed operation layer(DOL)** is a software development framework to program parallel applications. The DOL allows to specify applications based on the Kahn process network model of computation and features a simulation engine based on SystemC. Moreover, the DOL provides an XML-based specification format to describe the implementation of a parallel application on a multi-processor systems, including binding and mapping.
## How to install ##
Ctrl + Alt + T to open terminal of the ubuntu:

#####1. 安装必要环境#####

	$	sudo apt-get update  
	$	sudo apt-get install ant  
	$	sudo apt-get install openjdk-7-jdk  
	$	sudo apt-get install unzip  

#####2. 下载文件#####

	$	sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz
	$	sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip

#####3. 解压文件#####
1. 新建dol的文件夹

    `$	mkdir dol`

2. 将dolethz.zip解压到 dol文件夹中

	`$	unzip dol_ethz.zip -d dol`

3. 解压systemc

	`$	tar -zxvf systemc-2.3.1.tgz`

#####4. 编译systemc#####
1. 解压后进入systemc-2.3.1的目录下

	`$	cd systemc-2.3.1	`

2. 新建一个临时文件夹objdir

	`$	mkdir objdir`

3. 进入该文件夹objdir

	`$	cd objdir`

4. 运行configure(能根据系统的环境设置一下参数，用于编译)

	`$	../configure CXX=g++ --disable-async-updates`

	下图为运行configure之后的截图
	![figure1](http://i.imgur.com/uKhXbwk.png)

5. 编译

	`$	sudo make install`

	编译完后文件目录如下图(`$ cd ..        $ ls`)，能看到include, lib-linux64(对于32位系统，这里是lib-linux)
	![figure2](http://i.imgur.com/xz1vZAX.png)

6. 记录当前的工作路径(会输出当前所在路径，**记下来，待会有用**)

	`$	 pwd`

	这里我当前的工作路径为 /home/zhongshipeng/systemc-2.3.1

#####5. 编译dol#####
1. 进入刚刚dol的文件夹

	`$	cd ../dol`
2. 修改build_zip.xml文件

	找到下面这段话，就是说上面编译的systemc位置在哪里，

		<property name="systemc.inc" value="YYY/include"/>
		<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>

	把YYY改成上页pwd的结果（注意，**对于  64位 系统的机器，lib-linux要改成lib-linux64**）
	![figure3](http://i.imgur.com/KBoZq5f.png)

3. 然后是编译

	`$	ant -f build_zip.xml all`

	若成功会显示build successful
	![figure4](http://i.imgur.com/ijePyfv.png)

5. 接着可以试试运行第一个例子，首先进入build/bin/mian路径下

	`$	cd build/bin/main`

6. 然后运行第一个例子

	`$	ant -f runexample.xml -Dnumber=1`

	成功结果如下图
	![figure5](http://i.imgur.com/wvQoXLh.png)
## Experimental experience ##
1. 问题与解决

	刚开始配置时就遇到了问题：执行

	`$ 	sudo apt-get install openjdk-7-jdk`

	时在库缺失软件包，再次更新库

	`sudo apt-get update`

	也不行。

	于是决定自行下载安装jdk7：

		sudo add-apt-repository ppa:webupd8team/java
		sudo apt-get update
		sudo apt-get install oracle-java7-installer

	接下来的安装过程也是曲折前进，但是基本没有出问题。

	到最后一步运行第一个例子时fail了。通过Q/A文件了解到是设置了中文系统的锅，在dol/build/bin/main 下 的 runexample.xml 215-217行需要注释掉。问题就解决了。最后成功配置~

2. 感想与体会

	- 按照给出的步骤一步步来做，安装过程还是比较顺畅的。有时候会懵圈，看漏了空格，**所以需要注意代码的空格！** 
	- 遇到问题可以自己寻找解决方案，无论是通过课堂资源还是网上资源。
	- 了解并认真学习了markdown语法，但是对于代码框（多行代码）还是很迷惑，暂时是用了用tab键的方式。以后对markdown的了解需要更深入。


