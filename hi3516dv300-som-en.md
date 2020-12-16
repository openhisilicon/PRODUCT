
# Hi3516DV300-SOM-BaseBoard

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

* 1, DC12V, UART0, 100M-RJ45, USB2.0, HDMI1.4;
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
* Open WIFI
  The system uses the cable network port by default. If you need to test WIFI, please do the following:
    * mv /app/startapp.sh /app/startapp.sh.bak
    * reboot
    * /app/wifi/wifi.sh
    * /app/startapp.sh.bak
* Compile APP
    * Download the source code: https://github.com/openhisilicon/HIVIEW
    * Set environment: source build/3516d
    * Compile: make
    * Make upgrade file: ./ins.sh 16d
    * Upload upgrade file: Use Google Chrome to open  http://192.168.0.2 and  Upload File at “Upload file using Ajax”
    
* Burn files
    link：https://pan.baidu.com/s/1GH7AbrUrEHT12cW83srevw code：x2nc 
* SDK files
    link：https://pan.baidu.com/s/1Z3DFCY-12PX5luIVQfzwiA code：1j64 
