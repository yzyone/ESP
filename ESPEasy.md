# ESP8266串口 NodeMCU Lua V3物联网开发板 CH340/CP2102 wifi模块



**原理图请复制图片点击放大：**  
![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN0145wERx1HeCNhWXQIa_!!2206710860782.png)

# ![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN01WR2q091HeCIlDV610_!!2206710860782.jpg)

#### 1.刷入固件

我这里使用的是ESPEasy固件  

官网地址：www.letscontrolit.com  

官网固件下载地址：https://www.letscontrolit.com/downloads/ESPEasy_R120.zip（这里是稳定版R120版本固件，想要测试版的自己去官网找）

#### 下载的时候先把模块链接电脑，驱动没问题的话会在设备管理器端口里有个设备，如下图所示：![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN01bFNrro1HeCInL8NHG_!!2206710860782.jpg)

#### 

记住后面的COM口号，我这里是4  

解压下载好的固件压缩包

![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN01MhTyDt1HeCIjEPRir_!!2206710860782.jpg)  
模块链接电脑的情况下双击“flash.cmd”  
![](https://img.alicdn.com/imgextra/i1/2206710860782/O1CN01i8J5u91HeCIdbWoIO_!!2206710860782.jpg)

**第一行是端口号，就是上面设备管理器的COM口号根据自己的来填写，写完按回车键**

**第二行是flash的大小，nodemcu模块就输入4096，然后回车**

**第三行是版本号，输入120 然后回车，按一下板子上的flash按键开始刷入，等一会，下图所示就是成功了![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN01weSYCy1HeCIjETPJN_!!2206710860782.jpg)**

**然后拔掉模块等下再用  
**

# 二、树莓派安装Domoticz

直接输入以下命令安装，简单粗暴

sudo curl -L install.domoticz.com | sudo bash

过程会很慢，请耐心等待，等待过后会弹出这个窗口，按回车键确定（由于我已经安装完了，所以下面这图是我从其它地方偷的）![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN01n6E4mx1HeCImYfu5f_!!2206710860782.jpg)

设置http访问和https访问端口（选一个http访问就可以）  
![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN012FtlXx1HeCIphJ406_!!2206710860782.jpg)![](https://img.alicdn.com/imgextra/i3/2206710860782/O1CN01PwONt41HeCIlPk6nk_!!2206710860782.jpg)

http端口（做过魔镜的小伙伴们这里用其它端口代替 如1234端口）  
![](https://img.alicdn.com/imgextra/i3/2206710860782/O1CN01deM4lh1HeCIjEglXo_!!2206710860782.jpg)

这一步默认就行  
![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN01cr1FR11HeCIphU4YM_!!2206710860782.jpg)

按确定就成功了  
![](https://img.alicdn.com/imgextra/i2/2206710860782/O1CN01P1Sd1n1HeCIpha33U_!!2206710860782.jpg)

记住上面的http那个ip和端口，在浏览器输入上面的ip和端口192.168.31.89:8080按回车访问

接下来就进到Domoticz里了

![](https://img.alicdn.com/imgextra/i1/2206710860782/O1CN01di83pC1HeCIphqDxn_!!2206710860782.jpg)

由于是英文，我们要改成中文，如下图所示：  
![](https://img.alicdn.com/imgextra/i2/2206710860782/O1CN01VSPYg41HeCIphryDX_!!2206710860782.jpg)

1.选择语言选项

2.Domoticz选择中文

3.填写当地的经纬度（上面的是经度，下面是维度）不知道的去这里查：www.gpsspg.com/maps.htm

4.“应用到设置”

然后界面会变成中文![](https://img.alicdn.com/imgextra/i2/2206710860782/O1CN015krU151HeCIjXrS0j_!!2206710860782.jpg)

点 “设置”—“硬件” 添加一个硬件  
![](https://img.alicdn.com/imgextra/i2/2206710860782/O1CN01E5KrYj1HeCIiGckpX_!!2206710860782.jpg)

名称随便填一个

类型我们选择“Dummy (Does nothing, use for virtual switches only)”

![](https://img.alicdn.com/imgextra/i3/2206710860782/O1CN01X1Ld6t1HeCInCiTDO_!!2206710860782.jpg)

然后按“增加”  
![](https://img.alicdn.com/imgextra/i2/2206710860782/O1CN015814v21HeCIdcTkxv_!!2206710860782.jpg)

我们看到增加了一个硬件，点 “创建虚拟传感器”  
![](https://img.alicdn.com/imgextra/i1/2206710860782/O1CN016AfU9F1HeCIdcWybT_!!2206710860782.jpg)

名称跟上面的填一样，传感器类型选择“开关”然后点“OK”

之后会看到一个提示创建成功

![](https://img.alicdn.com/imgextra/i3/2206710860782/O1CN013x6Q5X1HeCIlkC5WX_!!2206710860782.jpg)

OK，Domoticz平台的配置先到这里

# 三、ESP8266模块的配置

首先把写入固件的模块连接上电源，然后打开电脑的wifi（没电脑的用手机也可以），列表里会有个叫ESP_0的wifi，连上它，默认密码是configesp

连上去之后浏览器输入默认网关地址192.168.4.1

模块会自动搜索附近wifi，选择你家的wifi，然后把密码填进去，点“connect”连接，连上去之后会出现个倒计时20秒的页面，倒计时结束后会显示一个局域网ip，然后电脑连上你的wifi之后打开这个显示的ip（这个ip就是模块在你的局域网里的ip）![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN014ES4cf1HeCIlEttxj_!!2206710860782.jpg)

打开后来到config这一栏，这里主要改两个地方“Controller IP”和“Controller Port”

Controller IP填写Domoticz管理页面的ip地址

Controller Port填写Domoticz管理页面的ip的端口

![](https://img.alicdn.com/imgextra/i3/2206710860782/O1CN01YjZ19O1HeCIjY6ckE_!!2206710860782.jpg)

下面的选项默认就行，然后点“Submit”保存  
![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN01VjiXh81HeCInD0vfy_!!2206710860782.jpg)

# 四、NodeMcu模块与Domoticz平台联动

来到“Devices”这一栏，选择“Edit”添加

![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN018Ri9Rt1HeCIkxFflC_!!2206710860782.jpg)

Device选择“Switch input”  
![](https://img.alicdn.com/imgextra/i2/2206710860782/O1CN0190ABSz1HeCIjYAikI_!!2206710860782.jpg)![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN01BCw1yY1HeCIlQh3Se_!!2206710860782.jpg)![](https://img.alicdn.com/imgextra/i1/2206710860782/O1CN01QY4b8N1HeCIlF2xx3_!!2206710860782.jpg)

我们回到Domoticz页面，点“设置”—“设备”，看一下我们添加的那个开关的“IDX”的值，记住这个值  
![](https://img.alicdn.com/imgextra/i2/2206710860782/O1CN01fFlYpT1HeCInD9Jzu_!!2206710860782.jpg)

把我们刚才看到的IDX值填到下面的的“IDX/Var”里，GPIO选择GPIO-0，然后选择“Submit”保存  
![](https://img.alicdn.com/imgextra/i2/2206710860782/O1CN01l8sIGN1HeCIpIbzXv_!!2206710860782.jpg)

然后回到Domoticz页面的“开关”这一栏，找到我们添加的开关设备，然后点击“编辑”  
![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN01vLdDtr1HeCIlkUYE9_!!2206710860782.jpg)

开 触发这一栏填写：（记得把中间的ip地址改成自己的NodeMcu的局域网ip）

http://192.168.2.196/control?cmd=GPIO,0,1

意思就是GPIO0的值为1

关 触发这一栏填写：（记得把中间的ip地址改成自己的NodeMcu的局域网ip）

http://192.168.2.196/control?cmd=GPIO,0,0  

同上，意思就是GPIO0的值为0

然后点击“保存”![](https://img.alicdn.com/imgextra/i4/2206710860782/O1CN01zFeni41HeCIiH8NHV_!!2206710860782.jpg)

找个3V的LED灯，负极接在GND针脚，正极接在GPIO-0针脚上，然后点击开关面板的灯泡图标试试能否点亮，不出意外的话是会亮的，成功后把LED灯换成3V的继电器，继电器再并入电器线路中就能用它控制一些小功率的电器了（最大功率电器根据继电器允许的功率计算）

开灯关灯这些都可以进入到Domoticz平台的管理界面进行管理，如果家里有不用的手机或者平板都可以作为控制设备

看官方文档时发现有Domoticz的Android客户端

https://www.jianshu.com/go-wild?ac=2&url=http%3A%2F%2Fpan.baidu.com%2Fshare%2Flink%3Fshareid%3D4011804182

![](https://img.alicdn.com/imgextra/i1/2206710860782/O1CN01HrMHY91HeCIllCjQA_!!2206710860782.jpg)

局域网任意一台联网设备的浏览器输入Domoticz的管理界面也可以控制  
![](https://img.alicdn.com/imgextra/i3/2206710860782/O1CN01BZ00AU1HeCIo9hHlS_!!2206710860782.jpg)

Domoticz平台也可以设置条件，比如当温度传感器温度达到30℃时自动触发风扇开关，门后的红外传感器检测到有人打开门时自动触发灯的开关这些

参考链接：https://www.jianshu.com/p/4828e78cc7b5
