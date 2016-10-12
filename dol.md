#DOL 实例分析&编程#
##*.dot截图##
修改后example1.dot截图如下：

![figure1](http://i.imgur.com/b7lYoNI.png)

修改后example2.dot截图如下：

![figure2](http://i.imgur.com/o7LjBch.png)

##具体如何修改的解释##
1. 修改example1，使其输出3次方数

	修改./dol/build/bin/main/example1/src/square.c文件，找到`i=i*i`
	
	改为`i=i*i*i`即可。其运行结果如下：
	![figure3](http://i.imgur.com/joigdsr.png)

	原因：参数i是由generator写到PORT_OUT端口上，由square读得。要使得这个参数变成三次方，并且输出到sqare的PORT_OUT端口上，只需要等式i = i * i * i;即可完成任务。

2. 修改example2，让3个square模块变成2个

	修改./examples/example1/example2.xml文件，找到`<variable value="3" name="N"/>`

	改为`<variable value="2" name="N"/>`即可。其运行结果如下：
	![figure4](http://i.imgur.com/aRQEAhE.png)

	原因：这个xml文件中使用了iterator，也就是相当于C/C++中的for循环那样，而这里是从1到N，共N次循环，建立了N个连接组（因为一个square需要两个连接），原代码N=3，共有3个square模块。因此把N改成2就可以了。

##实验感想##

1. dol的实例1，2原理看起来比较简单易懂。generator、consumer和square都被统一视为一个模块，由于一条条线——channel来连接，线的两头都需要绑好——connection。实际上是把模块的地址（名字）和端口写好，再进行端口对接。而square更多是像一个数据中转站。
2. 一开始直接进入指定文件夹运行`ant –f runexample.xml –Dnumber=XXX`发现BUILD FAIL，权限不足，加入sodu就好了。
	