
# Hi3516DV300视频编码板
**作者: thomas@hiview-tech.cn**

## 视频输入与编码

* 1, HDMI1.4   1080P60FPS;
* 2, HD-SDI-3G 1080P60FPS;
* 3, SONY-LVDS 1080P60FPS;
* 4, 视频编码  1080P60FPS + 480P60FPS;
* 5, 音频编码  AAC;

## 快速使用指南


#### 串口终端
*  J10-座子
*  波特率-115200
*  数据位-8
*  奇偶校验-无
*  停止位-1
*  流控-无

#### 网口终端

* 板子默认IP: 192.168.0.2
* telnet/ssh默认用户名-root, 密码-hiview 

#### FAQ
* **如何在WEB端打开音频**
    * 在视频预览页面, 使能audio高亮, 再点击Play;

* **如何切换视频源**
    * RED-1 OFF:HDMI, ON:SDI;
    * SW2-2 OFF:HDMI/SDI, ON:SONY-LVDS;

* **如何切换音频源**
    * RED-2 OFF:HDMI/SDI, ON:LINE-IN(RED-side);

* **注释**
    * RED 表示接口板上的红色拨码开关;
    * SW2 表示接口板上的黑色拨码开关;
    * RED-side 表示靠近红色拨码开关位置;
    * 其他请参考 hi3516dv300-som-zh.md;


* **烧录文件下载**
  链接：https://pan.baidu.com/s/1GqM2OvlGFG9rshOoWS-I7Q 提取码：3ln1 
* **海思SDK下载**
  链接：[`请联系我们获取下载方式`]
