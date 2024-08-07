
# Hi3516DV300-SOM-BaseBoard
**Author: thomas@hiview-tech.cn**

## Hi3516DV300

* 1, Dual-core ARM A7-900;
* 2, 4M@30FPS venc/vdec;
* 3, MIPI-CSI 4L/2Lx2 input;
* 4, MIPI-DSI 4L/LCD/HDMI output;
* 5, 1Tops Neural network acceleration engine;

## SOM

* 1, DDR3 1GByte;
* 2, SPI-NAND 512MByte;  
* 3, 180Pin Full function connector;

## BaseBoard

* 1, DC12V, UART0, 100M-RJ45, USB2.0, SDCard, HDMI1.4;
* 2, RTL8189FTV-WIFI, MIPI-CSI, MIPI-DSI;
* 3, KEY,LED,GPIO,UART1,RS485,AUDIO-IN-OUT;


## SOM-BaseBord Specification
  https://github.com/openhisilicon/PRODUCT/blob/master/HI3516DV300_SOM%20datasheet%20_E.pdf


## Quick Use Guide


#### serial console
* J10 slot
* Baud rate -115200
* Data bit -8
* Parity - none
* Stop bit -1
* Flow control - None

#### Terminal of network
* Default IP of the board: 192.168.0.2
* Telnet default username -root, password - none
* Note: New firmware Telnet/SSH username -root, password - hiview

#### startup script

* /etc/init.d/S99app
    Start /factory/startup.sh
* /factory/startup.sh
    Initialize the network IP, unpack if there is an upgrade package file, and start /app/startapp.sh
* /factory/cfg/bsp_def.json
    Save the default parameters that are not modified by the end user. The sensor type is configured in this file
* /app/startapp.sh
    Initialize the environment variables, start the bsp.exe,codec.exe,svp.exe,...

#### FAQ
* **How do open WIFI-STA**
  The system uses the cable network port by default. If you need to test WIFI-STA, please do the following(wifi password file: /app/wifi/wpa.conf):
    * mv /app/startapp.sh /app/startapp.sh.bak
    * reboot
    * /app/wifi/wifi.sh
    * /app/startapp.sh.bak
* **How do compile APP**
    * Download the source code: https://github.com/openhisilicon/HIVIEW
    * Set environment: source build/3516d
    * Compile: make
    * Make upgrade file: ./ins.sh 16d
    * Upload upgrade file: Use Google Chrome to open  http://192.168.0.2 and  Upload File at “Upload file using Ajax”
    
* **How do use the left space in FLASH**
  * current uboot MTD partitions: uboot:1M,  kernel: 4M, rootfs: 120M, user: 387M (not use)
  * To use a user partition, do the following:
  * 1 Format the UBI partition (do this only once):
      *  ubiformat /dev/mtd3
      *  ubiattach /dev/ubi_ctrl -m 3
      *  ubinfo /dev/ubi1
      *  ubimkvol /dev/ubi1 -N user -s xxxMiB 
         (xxx from the ubinfo command output available space size,
        delete the volume if you type incorrectly:  ubirmvol /dev/ubi1 -N user)
      *  mount -t ubifs /dev/ubi1_0 /mnt/ 
    * 2 mount UBI partition (mount is required every  powered on):
      *  ubiattach /dev/ubi_ctrl -m 3
      *  mount -t ubifs /dev/ubi1_0 /mnt/
  
* **How do switch IR-CUT**
  * IR-CUT is GPIO3-3 and GPIO3-4, which is controlled by the kernel standard GPIO interface:
  * Create GPIO node(Added to /factory/startup.sh in new software):
    * echo 27 > /sys/class/gpio/export
    * echo 28 > /sys/class/gpio/export
    * echo low > /sys/class/gpio/gpio27/direction
    * echo low > /sys/class/gpio/gpio28/direction
    *  IRCUT switch:
        * Day:  `echo 1 > /sys/class/gpio/gpio27/value;echo 0 > /sys/class/gpio/gpio28/value;sleep 0.1;echo 0 > /sys/class/gpio/gpio27/value`
        * Night:`echo 0 > /sys/class/gpio/gpio27/value;echo 1 > /sys/class/gpio/gpio28/value;sleep 0.1;echo 0 > /sys/class/gpio/gpio28/value`
    *  IMX334-IRCUT switch:
        * Day:  `echo 1 > /sys/class/gpio/gpio27/value;echo 1 > /sys/class/gpio/gpio28/value`
        * Night:`echo 0 > /sys/class/gpio/gpio27/value;echo 0 > /sys/class/gpio/gpio28/value`

* **How do open MIPI-DSI**
    * Turn on/off MIPI output flag: touch /app/mipi or rm /app/mipi;
      (Note: In the old software, you need to modify mod/src/ipc/codec.c:int mipi_800x1280 = 0/1, and update codec.exe to the board)
    * The MIPI-DSI reset pin is GPIO0-5, please follow the following command:
        * mv /app/startapp.sh /app/startapp.sh.bak
        * reboot
        * echo 5 > /sys/class/gpio/export
        * echo out > /sys/class/gpio/gpio5/direction
        * `echo 0 > /sys/class/gpio/gpio5/value;sleep 0.5;echo 1 > /sys/class/gpio/gpio5/value`
        * /app/startapp.sh.bak

* **How do USB-BURN**
    * Please reference /ReleaseDoc/en/01.software/pc/HiTool/HiBurn User Guide.pdf [1.5 Environment Preparation]
    * Note: Switch update-key to GND when powering on. If WIN10 cannot find the device, please use WIN7

* **How do ETH-BURN**
    * Please reference /ReleaseDoc/en/01.software/pc/HiTool/HiBurn User Guide.pdf
    * Note: Switch update-key to UPDATE when powering on.

* **Burn files**
    link：https://pan.baidu.com/s/1GqM2OvlGFG9rshOoWS-I7Q code：3ln1 
* **SDK files**
    link：[`Please contact us for download`] 

