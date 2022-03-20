# 程序调试

　　程序调试工具提供基于串口的交互界面，包含基本的运行命令，应用程序编辑下载功能等。针对NodeMCU有多款开源工具可以使用，比如LuaLoader、NodeMCUStudioIDE、Decoda等。本教程以LuaLoader为例说明程序调试步骤。

　　Tips：NodeMCUStudioIDE下载地址：

　　https://github.com/nodemcu/nodemcu-studio-csharp

　　Decoda下载地址：

　　[Releases · unknownworlds/decoda · GitHub](https://github.com/unknownworlds/decoda/releases)



# “Hello World”

　　断开NodeMCU的USB连接线。运行Lualoader，运行画面如下。 　　 　　

![](http://nodemcu-dev.doit.am/6.png)

　　在“Setting”菜单中，选择“Comm Port Settings”，弹出“Serial Advanced Setting”。在“Port”中设置串行端口号，其他为默认即可。 　　 　　　　　　　　

![](http://nodemcu-dev.doit.am/7.png)

　　插入NodeMCU的USB连接线。在主界面菜单栏选择“Connect”，完成连接。 　　 　　

![](http://nodemcu-dev.doit.am/8.png)

　　在Lualoader下方有命令输入编辑框，可以通过发送命令的方式与NodeMCU交互。例如输入如下。

![](http://nodemcu-dev.doit.am/9.png) 　　 　　

将会得到如下输出： 　　

```
　　print("Hello World!")
　　Hello World!
```

　　第一个Lua代码执行成功，是不是很简单？

　　Tips:在程序调试过程中，需要查阅NodeMCU支持的API函数。查看地址：

　　[http://www.nodemcu.com/docs/node-module/或](http://www.nodemcu.com/docs/node-module/%E6%88%96)

　　[Home · nodemcu/nodemcu-firmware Wiki · GitHub](https://github.com/nodemcu/nodemcu-firmware/wiki/nodemcu_api_cn)

# GPIO测试

　　使用LuaLoader连接到NodeMCU后，可以使用LuaLoader自带的快捷功能实现对NodeMCU的操作。这些功能与直接使用Lua代码执行的效果等同。

　　根据NodeMCU API可以查知中ESP8266的GPIO16映射到0号IO口。即使用Lua对0号IO口操作，将会在GPIO16上实现。

　　使用LuaLoader右侧“GPIO”组的功能可以完成GPIO测试。

　　例如设置0号IO口为输出模式。只需要选择“0 GPIO16”，“Output”，然后点击“Set”即可。此时Lualoader向NodeMCU写入“gpio.mode(0,gpio.OUTPUT)”设置完成。点击“0”或“1”即可设置该端口为低电平输出或者高电平输出。输出的代码为：“gpio.write(0,gpio.LOW)”或“gpio.write(0,gpio.HIGH)”

　　**Tips：由于0号端口连接到板载LED，当将0号端口设置为输出模式，输出低电平时，LED将会被点亮，否则熄灭。** 　　 　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

![](http://nodemcu-dev.doit.am/10.png)

　　 　　运行效果：

```
gpio.mode(0,gpio.OUTPUT)
> gpio.write(0,gpio.LOW)
> gpio.write(0,gpio.HIGH)
>
```

　　为了测试1号端口的输入功能，使用一根杜邦线连接“1”号端口和GND。然后选择“1 GPIO5”，“Input”，“Pullup”，点击“Set”，可以完成对该端口的输入设置。点击“Read”可以执行一次读操作，并返回结果“0”。当将杜邦线连接到“1”号端口和3V3端口时，使用“Read”将返回“1”。旁边的定时器功能可以周期性读取。 　　 　　 　　 　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　

![](http://nodemcu-dev.doit.am/11.png)

　　运行效果：

```
gpio.mode(1,gpio.INPUT,gpio.PULLUP)
> = gpio.read(1)
1
> = gpio.read(1)
0
>
```

　　**Tips：将鼠标停留在LuaLoader按钮上方，可以显示该按钮的功能说明。**

　　**Tips：如果在LuaLoader主界面上显示的字符太多，影响阅读，可以使用软件下方的“Clear”按钮进行清屏。**



# 系统命令测试

　　在Lualoader中提供了NodeMCU常见的系统命令。比如：读取剩余内存容量的Heap按钮。读取ESP8266芯片ID的“chipID”按钮，软件重启按钮“Restart”以及停止指定定时器的“tmr.stop”按钮。

![](http://nodemcu-dev.doit.am/12.png)

　　其中“tmr.stop”可以通过右键点选需要停止的定时器编号。 　　 　　　　　　　

![](http://nodemcu-dev.doit.am/13.png)

　　上述按钮运行效果： 　　 　　

![](http://nodemcu-dev.doit.am/22.png)

　　NodeMCU重启时包含有乱码，是因为NodeMCU启动时的波特率是74880bps，不是NodeMCU默认的9600bps。 　　

　　“lua:cannot open init.lua”表示固件启动的时候没有找到init.lua文件。这是因为NodeMCU在启动时预留了init.lua作为应用程序入口，如果没有该文件则忽略，如果存在则开始执行该文件。利用这个特性可以在init.lua中写入需要执行的代码，以便上电自动运行。



# WiFi测试

　　NodeMCU的WiFi具有AP（Access Point）、STA（STATION）、AP+STA三种模式。AP模式下，NodeMCU作为热点，发出无线信号，接受其他节点连接，相当于一台无线路由器。STA模式下，NodeMCU作为一个节点，可以连接到其他热点（智能手机或无线路由器上）。

　　Lualoader提供了对NodeMCU在STA模式进行测试的功能。关于AP和AP+STA模式的测试请查阅NodeMCU文档。在下一章中将给出示例代码。 

![](http://nodemcu-dev.doit.am/14.png)

　　“SSID”和“Password”编辑框用于输入无线路由器的SSID和密码。

　　“Survey”是搜索NodeMCU所在环境的SSID，并打印出来。等同于下列程序。 　　

```
wifi.setmode(wifi.STATION) 
wifi.sta.getap(function(t) 
if t then print("\n\nVisible Access Points:\n") 
for k,v in pairs(t) do l = string.format("%-10s",k) print(l.."  "..v) end 
else 
print("Try again") 
end
end)
```

程序的执行结果如下：

![](http://nodemcu-dev.doit.am/23.png)

“Set AP”是使用SSID和Password的内容连接到无线路由器。

![](http://nodemcu-dev.doit.am/24.png)



# 文件操作

　　NodeMCU使用文件来保存Lua代码，得益于Lua的解释性执行特性，Lua源程序可以在线编译运行，因此文件操作非常重要。 NodeMCU支持多种文件操作。常用的文件操作有文件上传、删除、编译为二进制文件、运行等。Lualoader中提供了丰富的文件操作功能。

![](http://nodemcu-dev.doit.am/15.png)

　　（1）上传文件 　 “Upload File”：上传编写好的lua源代码文件到NodeMCU中。例如上传init.lua文件。Init.lua示例程序内容如下：

```
print("\n")
print("NodeMCU Started")
print("Type something to start")
```

 点击“Upload File”： 　　　　 　　　　 　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　 

![](16.png) 　　 　　

上传完成后： 　　 　　

![](17.png) 　

**　Tips：上传完成后，在“Upload File”按钮下方的可编辑下拉列表框中将显示该文件的文件名，后续操作都是基于该选择。如果有多个文件上传，该下拉列表框将记录所有已经上传过的文件名称。旁边的“ ”按钮是打开lua文件编辑器，实现文件编辑。** 　 　　（2）查看文件 　　如果需要查看该文件，可以使用“cat”按钮。点击“cat”按钮后，Lualoader下载如下程序到NodeMCU中。 

```
print("\n\ncat init.lua\n") 
file.open( "init.lua", "r") 
while true 
do 
line = file.readline() 
if (line == nil) then break 
end 
print(string.sub(line, 1, -2)) 
tmr.wdclr()
end 
file.close()
```

　　其中tmr.wdclr()用于清除看门狗，避免触发看门狗。程序运行结果： 　 　

```
　
cat init.lua
print("\n")
print("NodeMCU Started")
print("Type something to start")
```

 **Tips：一个长时间的循环或者事务，需内部调用tmr.wdclr()清除看门狗计数器，防止看门狗计数器溢出导致重启。**

　　（3）运行文件

　　点击按钮“dofile”，运行该文件，输出执行结果。该命令相当于执行：dofile("init.lua"):

```
dofile(init.lua)  2015年4月19日  16:07:24
dofile("init.lua")
NodeMCU Started
Type something to start
```

 （4）文件编译

　　点击“compile”按钮实现文件的编译，相当于运行：node.compile()。编译完成后，自动生成“文件名.lc”文件

　　node.compile("init.lua")

　　如果将init.lua文件中print("NodeMCU Started") 语句改为：print("NodeMCU Started" 编译将出现错误，将会给出响应的提示。

　　node.compile("init.lua")

　　stdin:1: init.lua:3: ')' expected (to close '(' at line 2)

　　near 'print'

　　**Tips：Lua程序的二进制文件为.lc格式。将.lua格式的源程序转为二进制文件运行可以提高程序效率，降低内存使用。因此建议在程序调试完成需要运行时，使用.lc文件运行。**

　　（5）运行lc文件 点击按钮“dolc”，运行lc文件，输出执行结果。该命令相当于执行：dofile("init.lc")，执行效果与dofile("init.lc")相同。

　　（6）列出文件列表

　　点击按钮“file.list”，该操作相当于执行： for k,v in pairs(file.list()) do l = string.format("%-15s",k) print(l.." "..v.." bytes") end

　　执行结果如下：

　　init.lua 70 bytes

　　init.lc 160 bytes

　　（7）删除文件

　　点击按钮“remove”，该操作相当于执行：file.remove("文件名.lua")。

　　更多关于LuaLoader的功能，请参考： 　　

[http://benlo.com/esp8266/index.html#LuaLoader。](http://benlo.com/esp8266/index.html#LuaLoader%E3%80%82)

　　（8）格式化文件系统 点击按钮“Format”按钮，格式化文件系统，该操作相当于执行：

　　file.format("文件名.lua")。

　　file.format()

　　format done.



# 程序编辑

　　Lua程序编写可以直接使用记事本，也可以使用专用调试编辑IDE。本教程推荐使用LuaStudio。软件如下图所示。

![](http://nodemcu-dev.doit.am/18.png)

　　在LuaStudio中源文件的管理采用工程模式。在菜单“工程”中可以新建工程或者添加现有工程。

　　右侧“解决方案”选项卡里面可以查看现有工程。在工程名称上使用右键可以对源文件进行管理。例如新建、添加现有文件、删除、重命名等。

　　在编辑lua文件时，LuaStudio可以高亮关键词、采用不同颜色显示数据类型、进行输入提示等，非常方便上手。

　　编辑完成的lua文件通过LuaLoader下载到NodeMCU中运行。

　　**Tips：如果编写的Lua程序有BUG，下载到NodeMCU之后运行出现问题导致程序不能通过串口下载或者不能正常执行串口命令（例如file.formart()语句已经无法下载到NodeMCU）。此时需要重新烧写固件。**




