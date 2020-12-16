
# Hi3516DV300核心板与底板

## Hi3516DV300参数

* 1, 双核A7-900;
* 2, 5M@30FPS编码/解码;
* 3, MIPI-CSI 4L/2Lx2输入;
* 4, MIPI-DSI 4L/LCD/HDMI输出;
* 5, 1Tops NNIE神经网络加速器;

## 核心板参数

* 1, DDR3 1GByte;
* 2, SPI-NAND 512MByte;  
* 3, 180Pin全功能连接器;

## 底板参数

* 1, DC12V, UART0, 100M-RJ45, USB2.0, HDMI1.4;
* 2, RTL8189FTV-WIFI, MIPI-CSI, MIPI-DSI;
* 3, KEY,LED,GPIO,UART1,RS485,AUDIO-IN-OUT;

## 快速使用指南


#### 串口终端
*  波特率-115200
*  数据位-8
*  奇偶校验-无
*  停止位-1
*  流控-无

#### 网口终端

* 板子默认IP: 192.168.0.2
* telnet默认用户名-root, 密码-无 


#### 启动脚本

* /etc/init.d/S99app
    调用/factory/startup.sh
* /factory/startup.sh
    初始化网络IP,判断是否存在升级包文件需要加压,启动/app/startapp.sh
* /factory/cfg/bsp_def.json
    保存不被终端用户修改的默认参数, sensor类型在此文件配置
* /app/startapp.sh
    初始化环境变量,启动bsp.exe,codec.exe,svp.exe,...

#### FAQ
* 如何测试WIFI
    系统默认使用用线网口,如需要测试wifi请进行以下操作:
    * mv /app/startapp.sh /app/startapp.sh.bak
    * reboot
    * /app/wifi/wifi.sh
    * /app/startapp.sh.bak
* 如何编译app程序
    * 下载源代码 https://github.com/openhisilicon/HIVIEW
    * 设置编译环境 source build/3516d
    * 编译代码 make
    * 制作网络升级包 ./ins.sh 16d
    * 设备升级 使用Google Chrome 打开设备 http://192.168.0.2 在页面最下方 Upload file
    
* 烧录文件下载
  链接：https://pan.baidu.com/s/1GH7AbrUrEHT12cW83srevw 提取码：x2nc 
* 海思SDK下载
  链接：https://pan.baidu.com/s/1Z3DFCY-12PX5luIVQfzwiA 提取码：1j64 
