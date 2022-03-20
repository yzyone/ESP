# 简介

　　该文档简要讲解了DoitCar的开发流程，主要针对高级用户使用。

# DoitCar开发流程

　　如果刚刚拿到DoitCar，在Windows系统下的开发流程如下。

　　步骤1：硬件准备：开发PC机一台。MicroUSB线一根（非MiniUSB）

　　步骤2：软件准备：

　　（a）DoitRobo的核心是基于NodeMCU的， 最新版本的NodeMCU固件下载地址： [https://github.com/nodemcu/nodemcu-firmware/releases。](https://github.com/nodemcu/nodemcu-firmware/releases%E3%80%82) Tips:固件分为Float版本和Integer版本。为了最大限度降低运行内存消耗，通常情况下选用Interger版本。

　　（b）固件下载工具 nodemcu_flasher32bit.exe/nodemcu_flasher64bit.exe，请根据电脑系统选择Win32或Win64版本。最新版下载地址：[https://github.com/nodemcu/nodemcu-flasher。](https://github.com/nodemcu/nodemcu-flasher%E3%80%82)

　　（c）程序调试工具 程序调试使用LuaLoader。最新版下载地址： [https://github.com/GeoNomad/LuaLoader。](https://github.com/GeoNomad/LuaLoader%E3%80%82) 或者： [http://benlo.com/esp8266/index.html#LuaLoader。](http://benlo.com/esp8266/index.html#LuaLoader%E3%80%82)

　　（d）其他辅助工具：网络调试工具、串口工具等

　　步骤3：按照本章第二节内容烧写固件。

　　步骤4：按照本章第三节内容直接进行调试。

　　步骤5：按照本章第四节内容编写Lua文件，下载调试。



# 烧写固件

　　烧写固件通过ESP8266厂家提供的FLASH_DOWNLOAD_TOOLS工具下载。

　　步骤一：使用MICRO USB连接，USB线不仅实现通信，还对NodeMCU供电。如果第一次连接，需要安装USB驱动。DoitRobo的NodeMCU使用ttl转USB芯片为CP2102。

　　其驱动下载地址：[http://www.ddooo.com/softdown/56219.htm。](http://www.ddooo.com/softdown/56219.htm%E3%80%82) 　　 　　 ![](http://nodemcu-dev.doit.am/1.png)

　　步骤二：连接USB后，在Windows的设备管理器中可以查看串口号。Win7下，“计算机”右键->“管理”->“设备管理器”->“端口(COM和LPT)”。

![](http://nodemcu-dev.doit.am/2.png)

　　步骤三：打开FLASH_DOWNLOAD_TOOLS文件夹中的frame_test.exe。按照下图的1~5步依次选择。其中在第一步选择NodeMCU的固件，通常使用Integer版本。

![](http://nodemcu-dev.doit.am/3.png)

　　步骤四：点击烧写工具上的点START按钮，进入等待上电同步。

　　　　　![](http://nodemcu-dev.doit.am/4.png)

　　步骤五：按住模块的的FLASH键不放，然后再按一下RST键，进入烧写状态后，松开后即开始下载 　　

　　　　　![](http://nodemcu-dev.doit.am/5.png)

　　**Tips: 下载进度完成后会显示error错误提示，可忽略该错误，这是官方烧写工具的一个BUG。**

　　下载完成后，关闭程序。即可开始NodeMCU开发之旅。
