# IKEA TRÅDFRI Billy EZSP.

### Loading Silicon Labs EmberZNet Zigbee coordinator firmware on IKEA TRÅDFRI ICC-A-1 Module and transforming it in to a "Ikea Billy EZSP".

[s-hadinger](https://github.com/s-hadinger) have the EZSP upp and running on [Sonoff Zigbee Bridge](https://github.com/arendst/Tasmota/issues/8583) with help of [mtx512](https://github.com/mtx512)
one of our maker of bootoader and NCP firmware.
If all going well sud it working with [HA and OpenHab](https://sprut.ai/client/article/2583).  
[MPM1107](https://github.com/MPM1107) like to implenting it for Z2M. If hi and the other in Z2M team making it working then it being a graet thing in the HA comunity.  
For the moment is [HA](HA) working with EZSP version 6.6.4.0.


### ICC-1 / ICC-A-1 Module:

* Microcontroller: EFR32MG1P132F256GM32
* 2 Mbit SPI NOR Flash: IS25LQ020B
* Crystal: 38.4 MHz

#### For more info: [Teardown ICC-A-1](teardowns/ICC-A-1).


### For EZSP use :

| Pad | EFR32 pins | Description |
|------------|-----------|-------|
| 02         | PB15      | RX |
| 03         | PB14      | TX |
| 06         | PF0       | SWCLK |
| 07         | PF1       | SWDIO |
| 08         | PF2       | SWO   |
| 10         | RESETn    | Target reset | 
| 11         | VDD       | 3.3V | 
| 12         | GND       | GND |
| 16         | PA0       | Force bootloader (LL buttom) | 

RX and TX pins is matched for the pads of E1743 and the LL buttom its force boot in bootloadre mode.

### Flashing:

[Flashing ICC-1](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Modul/tree/master/Flashing-MG).  


### Firmware:

Dumped LED1837R5, LED1836G9 and E1743 with BlackMagic-espidf. Look in the [teardowns files:](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Modul/tree/master/teardowns).   

Bootloader: First stage bootloader its OK. Also possible update to latest version.  
Secund stage (main) bootloader updated to standalone bootloader and can force bootloader boot and flashing app.  

[mtx512](https://github.com/mtx512) have compilles a inital [firmware set](https://github.com/mtx512/efr32/tree/master/icc-a-1).  
I have changing the pins for RX and TX for matching with the layout of E1743 so if using mx512s main bootloader you need using PC10 as TX and PAC11 as RX with it. The NCP app its crashing hard.  
  
[nicolson](https://github.com/jnicolson) and [grobasoz](https://github.com/grobasoz) have supplied sets with working first and main bootloader and NCP app.  
Both jnicolsons and grobasoz bootloader use RX and TX pins in a standard way (PB15 and PB14) and its woking.   

jnicolsons NCP app behaving like mx5112s = crashing hard.  
grobasozs NCP looks working well and its made in version 6.6.4.0 and 6.7.4.0.  

If updating the bootloader and flashing NCP your get a very happy Mighty Gecko being transformed to an "Ikea Billy EZPS".

For more info of current [firmware files:](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Modul/tree/master/Firmware).  

Problem with NCP app loaded but crashing:
Its a very hard one then its not possible connecting with SWD, must reboot in bootloader mode then SWD its working.  


#### Zigbee EmberZNet: ver 6.7.6.0: 

[Release Notes](https://www.silabs.com/documents/public/release-notes/emberznet-release-notes-6.7.6.0.pdf) 2.2 API Changes: EZSP Protocol Version 8 and Secure EZSP Protocol Version 2. (Changed in release 6.7.0.0). Both EZSP and Secure EZSP have adopted a new frame format with the following changes: (1) the fields of "Frame Control" and "Frame ID" are now two bytes; (2) no longer use "Legacy Frame ID"; (3) consume two bits of "Frame Control" to indicate the frame format version
which is version 1 now. 

Disabling fallback to older prottokoll making its a braking thing for zigpy / bellows thats making the 6.7.x.x and newer relase not working with HA and OpenHab.  

Zigpy have starting looking getting v8 support and also some Zigbee2Mqtt fans are trying implenting EZSP in Z2M.  
[s-hadinger](https://github.com/s-hadinger) have it upp and running on Sonoff Zigbee Bridge with Tasmota and woking to getting it working for 100%.  


#### Zigbee EmberZNet: ver 6.6.4.0:

[Release Notes](https://www.silabs.com/documents/public/release-notes/emberznet-release-notes-6.6.4.0.pdf) Laterst version with EZSP Protocol Version 7 and Secure EZSP Protocol Version 1. 

Its backword compatible with the old EZSP protocol (v 4/5) used in zigpy / bellows and making it working in with HA and OpenHab.

### Using Tasmota: 
[Tasmota](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Module/tree/master/Tasmota). 

### Using ZHA:
[ZHA](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Module/tree/master/HA). 


### More references and sources of information
- basilfx hacking IKEA TRÅDFRI light bulbs and accessories: https://github.com/basilfx/TRADFRI-Hacking
- Detailed hardware specification from RIOT OS: https://github.com/RIOT-OS/RIOT/blob/master/boards/ikea-tradfri/doc.txt
- Official manual from Silabs: https://www.silabs.com/documents/public/reference-manuals/efr32xg1-rm.pdf
- Official datasheet from Silabs: https://www.silabs.com/documents/public/data-sheets/efr32mg1-datasheet.pdf
