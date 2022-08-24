# zcu102纯usb启动：
#### 2020.1（ug1209教程版本）和2022.1（要写的blog版本）现象一致：

|启动阶段|上电|写入BOOT.bin后|启动dfu_ram后|
|  :---:  |  :---:  |  :---:  |  :---:  |
|   linux dfu-util识别为  |    Xilinx DFU Downloader    |       Xilinx DFU Downloader     |      image.ub|
|       win10 zadig识别为     | Xilinx DFU Downloader  |         DFU 2.0 emulation v 1.1   |    USB download gadget|
|  win10设备管理器识别为    | DFU 2.0 emulation v 1.1       |   DFU 2.0 emulation v 1.1   |    USB download gadget
|  |     或   Xilinx DFU Downloader      |     或 Xilinx DFU Downloader  |       
|         |        有哪个驱动就被识别成哪个      |    有哪个驱动就被识别成哪个| |                 
| win10 dfu-util识别为     | Xilinx DFU Downloader       |     Xilinx DFU Downloader   |    image.ub |

#### 总结：
+ linux中可以无视驱动一路直接运行；
+ win10中提前安装好DFU 2.0 emulation v 1.1和Xilinx DFU Downloader二者其中之一外加USB download gadget驱动就可以一路成功运行；
+ 电脑上没有任何驱动，上电后按照zadig的识别结果先后安装Xilinx DFU Downloader和USB download gadget可以成功运行；
+ DFU 2.0 emulation v 1.1和Xilinx DFU Downloader 的device ID相同，都为03fd:0050; USB download gadget的device ID为03fd:0300;

# vck190prod的usb as secondary boot device启动：

#### 2020.2（github原example版本）现象和2022.1（新的github example版本不一致）：
|启动阶段|写入primary pdi后(jtag和sd一样)|
|  :---:  |  :---:  |
 |               linux dfu-util识别为     |     Xilinx DFU Downloader  |         
  |                  win10 zadig识别为    |     DFU 2.0 emulation v 1.1   |        
|   win10设备管理器识别为   |    DFU 2.0 emulation v 1.1   |      
|	 win10 dfu-util识别为     |     Xilinx DFU Downloader      |      


####		2020.2版本project总结:
+ linux中可以无视驱动成功使用dfu写入secondary pdi；
+ win10中如果电脑中有DFU 2.0 emulation v 1.1和Xilinx DFU Downloader任何一个驱动，plm启动后会直接挂掉，显示Image Header Table Validation failed；
+ win10中如果没有预先装任何驱动，可以发现设备管理器和zadig都将设备识别为DFU 2.0 emulation v 1.1，此时，plm正常停在
```
[968.692618]Loading PDI from USB 
[971.496271]Monolithic/Master Device，
```
   但之后一旦安装驱动plm就会挂掉，log显示同上；
+ 用了能想到的所有办法都不能在win10中正常使用dfu-util；                                                     
                    
                    
####		2022.1版本project总结：
+ linux中可以无视驱动成功使用dfu写入secondary pdi；
+ win10中如果电脑中预装有DFU 2.0 emulation v 1.1驱动可以成功使用vitis 2022.1安装目录下的dfu写入secondary pdi。
+ win10中如果没有任何驱动，使用zadig安装DFU 2.0 emulation v 1.1驱动后可以正常使用dfu-util。
		    
# 结论
+ 根据实验现象，使用2020.2版本工具制作的boot_primary.bin中的usb驱动和zadig提供的DFU 2.0 emulation v 1.1驱动大概率不匹配。
