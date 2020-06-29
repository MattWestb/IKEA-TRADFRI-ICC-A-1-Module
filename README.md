# IKEA TRÅDFRI ICC-A-1 Module.

### Loading Silicon Labs EmberZNet Zigbee coordinator firmware on IKEA TRÅDFRI ICC-A-1 Module.

[s-hadinger](https://github.com/s-hadinger) have the EZSP upp and running on [Sonoff Zigbee Bridge](https://github.com/arendst/Tasmota/issues/8583) with help of [mtx512](https://github.com/mtx512)
our maker of bootoader and coordinator firmware.
If all going well sud it woring with [HA and OpenHab](https://sprut.ai/client/article/2583).  
[MPM1107](https://github.com/MPM1107) like to implenting it for Z2M. If hi and the other in Z2M team making it working then it being a graet thing in the HA comunity.


### ICC-1 / ICC-A-1 Module:

* Microcontroller: EFR32MG1P132F256GM32
* 2 Mbit SPI NOR Flash: IS25LQ020B
* Crystal: 38.4 MHz

#### For more info: [Teardown ICC-A-1](teardowns/ICC-A-1).


### For EZSP use :

| Pad | EFR32 pins | Description |
|------------|-----------|-------|
| 02         | PA15      | TX * |
| 03         | PA14      | RX * |
| 04         | PC10      | TX * |
| 05         | PC11      | RX * |
| 06         | PF0       | SWCLK |
| 07         | PF1       | SWDIO |
| 08         | PF2       | SWO   |
| 11         | VDD       | 3.3V | 
| 12         | GND       | GND |
| 16         | PA0       | Force bootloader (LL) buttom | 

1* Alternative test pins for RX and TX.

### Flashing:

[Flashing ICC-1](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Modul/tree/master/Flashing-MG).  


### Firmware:

Dumped one new (never pared) LED1837R5 with BlackMagic-espidf.  

Bootloader: First stage bootloader its OK. Also updated to latest version.  
Seccund stage bootloader updated with mtx512 and can force bootloader boot and loading app.  

[mtx512](https://github.com/mtx512) have compilles a inital [firmware set](https://github.com/mtx512/efr32/tree/master/icc-a-1).  

Then having the new bootloader in place it its only booting in bootloader mode thrue pressing the LigthingLink button (if using a controll device)(PA0 low) and poweron and then upload the EZSP from the bootloader over serial. 

Problem with NCP app loaded but crashing. Can being that 6.7.6.x NCP its too large for the MG with 256K flash or its problem with the port for RX and TX.


#### Zigbee EmberZNet: ver 6.7.6.0: 

[Release Notes](https://www.silabs.com/documents/public/release-notes/emberznet-release-notes-6.7.6.0.pdf) 2.2 API Changes: EZSP Protocol Version 8 and Secure EZSP Protocol Version 2. (Changed in release 6.7.0.0). Both EZSP and Secure EZSP have adopted a new frame format with the following changes: (1) the fields of "Frame Control" and "Frame ID" are now two bytes; (2) no longer use "Legacy Frame ID"; (3) consume two bits of "Frame Control" to indicate the frame format version
which is version 1 now. 

Disabling fallback to older prottokoll making its a braking thing for zigpy / bellows thats making the 6.7.x.x and newer relase not working with HA and OpenHab.  

Zigpy have starting looking getting v8 support and also some Zigbee2Mqtt fans are trying implenting EZSP in Z2M. 


#### Zigbee EmberZNet: ver 6.6.4.0:

[Release Notes](https://www.silabs.com/documents/public/release-notes/emberznet-release-notes-6.6.4.0.pdf) Laterst version with EZSP Protocol Version 7 and Secure EZSP Protocol Version 1. 

Its backword compatible with the old EZSP protocol (v 4/5) used in zigpy / bellows and making it working in with HA and OpenHab.

