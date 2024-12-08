Vendor of the products:　D-Link

Affected products:　DSL-3782 v1.01

# Buffer overflow

首先看路由器的web界面，漏洞界面位于Features的Website Filter中

![image-20241207204409366](IOTfuzz相关资料.assets/image-20241207204409366.png)

在Only deny computers with IP address listed below to access the network选项中随便填写，使用burpsuite抓包

![image-20241207204510327](IOTfuzz相关资料.assets/image-20241207204510327.png)

可以看到请求包，首先获取key，接着发出对应请求，修改对应字段keywords重发

先获取sessionkey，复制到对应字段

![image-20241208133536002](IOTfuzz相关资料.assets/image-20241208133536002.png)

发送

![image-20241208133742046](IOTfuzz相关资料.assets/image-20241208133742046.png)

可以看到路由器崩溃了

![image-20241208133824210](IOTfuzz相关资料.assets/image-20241208133824210.png)

可以计算得出偏移为156

![image-20241208133955317](IOTfuzz相关资料.assets/image-20241208133955317.png)

## Code in cfg_manager

根据路径信息锁定ParentalControl.asp文件

![image-20241208134120963](IOTfuzz相关资料.assets/image-20241208134120963.png)

根据UrlFilter_Entry可以确定代码在cfg_manager文件中

![image-20241208134204886](IOTfuzz相关资料.assets/image-20241208134204886.png)

ida查看cfg_manager，可以看到strcpy在拷贝url参数时导致了溢出

![image-20241208134400197](IOTfuzz相关资料.assets/image-20241208134400197.png)

