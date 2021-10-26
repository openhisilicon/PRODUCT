
# Hi3559V200核心板与底板
**作者: thomas@hiview-tech.cn**

## Hi3559V200参数

* 1, 双核A7-900;
* 2, 4K@30FPS编码/解码;
* 3, MIPI-CSI 4L/2Lx2输入;
* 4, MIPI-DSI 4L/LCD/HDMI输出;
* 5, 0.4Tops NNIE神经网络加速器;

## 核心板参数

* 1, DDR3 1GByte;
* 2, SPI-NAND 512MByte;  
* 3, 180Pin全功能连接器;

## 底板参数

* 1, DC12V, UART0, USB2.0, SDCard, HDMI1.4;
* 2, RTL8189FTV-WIFI, MIPI-CSI, MIPI-DSI;
* 3, KEY,LED,GPIO,UART1,RS485,AUDIO-IN-OUT;

## 核心板与底板规格书
  https://github.com/openhisilicon/PRODUCT/blob/master/HI3559V200_SOM%20datasheet_C.pdf

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
* telnet默认用户名-root, 密码-无 
* 注意: 新的固件Telnet/SSH用户名-root，密码- hiview

#### 启动脚本

* /etc/init.d/S99app
    调用/factory/startup.sh
* /factory/startup.sh
    初始化网络IP,判断是否存在升级包文件需要加压,启动/app/startapp.sh
* /factory/cfg/bsp_def.json
    保存不被终端用户修改的默认参数, sensor类型在此文件配置
* /app/startapp.sh
    初始化环境变量,启动bsp.exe,codec.exe,svp.exe,...

#### FAQ(有些功能已在web页面可控制)
* **如何测试WIFI-STA**
    系统默认使用用线网口,如需要测试wifi-sta请进行以下操作(wifi密码文件/app/wifi/wpa.conf):
    * mv /app/startapp.sh /app/startapp.sh.bak
    * reboot
    * /app/wifi/wifi.sh
    * /app/startapp.sh.bak
* **如何编译app程序**
    * 下载源代码 https://github.com/openhisilicon/HIVIEW
    * 设置编译环境 source build/3559
    * 编译代码 make
    * 制作网络升级包 ./ins.sh 59
    * 设备升级 使用Google Chrome 打开设备 http://192.168.0.2 在页面最下方 Upload file

* **如何使用FLASH的剩余空间**
    * 当前MTD分区 uboot:1M,kernel:4M,rootfs:120M,user:387M(未使用)
    * 如果要使用user分区,请进行如下操作:
    * 1 格式化UBI分区(只需要执行一次此操作):
      *  ubiformat /dev/mtd3
      *  ubiattach /dev/ubi_ctrl -m 3
      *  ubinfo /dev/ubi1
      *  ubimkvol /dev/ubi1 -N user -s xxxMiB 
        (xxx 来源于ubinfo命令输出可用的空间大小, 
        如果输入错误可删除卷: ubirmvol /dev/ubi1 -N user)
      *  mount -t ubifs /dev/ubi1_0 /mnt/ 
    * 2 挂载UBI分区(每次上电都需要执行挂载):
      *  ubiattach /dev/ubi_ctrl -m 3
      *  mount -t ubifs /dev/ubi1_0 /mnt/

* **如何切换 IR-CUT**
    *  IR-CUT对应的GPIO为 GPIO3-3,GPIO3-4, 使用内核标准GPIO操作接口控制:
    *  创建GPIO操作节点(在新的软件中已增加到/factory/startup.sh):
        * echo 27 > /sys/class/gpio/export
        * echo 28 > /sys/class/gpio/export
        * echo low > /sys/class/gpio/gpio27/direction
        * echo low > /sys/class/gpio/gpio28/direction
    *  IRCUT切换:
        * Day:  `echo 1 > /sys/class/gpio/gpio27/value;echo 0 > /sys/class/gpio/gpio28/value;sleep 0.1;echo 0 > /sys/class/gpio/gpio27/value`
        * Night:`echo 0 > /sys/class/gpio/gpio27/value;echo 1 > /sys/class/gpio/gpio28/value;sleep 0.1;echo 0 > /sys/class/gpio/gpio28/value`
    *  IMX334-IRCUT切换:
        * Day:  `echo 1 > /sys/class/gpio/gpio27/value;echo 1 > /sys/class/gpio/gpio28/value`
        * Night:`echo 0 > /sys/class/gpio/gpio27/value;echo 0 > /sys/class/gpio/gpio28/value`
* **如何测试MIPI-DSI屏幕**
    * 打开或关闭mipi输出标识: touch /app/mipi or rm /app/mipi;
      (注: 旧的软件版本需要修改mod/src/ipc/codec.c:int mipi_800x1280 = 0/1, 并更新codec.exe到板端)
    * MIPI-DSI复位引脚为GPIO0-5, 请按照以下命令操作:
        * mv /app/startapp.sh /app/startapp.sh.bak
        * reboot
        * echo 5 > /sys/class/gpio/export
        * echo out > /sys/class/gpio/gpio5/direction
        * `echo 0 > /sys/class/gpio/gpio5/value;sleep 0.5;echo 1 > /sys/class/gpio/gpio5/value`
        * /app/startapp.sh.bak

* **如何测试USB网口**
    * Hi3559V200无ETH-PHY接口, 可通过usb-gadget虚拟网口或使用usb2eth网口转换器
    * 使用usb-device模式开启usb-gadget虚拟网口操作如下:
      * 在windows操作系统下安装usbrndis相关驱动,请参考:`ReleaseDoc/zh/01.software/board/OSDRV/外围设备驱动 操作指南.doc[1.2.3.6 内核下USB Device复合设备操作示例]`
      * 在板端执行: 
          * mv /app/startapp.sh /app/startapp.sh.bak
          * reboot
          * /app/usbgadget/usb.sh
          * /app/startapp.sh.bak

    * 使用usb-host模式,购买usb2eth连接器
      * 下载 uImage-uhost 镜像文件 [`请联系我们获取下载方式`]
      * 可通过HiTool烧录工具下载uImage-uhost到板端或通过如下方式烧录:
      * flash_erase /dev/mtd1 0 0
      * nandwrite -p /dev/mtd1 uImage-uhost

* **如何使用USB烧写**
    * 请参考 /ReleaseDoc/zh/01.software/pc/HiTool/HiBurn 工具使用指南.pdf [1.5 环境准备]
    * 注: 上电前切换update-key为GND, 如果WIN10无法找到设备,请使用WIN7进行测试

* **关于WIN10下无法使用USB烧录的问题**
    * win10下hitool手动控制usb烧录
    * (update-key保持在"update"即串口升级模式)

        * 1, 打开hitool, 选择CPU类型;
        * 2, 配置hitool, 选择串口烧录方式, 仅烧录boot分区;
        * 3, 配置hitool, 选择usb烧录方式, 选择除boot外的其他分区;
        * 4, 进入uboot输入 "usb device"
        * 5, 点击 "烧写", 当看到hitool提示如下时, 复位板子,再次在uboot输入 "usb device"; 
        `# ---- 100% `
        `Boot download completed!`
        * 6, 等待烧录完成;

* **烧录文件下载**
  链接：https://pan.baidu.com/s/1k525rZtWp54Xfu4Ie1C1rA 提取码：drf7 


