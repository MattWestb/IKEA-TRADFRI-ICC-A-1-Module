# Flashing the ICC-1 Module  
  
Then i not having any JLink: €ß$ as [Basilfix](https://github.com/basilfx/TRADFRI-Hacking#pinout), so insted i using a ESP8266 or SMT32 as SWD / jtag probe and flashing with GDB.  
With BlackMagic Probe (BMP) or with  Blue Pill as a BMP (STM32F103 board as SWD / jtag probe): [ZW](https://github.com/zw/TRADFRI-Hacking/tree/master/hacks/L1527).  
With ESP8266 as a BMP: [BlackMagic-espidf](https://github.com/MattWestb/blackmagic-espidf).  
(BMP: if having problem connecting to the target or writing to target fails add ~130 Ohm restistor in serie with SWD/Jtag pins). 

## Connecting the probe in Windows:
Open a cmd or PowerShell.  
Move to a directory with wrigth promission (Your Documents)  
Starting [arm-none-eabi-gdb](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads).   
In GDB shell: target extended-remote "Your comport" (For ports >= COM10 use "target extended-remote \\.\COM10")  

```
arm-none-eabi-gdb
     
(gdb) target extended-remote COM2  
Remote debugging using COM2  
...........  
(gdb)  
```

## Connecting the probe in Lubuntu:

Starting [gdb-multiarch](https://packages.ubuntu.com/focal/gdb-multiarch)  
In GDB shell: target extended-remote "Your TTY dev"  
```
gdb-multiarch
    
(gdb) target extended-remote /dev/ttyACM0  
Remote debugging using /dev/ttyACM0  
...........  
(gdb)  
```

### Connecting the [BlackMagic-espidf](https://github.com/MattWestb/blackmagic-espidf) probe in Windows / Lubuntu:  
In GDB shell: target extended-remote "Your target ip:2022"
```
  
(gdb) target extended-remote 192.168.4.1:2022  
Remote debugging using 192.168.4.1:2022  
...........  
(gdb)  
```

## Conecting the probe with our Mighty Gecko:  
Scan for SWD targets. Attatch target. Show info of your Gecko. 
```
   
(gdb) mon sw
Target voltage: 2984mV
Available Targets:
No. Att Driver
 1      EFR32MG1P 132 F256 Mighty Gecko M3/M4
(gdb) 
  
  
(gdb) att 1
Attaching to Remote target
warning: No executable has been specified and target does not support
determining executable automatically.  Try using the "file" command.
0x0000234a in ?? ()
(gdb) 
  
  
(gdb) mon efm_info  
DI version 1 (silabs remix?) base 0x0fe08000  

EFR32MG1P 132 F256 = Mighty Gecko 256kiB flash, 32kiB ram  
Device says flash page size is 2048 bytes, we're using 2048 bytes  
  
Radio si0  
(gdb)  
```  

## Mem regions:
```
  
(gdb) info mem  
Using memory regions provided by the target.  
Num Enb Low Addr   High Addr  Attrs  
0   y   0x00000000 0x00040000 flash blocksize 0x800 nocache  
1   y   0x0fe00000 0x0fe00800 flash blocksize 0x800 nocache  
2   y   0x0fe10000 0x0fe12800 flash blocksize 0x800 nocache  
3   y   0x20000000 0x20008000 rw nocache  
(gdb)  
```

Reg 0 = Internal flash (256K) (First and main bootloader, app and emulated EEPROM).    
Reg 1 = Userdata (2K) (Its an separate region of internal flash ouside the normal Reg 0 that normally not being erased with erase_mass).  
Reg 2 = Bootloader (10K) (Factory bootloader ??)  
Reg 3 = Ram (32K)  

For reference, Readout with J-Link and Simplicity Commander:   
  
[<img src="SC01.PNG" alt="SimplicityCommander" width="512">](E1743.jpg)
  
## Dumping internal flash / memory to file:
```
   
(gdb) dump memory XYZ17.bin 0x00000000 0x00040000   
(gdb) dump memory XYZ18.bin 0x0fe00000 0x0fe00800  
(gdb) dump memory XYZ19.bin 0x0fe10000 0x0fe12800  
(gdb) dump memory XYZ20.bin 0x20000000 0x20008000  
(gdb)  
 ```

## Erase internal flash:  
```
  
(gdb) mon erase_mass  
Erase successful!  
(gdb)  
```

Now we have a SoC with empty internal flash.  

After erase_mass you must writing the bootloaders to the flash. The first stage bootloader: as elf file @0x0 or s37 file. And secund stage (Main) bootloader: as elf @0x0800 or s37 file. Or both first and secund (main) bootloaders: one dumped elf file with both bootloader @0x0 or one s37 file with both bootloader (s37 files have all meta inside).  
If not writing anny app @0x04000 or app image upload / flash fails, the app isn’t valid (wrong CRC) and next boot is stoping in the main bootloader.  
  
 
## Write to internal flash: 

### Bin files: 

Coverting bin file to elf in a terminal with [arm-none-eabi-objcopy](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads).
```
arm-none-eabi-objcopy --input-target binary --output-target elf32-little XYZ17.bin XYZ17.elf  
```
Then flashing with GDB: load [FILE] [OFFSET]  
```
  
(gdb) load XYZ17.elf 0x0    
Loading section .data, size 0x40000 lma 0x0
Start address 0x0, load size 262144
Transfer rate: 15 KB/sec, 985 bytes/write. 
(gdb)  
 ```
 
 Now we have writing back the compleete dumped internal flash to the SoC.  
 
 
### s37 files:  
  ```
    
(gdb) load icc-a-1-bootloader-combined.s37
Loading section .sec1, size 0x59c lma 0x0
Loading section .sec2, size 0x3210 lma 0x800
Start address 0x0, load size 14252
Transfer rate: 11 KB/sec, 890 bytes/write.  
(gdb)  
```

Now we have updated our gecko witha a new first and main bootloader.  

## App flashing:  

Apps can being flashed as elf @0x04000 or s37 files with GDB.  
It can also being flashed from the main bootloader in gbl file format.  
If trigging bootloder pin on boot you see this in the terminal:  
  ```
Welcome to minicom 2.7.1                                                               
                                                                                       
OPTIONS: I18n                                                                          
Compiled on May  3 2018, 15:20:11.                                                     
Port /dev/ttyUSB0, 20:08:16                                                            
                                                                                       
Press CTRL-A Z for help on special keys                                                
                                                                                       
                                                                                       
Gecko Bootloader v1.A.3                                                                
1. upload gbl                                                                          
2. run                                                                                 
3. ebl info                                                                            
BL >                           
  ```
Then press 1.  
```
begin upload                                                                            
CCCC  
```
Bootloader sending "C" evry secund and waiting for a xmodem upload.  
```
Serial upload complete  
BL >   
```
Then the uploade its finish and if CRC its OK you getting a message and can reboot your very HAPPY Mighty Gecko.  

[Firmware files](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Module/tree/master/Firmware) for more info of NCP firmware for our Mighty Gecko.  

# BRICK WARING !!

#### If erasing the internal flash you must flashing both the first stage and secund stage (Main bootloader) or your gecko dont boot at all or making cracy GECKO things !
#### I have booting to bootloader after erasing internal flash and flashing botloader and reading it back and the first 0x0800 wos only ff = empty, and from the bootloader flashing one app and it wos writing the app at wrong ofset and coruppted the perfials and lockbits aria = Hard brick !!!

If you is having on Silabs STK you can unbricking with Siply commander but you need having the reset pin conected to the dev kit and the debug selected to "out".   
"Normal" J-Link adapters dont have the script for halting the CPU after teriggering on reset so they is nott working.   
Click on "recover bricked devic" and then uppload one cobined boot loader to the chip.   
I can being you need putting the chip type "EFR32MG1P132F256IM32" in the "device" for getting the unbrick to working.  
In CLI is more likely you is getting it working then putting the SWO sprrd down like this:
```
PS D:\SSv5\developer\adapter_packs\commander> .\commander.exe  device lock --debug disable --speed 100 -d efr32mg1p
Setting debug interface speed to 100 kHz
Unlocking debug access (triggers a mass erase)...
Unlocking debug access (triggers a mass erase)...
Attempting EFR32/EFM32 Series 1 recovery using system bus stall...
Attempting connection while holding device in reset...
Waiting for AAP mass erase to complete...
Verifying debug access...
Found DP ID:  0x2ba01477
Found AAP ID: 0x24770011
Chip successfully unlocked.
DONE
PS D:\SSv5\developer\adapter_packs\commander> .\commander.exe  device masserase  -d efr32mg1p
Reconfiguring debug connection with detected device part number: EFR32MG1P132F256IM32
Erasing chip...
Flash was erased successfully
DONE
```

