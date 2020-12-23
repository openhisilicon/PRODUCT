
# Hi3516DV300-SOM-BaseBoard
**Author: thomas@hiview-tech.cn**

## Hi3516DV300

* 1, Dual-core ARM A7-900;
* 2, 5M@30FPS venc/vdec;
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

## Quick Use Guide


#### serial console
* Baud rate -115200
* Data bit -8
* Parity - none
* Stop bit -1
* Flow control - None

#### Terminal of network
* Default IP of the board: 192.168.0.2
* Telnet default username -root, password - none


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
* **How do open WIFI**
  The system uses the cable network port by default. If you need to test WIFI, please do the following(wifi password file: /app/wifi/wpa.conf):
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
  * Create GPIO node:
    * echo 27 > /sys/class/gpio/export
    * echo 28 > /sys/class/gpio/export
    * echo low > /sys/class/gpio/gpio27/direction
    * echo low > /sys/class/gpio/gpio28/direction
  *  Set GPIO value: 
        * `echo 0 > /sys/class/gpio/gpio27/value;echo 1 > /sys/class/gpio/gpio28/value;sleep 0.1;echo 0 > /sys/class/gpio/gpio28/value`
        * `echo 1 > /sys/class/gpio/gpio27/value;echo 0 > /sys/class/gpio/gpio28/value;sleep 0.1;echo 0 > /sys/class/gpio/gpio27/value`

* **How do open MIPI-DSI**
    * modify mod/codec/ipc/codec.c:mipi_800x1280 = 1;
      recompile the codec module and copy codec.exe file to /app/bin/codec.exe;
    * MIPI-DSI reset pin is GPIO0-5 and can be reset at before running codec.exe:
        * echo 5 > /sys/class/gpio/export
        * echo out > /sys/class/gpio/gpio5/direction
        * `echo 0 > /sys/class/gpio/gpio5/value;sleep 0.5;echo 1 > /sys/class/gpio/gpio5/value`

* **Burn files**
    link：https://pan.baidu.com/s/1GH7AbrUrEHT12cW83srevw code：x2nc 
* **SDK files**
    link：https://pan.baidu.com/s/1Z3DFCY-12PX5luIVQfzwiA code：1j64 
