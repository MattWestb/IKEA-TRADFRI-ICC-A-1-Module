# Flashing the ICC-1 Module  
  
Then not having any JLink: €ß$ [Basilfix](https://github.com/basilfx/TRADFRI-Hacking#pinout) insted using a ESP8266 or SMT32 as SWD / jtag probe.  
With BlackMagic Probe (BMP) or 
with  Blue Pill as a BMP (STM32F103 board as SWD / jtag probe): [ZW](https://github.com/zw/TRADFRI-Hacking/tree/master/hacks/L1527).  
With ESP8266 as a BMP: [BlackMagic-espidf](https://github.com/MattWestb/blackmagic-espidf).  
(BMP: if problem connecting to the device or writing to fals add one around 100 Ohm restistor in serie with SWLK). 

## Wundows:
Open a cmd or PowerShell.
Move to a directory with wrigth promission (Your Documents).

Starting [arm-none-eabi-gdb](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads).

```
arm-none-eabi-gdb
(gdb) target extended-remote COM2 (For ports >= COM10 use "target extended-remote \\.\COM10")  
Remote debugging using COM2  
...........  
(gdb)  
```

## Lubuntu:

Starting [gdb-multiarch](https://packages.ubuntu.com/focal/gdb-multiarch)  
```
gdb-multiarch
(gdb) target extended-remote /dev/ttyACM0  
Remote debugging using /dev/ttyACM0  
...........  
(gdb)  
```

### Windows / Lubuntu with [BlackMagic-espidf](https://github.com/MattWestb/blackmagic-espidf):  
```
(gdb) target extended-remote 192.168.4.1:2022  
Remote debugging using 192.168.4.1:2022  
...........  
(gdb)  
```

## Conecting Mighty Gecko:  
```
(gdb) mon sw  
Available Targets:  
No. Att Driver  
 1      EFR32MG1P 132 F256 Mighty Gecko M3/M4  

(gdb) att 1  
Attaching to Remote target  
warning: No executable has been specified and target does not support  
determining executable automatically.  Try using the "file" command.  
0x00007186 in ?? ()  
  
(gdb) mon efm_info  
DI version 1 (silabs remix?) base 0x0fe08000  

EFR32MG1P 132 F256 = Mighty Gecko 256kiB flash, 32kiB ram  
Device says flash page size is 2048 bytes, we're using 2048 bytes  
  
Radio si0  
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
```

Reg 0 = Flash (256K)  
Reg 1 = Userdata (2K)  
Reg 2 = Lockbits (10K)  
Reg 3 = Ram (32K)  


## Dumping to file:
```
(gdb) dump memory XYZ18.bin 0x0fe10000 0x0fe12800  
(gdb) dump memory XYZ19.bin 0x0fe00000 0x0fe00800  
(gdb) dump memory XYZ20.bin 0x20000000 0x20008000  
  ```

## Erase flash:  
```
(gdb) mon erase_mass  
Erase successful!  
```
After erase_mass you must writing a bootloader to the flash as elf file @0x0 for the first bootloader (or one dumped with both bootloder @0x0) and @0x800 for the main bottloader or one s37 file (s37 have all meta inside).  
If not writing anny app @0x4000 or the app its not flaged OK next boot is stoping in the main bootloader.  
  
  
## Write to flash: 

### Bin files: 

Coverting bin file to elf in a terminal.
[arm-none-eabi-objcopy](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)
```
arm-none-eabi-objcopy --input-target binary --output-target elf32-little XYZ18.bin XYZ18.elf  

(gdb) load XYZ18.elf 0x0 ([FILE] [OFFSET])  
Loading section .data, size 0x40000 lma 0x0  
Start address 0x0, load size 262144  
Transfer rate: 24 KB/sec, 985 bytes/write.  
  ```
  
### .s37 files:  
  ```
(gdb) load ncp-uart-sw.s37  
Loading section .sec1, size 0xac lma 0x4000  
Loading section .sec2, size 0x30300 lma 0x4100  
Start address 0x41c8, load size 197548  
Transfer rate: 23 KB/sec, 968 bytes/write.  
```

## Bootloader app flashing:  

Apps @0x4000 can also being flashed from the main bootloader in elf file format.  
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
Then uploade its finish and CRC its OK you getting a message and can reboot your Mighty Gecko.  
  

