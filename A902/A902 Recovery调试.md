### A902 Recovery调试 ###

#### 一、问题描述 ####

1. A902双屏在Recovery中无法使用触屏；
2. 单屏A902在Recovery中可以使用触屏；
3. A901无此问题；

#### 二、问题分析 ####

​	将机器刷完A902的固件后，进入Rec模式，触屏不能使用，将A902的副屏那掉，Rec模式可以正常触屏，所以问题应该出在A902的双屏上。

#### 三、解决问题的思路 ####

##### 1.将Rec的调试信息输出到串口 #####

​	Rec模式中Log的默认输出方式是/tmp/recovery.log文件，每次在Rec中操作后，如果想查看Log信息，必须重启机器将Log文件导出才能查看，非常不利于调试，所以首先要将调试信息输出到串口。Rec采用的方式是直接重定向标准输出（stdout），将其重定向到` /tmp/recovery.log `文件，所以直接找到串口的设备节点，将stdout重定向到串口的设备文件即可：

` echo "1111111" > /dev/console `

发现1111111正常在串口输出，所以` /dev/console `就是串口设备文件，重定向是在recovery.cpp文件的main函数中进行的，我们改写这个函数便可以将调试信息输出到串口中。

##### 2.关闭Selinux #####

​	将标准输出重定向到` /dev/console `文件后，进入Rec模式发现调试信息并不能从串口输出，根据Log信息发现是Selinux在作怪，所以需要在调试时将Selinux暂时关闭，常用的关闭Selinux的方法有三种：

 	1. 在ADB命令行中使用命令` setenforce 0 `将Selinux改变为Premissive模式，这种方法是暂时生效的，重启后就会复原；
 	2. 在内核编译选项中将Selinux移除编译项；
 	3. 在system.prop中定义` ro.boot.selinux=disable `；

