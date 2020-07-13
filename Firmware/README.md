# Firmware files:

### Gecko Bootloaders:
The ICC-A-1 module have an Gecko Application Bootloader installed that only works with OTA files downloaded thrue Zigbee and stored on the external flash.  
  
So first we need to flash one new bootloader in the internal flash for loading the NCP app in the internal flash.  
  
Flashing new bootloader to the gecko must being done with J-tag or SWD and using elf (not recomended) or s37 files.    
  
Its normaly OK flashing without erasing the internal flash, then you can flash the [icc-a-1-bootloader.s37](icc-a-1-bootloader.s37) that contain only the secund stage (Main) bootloader.  
If erasing the internal flash you must flashing the [icc-a-1-bootloader-combined.s37](icc-a-1-bootloader-combined.s37) or [BTL_STD_S1_256-COM_PB14-PB15-PA0.s37](BTL_STD_S1_256-COM_PB14-PB15-PA0.s37) that contains both the first stage and secund stage (Main) bootloader, or your gecko dont boot at all or making cracy GECKO things !  
I have booting in to bootloader after erasing internal flash and flashing botloader and reading it back and the first 0x0800 wos only ff = empty, and from the bootloader flashing one app and it wos written it ofset and coruppted the perfials and lockbits aria = Hard brick !!!  

### Gecko APP = "Billy EZSP":
NCP app can being flashed from main bootloader with gbl files or with  J-tag / SWD with elf file @0x04000 (not recomended) or s37 file.  
My current NCP gbl file its not accsepted bye the bootloader but the s37 version flashed over SWD works OK.  
The [NCP_USW_115K2_S1_F256_676_PB14-PB15.s37](NCP_USW_115K2_S1_F256_676_PB14-PB15.s37) its EZSP version 6.7.6.0 and [NCP_USW_115K2_S1_F256_664_PB14-PB15.s37](NCP_USW_115K2_S1_F256_664_PB14-PB15.s37) its version 6.6.4.0 of EZSP = our NCP.  

If all was going well your Mighty Gecko its transformed in to a "Billy EZSP" and sending "hallo world" in gecko lang...  
  
Windows with Extra PTTY:
```
▒▒�~
```
And in Lubuntu with Minicom -H:
```
Welcome to minicom 2.7.1

OPTIONS: I18n                                                                
Compiled on May  3 2018, 15:20:11.                                           
Port /dev/ttyUSB1, 14:13:41                                                  
                                                                             
Press CTRL-A Z for help on special keys                                              
                                                                                     
1a c2 02 8b c2 8a 7e  
```

And you have a very happy Mighty Gecko !!!

# Big  thanks to : 
### [mx512](https://github.com/mtx512)
### [grobasoz](https://github.com/grobasoz)
### [jnicolson](https://github.com/jnicolson) 
for helping making firmware and putting all things together to a working firmware set for the "Billy EZSP" ! ! !  



## Tasmota dev with ZIGBEE_EZSP
  
File: tasmota20.bin  
Compilled with:  
  ```
#define USE_ZIGBEE
#undef USE_ZIGBEE_ZNP
#define USE_ZIGBEE_EZSP
  ```
 Dev with ZIGBEE_EZSP.   
  ```
ICC-A-1 Zigbee Bridge  
Tasmota  
Program Version	8.4.0(tasmota)  
Build Date & Time	2020-07-13T07:28:21  
Core/SDK Version	2_7_2/2.2.2-dev(38a443e)  
  ```
  
Use Template:   
``` {"NAME":"ICC-A-1 Zigbee Bridge","GPIO":[255,255,157,255,255,255,0,0,255,166,255,165,255],"FLAG":15,"BASE":18} ```  
  
  
Desable loging to UART so EZSP can using hardware UART.   
In consol: ``` "SerialLog 0" ```  
  
Reboot for hardware UART take change.  
  
Use RX GIPO 13 and TX GIPO 15 for EZSP com so USB dont interferenc with it.  .  

  ```
 07:51:45 ZIG: Resetting EZSP device
07:51:46 RSL: tele/tasmota_B3C82A/RESULT = {"ZbState":{"Status":1,"Message":"EFR32 booted","RestartReason":"Software","Code":11}}
07:51:46 ZIG: ZbEZSPSend 000008
07:51:46 ZIG: {"ZbEZSPReceived":"000008026067"}
07:51:46 RSL: tele/tasmota_B3C82A/RESULT = {"ZbState":{"Status":55,"Version":"6.7.6.0","Protocol":8,"Stack":2}}
07:51:46 RSL: tele/tasmota_B3C82A/RESULT = {"ZbState":{"Status":3,"Message":"Configured, starting coordinator"}}
07:51:46 ZIG: ZbEZSPSend 53001E0200
07:51:46 ZIG: {"ZbEZSPReceived":"530000"}
.
.
07:51:46 ZIG: ZbEZSPSend 2800
07:51:46 ZIG: {"ZbEZSPReceived":"28000001CCCCCCCCCCCCCCCC631A140B0000000000F8FF07"}
07:51:46 RSL: tele/tasmota_B3C82A/RESULT = {"ZbState":{"Status":56,"IEEEAddr":"0xCCCCCCFFFEB9B319","ShortAddr":"0x0000","DeviceType":1}}
07:51:46 RSL: tele/tasmota_B3C82A/RESULT = {"ZbState":{"Status":0,"Message":"Started"}}
07:51:46 ZIG: Zigbee started
07:51:46 ZIG: No zigbee devices data in Flash
 ```
   
 Your Billy EZSP its up and running on Tasmota dev.  
 
 ### Compilling Tasmota:
  
The easyest way is to use [Gitpod](https://tasmota.github.io/docs/Gitpod/) for compilling the bin file and downloading it and flash your ESP82xx.  

