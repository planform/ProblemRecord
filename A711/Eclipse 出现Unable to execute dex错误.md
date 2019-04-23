### Eclipse编译Android App时出现Unable to execute dex错误

#### 一、问题描述

* 使用Eclipse + ADT编译安卓应用程序时出现两个错误；
* Unable to execute dex: method ID not in [0, 0xffff]: 65536；
* Conversion to Dalvik format failed；

#### 二、问题分析

1. 在编译代码时没有出现错误，在编译成Android APP并准备调试时问题出现，所以问题肯定不在代码上；
2. 安卓的方法数不能超过65K；

#### 三、解决方案

1. 需要在Project.proterty中配置：`dex.force.jumbo=true`;
2. 右键项目，点击Properties，选中左边Java Build Path项，并在该选项中打开Libraries选项卡，将Android Dependencies项和Android Private Libraries这两项去掉。
3. Clean Up，重新编译运行。