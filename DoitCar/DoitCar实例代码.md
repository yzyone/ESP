# 实例代码

---

# init.lua文件

　　NodeMCU在启动时预留了init.lua作为应用程序入口，如果没有该文件则忽略，如果存在则开始执行该文件。利用这个特性可以在init.lua中写入需要执行的代码，以便上电自动运行。

　　资料包中文件名为“init.lua”。

```
1     print("\n")
2     print("ESP8266 Started")
3     
4     local luaFile = {"fileName.lua"}
5     for i, f in ipairs(luaFile) do
6         if file.open(f) then
7           file.close()
8           print("Compile File:"..f)
9           node.compile(f)
10       print("Remove File:"..f)
11           file.remove(f)
12         end
13      end
14     
15     luaFile = nil
16     collectgarbage()
17     
18     dofile("fileName.lc");
```

　　程序第1、2行打印字符信息。

　　第4行定义需要编译的lua文件名。其中“fileName”需要根据不同的文件进行更改。如果要编译多个文件，在后面继续添加即可。例如： local luaFile = {"fileName1.lua","fileName2.lua"}

　　第5行是使用for循环完成多个文件的操作。

　　第6行判断文件是否存在，如果存在则执行编译。如果不存在则忽略。

　　第7行是关闭已经打开的文件。

　　第8~11行完成编译，自动生成“filename.lc”文件。

　　第15~16行是回收内存。

　　第17行是执行刚刚编译完成的二进制文件。 Tips：在Lua程序中，默认变量为全局类型，如果要限定变量只能在本文件中使用，需要在变量前加入关键词“local”。在使用完后，需要将其赋值为nil，然后调用collectgarbage()显示的回收内存。

　　下载程序，重启NodeMCU运行时，如果发现编译出现内存不足的警告。如下所示：

　　lua: init.lua:8: not enough memory

　　这是因为Lua程序在刚刚启动的时候，内存消耗较大，在启动完成后回到正常。因此可以在启动后，稍微延迟一定时间再进行编译。



# WiFi模式

如前所述，NodeMCU的WiFi具有AP（Access Point）、STA（STATION）、AP+STA三种模式。下面的代码演示了如何通过Lua代码设置这三种模式。

## AP模式

资料包中文件名为“ap.lua”。

将“init.lua”文件中“fileName.lua”修改为“ap.lua”，“fileName.lc”修改为“ap.lc”。修改完成后下载。

程序代码：

```
1     print("Ready to start soft ap")
2     
3     local str=wifi.ap.getmac();
4     local ssidTemp=string.format("%s%s%s",string.sub(str,10,11),string.sub(str,13,14),string.sub(str,16,17));
5     
6     cfg={}
7     cfg.ssid="ESP8266_"..ssidTemp;
8     cfg.pwd="12345678"
9     wifi.ap.config(cfg)
10     
11     cfg={}
12     cfg.ip="192.168.1.1";
13     cfg.netmask="255.255.255.0";
14     cfg.gateway="192.168.1.1";
15     wifi.ap.setip(cfg);
16     wifi.setmode(wifi.SOFTAP)
17     
18     str=nil;
19     ssidTemp=nil;
20     collectgarbage();
21     
22     print("Soft AP started")
23     print("Heep:(bytes)"..node.heap());
24     print("MAC:"..wifi.ap.getmac().."\r\nIP:"..wifi.ap.getip());
```

　　程序第1行打印字符信息。

　　第3、4行是获取AP模式下的MAC地址，以MAC地址后6位为AP的SSID，当然你也可以使用其他作为ID，比如通过node.chipid()得到ESP8266的芯片ID。

　　第6~9行设置AP模式下的SSID。SSID格式为“ESP8266_XXXXXX”，其中XXXXXX为MAC地址后6位。

　　第11~15行为设置模块的IP地址、子网掩码以及网关地址。

　　第16行调用wifi.setmode()函数设置执行。

　　第23行打印当前内存。

　　第24行打印mac地址和ip地址。

　　下载该程序，执行结果是使用无线网络设备可以搜索到NodeMCU发出来的AP信号。通过电脑连接到该SSID，见下图。

　　　　　　　　![](http://nodemcu-dev.doit.am/19.png)

![](http://nodemcu-dev.doit.am/20.png)

　　执行程序的Log如下：

```
1     NodeMCU 0.9.6 build 20150406  powered by Lua 5.1.4
2     
3     
4     ESP8266 Started
5     Compile File:ap.lua
6     Remove File:ap.lua
7     Readly to start soft ap
8     Soft AP started
9     Heep:(bytes)15328
10     MAC:1A-FE-34-A1-14-A7
11     IP:192.168.1.1
12     >
```

　　从上面可以看到ap.lua在被编译后删除了。

如果重新启动NodeMCU，程序Log如下：

1     NodeMCU 0.9.6 build 20150406  powered by Lua 5.1.4
2     
3     
4     ESP8266 Started
5     Ready to start soft ap
6     Soft AP started
7     Heep:(bytes)15048
8     MAC:1A-FE-34-A1-14-A7
9     IP:192.168.1.1
10     >

　　关于更多函数的功能请查阅：

　　[https://github.com/nodemcu/nodemcu-firmware/wiki/nodemcu_api_cn。](https://github.com/nodemcu/nodemcu-firmware/wiki/nodemcu_api_cn%E3%80%82)

## STA模式

　　资料包中文件名为“sta.lua”。

　　将“init.lua”文件中“fileName.lua”修改为“sta.lua”，“fileName.lc”修改为“sta.lc”。修改完成后下载。

　　程序代码：

```
1     print("Ready to Set up wifi mode")
2     wifi.setmode(wifi.STATION)
3     
4     wifi.sta.config("MERCURY_1013","123456789")
5     wifi.sta.connect()
6     local cnt = 0
7     tmr.alarm(3, 1000, 1, function() 
8         if (wifi.sta.getip() == nil) and (cnt < 20) then 
9         print("Trying Connect to Router, Waiting...")
10         cnt = cnt + 1 
11         else 
12         tmr.stop(3)
13         if (cnt < 20) then print("Config done, IP is "..wifi.sta.getip())
14         else print("Wifi setup time more than 20s, Please verify wifi.sta.config() function. Then re-download the file.")
15         end
16             cnt = nil;
17             collectgarbage();
18         end 
19          end)
```

　　程序第1行打印字符信息。

　　第2行设置WiFi为STA模式。

　　第4行设置STA模式下，将要连接的无线路由器SSID和密码。

　　第5行是发起连接。

　　第6~19行是启动3号定时器，每隔1000毫秒检查一次连接，如果超过20秒钟没有连接上无线路由器，则提示连接失败。如果在20秒内连接成功，则停止3号定时器，打印IP地址。

　　执行程序的Log如下：

```
1     NodeMCU 0.9.6 build 20150406  powered by Lua 5.1.4
2     
3     
4     ESP8266 Started
5     Compile File:sta.lua
6     Remove File:sta.lua
7     Ready to Set up wifi mode
8     > Trying Connect to Router, Waiting...
9     Trying Connect to Router, Waiting...
10     Config done, IP is 192.168.1.100
```

　　可以看到NodeMCU获取到IP地址为“192.168.1.100”。

## AP+STA

　　资料包中文件名为“apsta.lua”。

　　将“init.lua”文件中“fileName.lua”修改为“apsta.lua”，“fileName.lc”修改为“apsta.lc”。修改完成后下载。

　　程序代码：

```
1     print("Ready to start soft ap AND station")
2     local str=wifi.ap.getmac();
3     local ssidTemp=string.format("%s%s%s",string.sub(str,10,11),string.sub(str,13,14),string.sub(str,16,17));
4     wifi.setmode(wifi.STATIONAP)
5     
6     local cfg={}
7     cfg.ssid="ESP8266_"..ssidTemp;
8     cfg.pwd="12345678"
9     wifi.ap.config(cfg)
10     cfg={}
11     cfg.ip="192.168.2.1";
12     cfg.netmask="255.255.255.0";
13     cfg.gateway="192.168.2.1";
14     wifi.ap.setip(cfg);
15     
16     wifi.sta.config("MERCURY_1013","123456789")
17     wifi.sta.connect()
18     
19     local cnt = 0
20     gpio.mode(0,gpio.OUTPUT);
21     tmr.alarm(0, 1000, 1, function() 
22         if (wifi.sta.getip() == nil) and (cnt < 20) then 
23             print("Trying Connect to Router, Waiting...")
24             cnt = cnt + 1 
25                  if cnt%2==1 then gpio.write(0,gpio.LOW);
26                  else gpio.write(0,gpio.HIGH); end
27         else 
28             tmr.stop(0);
29             print("Soft AP started")
30             print("Heep:(bytes)"..node.heap());
31             print("MAC:"..wifi.ap.getmac().."\r\nIP:"..wifi.ap.getip());
32             if (cnt < 20) then print("Conected to Router\r\nMAC:"..wifi.sta.getmac().."\r\nIP:"..wifi.sta.getip())
33                 else print("Conected to Router Timeout")
34             End
35     gpio.write(0,gpio.LOW);
36             cnt = nil;cfg=nil;str=nil;ssidTemp=nil;
37             collectgarbage()
38         end 
39     end)
```

 程序第1行打印字符信息。 　　第2~14行设置AP；第16~17行设置STA参数。 　　第19~39行是设置定时器0，每隔1秒钟检查一次模块是否连接到无线路由器。 　　第20行设置D0即GPIO16为输出模式，改端口连接到NodeMCU的LED灯，通过第25~26行代码指示模块WiFi连接情况。 　　执行程序的Log如下：

```
1     NodeMCU 0.9.6 build 20150406  powered by Lua 5.1.4
2     
3     
4     ESP8266 Started
5     Compile File:apsta.lua
6     Remove File:apsta.lua
7     Ready to start soft ap AND station
8     > Trying Connect to Router, Waiting...
9     Trying Connect to Router, Waiting...
10     Trying Connect to Router, Waiting...
11     Soft AP started
12     Heep:(bytes)16192
13     MAC:1A-FE-34-A1-14-A7
14     IP:192.168.2.1
15     Conected to Router
16     MAC:18-FE-34-A1-14-A7
17     IP:192.168.1.100
```





# PWM功能

　　NodeMCU的D1~12管脚均具有PWM功能（除了D0）。 PWM频率可以设置范围1~1000Hz，占空比设置范围为0~1023（对应0%~100%）

　　资料包中文件名为“pwm.lua”。

　　将“init.lua”文件中“fileName.lua”修改为“pwm.lua”，“fileName.lc”修改为“pwm.lc”。修改完成后下载。 PWM功能程序代码：

```
1     print("PWM Function test")
2     pwm.setup(1,1000,1023);
3     pwm.start(1);
4     
5     local r=512;
6     local flag=1;
7     tmr.alarm(2,100,1,function()
8         pwm.setduty(1,r);
9         if flag==1 then 
10         r=r-50;        if r《0 then flag=0 r=0 end
11         else
12         r= r+50;    if r>1023 then flag=1 r=1023 end
13     end
14     end)
```

　　程序第1行打印字符信息。

　　第2行设置端口1的频率为1000Hz，占空比为1023.

　　第3行启动PWM输出。

　　第5~14行设置了定时器2，每隔100毫秒，对占空比r进行增加或者减小循环操作。通过变量flag进行记录。 下载程序，通过万用表或者示波器可以看到D1端口的电平输出变化。如果在D1端口和GND之间接入一个LED二极管。可以查看到二极管的亮度循环变化。

![](http://nodemcu-dev.doit.am/21.png)

　 **Tips：LED的长管脚为阳极，接D1；短管脚为阴极，接GND。**

　　执行程序的Log如下：

```
1     NodeMCU 0.9.6 build 20150406  powered by Lua 5.1.4
2     
3     
4     ESP8266 Started
5     Compile File:pwm.lua
6     Remove File:pwm.lua
7     PWM Function test
8     >
```



# 资料汇总

（1）乐鑫ESP8266官方网站

http://espressif.com/zh-hans/

（2）芯片手册地址： Nodemcu：

[NodeMCU · GitHub](https://github.com/nodemcu)

NodeMCU的API：

[GitHub - nodemcu/nodemcu-firmware: Lua based interactive firmware for ESP8266, ESP8285 and ESP32](https://github.com/nodemcu/nodemcu-firmware)

（3）Lua：

http://www.lua.org/

[lua_百度百科](http://baike.baidu.com/view/416116.htm)

http://zh.wikipedia.org/wiki/Lua

（4）eLua:

http://www.eluaproject.net/

（5）LuaLoader

[LuaLoader for ESP8266](http://benlo.com/esp8266/index.html#LuaLoader)

[GitHub - GeoNomad/LuaLoader: A serial communications tool and IDE for the NodeMcu Lua firmware for the ESP8266](https://github.com/GeoNomad/LuaLoader)



技术支持
[论坛 - Powered by Discuz!](http://bbs.doit.am/forum.php)
