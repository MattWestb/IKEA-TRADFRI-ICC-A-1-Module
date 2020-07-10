# Firmware files:

### Gecko Bootloaders:
The ICC-A-1 module have an Gecko Application Bootloader installed that only works with OTA files downloaded thrue Zigbee and stored on the external flash.  
  
So first we need to flash one new bootloader in the internal flash for loading the NCP app in the internal flash.  
  
Flashing new bootloader to the gecko must being done with J-tag or SWD and using elf (not recomended) or s37 files.    
  
Its normaly OK flashing without erasing the internal flash, then you can flash the "icc-a-1-bootloader.s37" that contain only the secund stage (Main) bootloader.  
If erasing the internal flash you must flashing the "icc-a-1-bootloader-combined.s37" that contains both the first stage and secund stage (Main) bootloader, or your gecko dont boot at all or making cracy GECKO things !  
I have booting to bootloader after erasing internal and flashing botloader and reading it back and the first 0x0800 wos only ff = empty,  and from the bootloader flashing one app and it wos written it ofset and coruppted the perfials and lockbits aria = Hard brick !!!  

### Gecko APP = our NCP:
NCP app can being flashed from main bootloader with gbl files or with  J-tag / SWD with elf file @0x04000 (not recomended) or s37 file.  
My current used NCP [gbl](https://github.com/grobasoz/zigbee-firmware/blob/master/NCP_USW_MG1P132F256-115k2-V676.ebl) file its not accsepted bye the bootloader but the [s37](https://github.com/grobasoz/zigbee-firmware/blob/master/NCP_USW_MG1P132F256-115k2-V676.s37) version flashed over SWD works OK.  

If all was going well your Mighty Gecko its sending "hallo world" in gecko lang...  
Windows with Extra PTTY:
```
▒▒�~
```
And in Lubuntu with Minicom:
```
Welcome to minicom 2.7.1

OPTIONS: I18n                                                                
Compiled on May  3 2018, 15:20:11.                                           
Port /dev/ttyUSB1, 14:13:41                                                  
                                                                             
Press CTRL-A Z for help on special keys                                              
                                                                                     
11 1a c2 02 8b c2 8a 7e  
```

And you have a very happy Mighty Gecko !!!

### Big  thanks to : mx512, grobasoz and jnicolson for helping putting working firmware together.  

