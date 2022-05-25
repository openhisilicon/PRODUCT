
# Hi3559V200-SOM-BaseBoard 
**Author: thomas@hiview-tech.cn**

## Hi3559V200

* 1, Dual-core ARM A7-900;
* 2, 4K@30FPS venc/vdec;
* 3, MIPI-CSI 4L/2Lx2 input;
* 4, MIPI-DSI 4L/LCD/HDMI output;
* 5, 0.4Tops Neural network acceleration engine;

## SOM

* 1, DDR3 1GByte;
* 2, SPI-NAND 512MByte;  
* 3, 180Pin Full function connector;

## BaseBoard

* 1, DC12V, UART0, USB2.0, SDCard, HDMI1.4;
* 2, RTL8189FTV-WIFI, MIPI-CSI, MIPI-DSI;
* 3, KEY,LED,GPIO,UART1,RS485,AUDIO-IN-OUT;

## SOM-BaseBord Specification
  https://github.com/openhisilicon/PRODUCT/blob/master/HI3559V200_SOM%20datasheet%20_E.pdf


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
* Telnet/SSH username: root, password: hiview

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

* **How do compile APP** (https://www.bilibili.com/video/BV1PP4y1P79i)
    * Download the source code: https://github.com/openhisilicon/HIVIEW
    * Set environment: source build/3559
    * Compile: make
    * Make upgrade file: ./ins.sh 59
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
  
* **How do open USB-NET**
    * Hi3559V200 no ETH-PHY interface. you can be used the USB-gadget     virtual network port or the USB2ETH network port converter.
  
    * Use USB-Host mode and purchase a USB2ETH connector
      * download uImage-uhost file [`Please contact us for download`]
      * You can download uimage-Uhost to the board through the HiTool burn tool or burn it as follows:
      * flash_erase /dev/mtd1 0 0
      * nandwrite -p /dev/mtd1 uImage-uhost

    * Use USB-Device mode to enable the USB-gadget virtual network port operation as follows:
      * Please refer to installing the USBRNDIS drivers on the Windows: `/ReleaseDoc/en/01.software/board/OSDRV/Peripheral Driver Operation Guide.doc [1.2.3.6 Using the Composite Device Function of the USB Device Under Kernel]`
      * Execute on the device side: 
          * mv /app/startapp.sh /app/startapp.sh.bak
          * reboot
          * /app/usbgadget/usb.sh
          * /app/startapp.sh.bak

* **How do USB-BURN**
    * Please reference /ReleaseDoc/en/01.software/pc/HiTool/HiBurn User Guide.pdf [1.5 Environment Preparation]
    * Note: Switch update-key to GND when powering on. If WIN10 cannot find the device, please use WIN7

* **WIN10 can not use USB burning problem**
    * In Win10, Hitool manually controls USB burning
    * (Keep update-key in "Update" mode, i.e. serial port upgrade mode)

        * 1, Open Hitool and select a CPU type;
        * 2, Configure Hitool, select serial port burning mode, only burn boot partition;
        * 3, Configure Hitool, select usb burning mode, and select other partitions except boot;
        * 4, Enter "usb device" in uboot;
        * 5, Click "Burn". When you see the following message from Hitool, reset the board, enter "usb device" again in uboot;
        `# ---- 100% `
        `Boot download completed!`
        * 6, Wait for the completion of burning;


* **Burn files**
    link：https://pan.baidu.com/s/1k525rZtWp54Xfu4Ie1C1rA code：drf7 

