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
 
 
### For flashing your module look at [Flashing-MG:](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Module/tree/master/Flashing-MG)  
   
