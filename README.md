# IKEA TRÅDFRI Billy EZSP.

### Loading Silicon Labs EmberZNet Zigbee coordinator firmware on IKEA TRÅDFRI ICC-A-1 Module and transforming it in to a "IKEA Billy EZSP".

[s-hadinger](https://github.com/s-hadinger) have tasmota up running on [Sonoff Zigbee Bridge](https://github.com/arendst/Tasmota/issues/8583) as "tasmota zbbridge" build with help of [mtx512](https://github.com/mtx512) one of our maker of bootoader and NCP firmware with EZSP 6.7.6.0 exlusiv in V8. Have fixing the initial ZB3 network problems and moving from real alphstage to more stabile beta for testing. [Tasmota zbbridge](Tasmota) its "Ikea Billy EZSP compatible".
 
Zigpy / bellows the core of [HAs ZHA](HA) is up and running on all EZPS versions as of HA 0.115.0. The devs have commited tons of refacted code for making it more fexible and modular for V4 to V8++.   
Zigpy / bellows is working with HA and making a large part of the comunity "EZSP compatible" and it is "Ikea Billy EZSP compatible".  

[MPM1107](https://github.com/MPM1107) like to implenting it for Z2M. If hi and the other in Z2M team making it working then it being a graet thing in the HA comunity.  


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

[Flashing ICC-1](Flashing-MG).  


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

Problem with NCP app loaded but crashing:
Its a very hard one then its not possible connecting with SWD, must reboot in bootloader mode then SWD its working.  

Silabs EZSP is not code compaatible between EFR generations then they is using diferent CPUs but the NCP stack is protocoll compatible, so if using one MG1 or MG2 serie is from the host application transparant.  
For more info of current [firmware files:](Firmware).  


#### Zigbee EmberZNet: ver 6.7.6.0: 

[Release Notes](https://www.silabs.com/documents/public/release-notes/emberznet-release-notes-6.7.6.0.pdf)
Have updating the protocol and is not backwards compatible and need the host talking "version 8" or its not working.
Tasmota and ZHA (dev) have adopting v8 and is working out of box.


#### Zigbee EmberZNet: ver 6.6.4.0:

[Release Notes](https://www.silabs.com/documents/public/release-notes/emberznet-release-notes-6.6.4.0.pdf)
Its the laterst version with EZSP Protocol Version 7 and Secure EZSP Protocol Version 1.  
Its backword compatible with the old EZSP protocol (v 4/5/6) and can being used in old host systems that have hot inplanting the v8.  

## Supported platforms:

### Using Tasmota: 
[Tasmota](Tasmota). 

### Using ZHA:
[ZHA](HA). 

### Using Z2M:
Our user [MPM1107](https://github.com/MPM1107) like to implenting but dont geting much help. And [Koenk](https://github.com/Koenkk) have zero intrest and have closing the [thred in his repro](https://github.com/Koenkk/zigbee-herdsman/issues/168#event-3580175320). 

## New IKEA TRÅDFRI MARKUS EZSP module.
As of 2020.12 the new GU10 RGBW LED1923R5 is having one new Zigbee module made of Silicon Laboratories Finland Oy with model MGM210L022INF2.
Its based on EFR32MG21 famely (secund gen) with 1024/96 kB of flash/RAM memory.
Good specifications but its not known if IKEA have using the hardware security functions and locked the chip or only protecting it thru signing the firmware. No OTA have being relesed and no flash dump of the firmware and data setting.
In the new GU10 RGBW bulb is mounted on one small PCB with one buck converter on the back side.

[<img src="/teardowns/MGM210L022INF2.jpg" alt="MGM210L022INF2" width="512">](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Module/tree/master/teardowns/MGM210L022INF2.jpg)

 

### More references and sources of information
- basilfx hacking IKEA TRÅDFRI light bulbs and accessories: https://github.com/basilfx/TRADFRI-Hacking
- Detailed hardware specification from RIOT OS: https://github.com/RIOT-OS/RIOT/blob/master/boards/ikea-tradfri/doc.txt
- Official manual from Silabs: https://www.silabs.com/documents/public/reference-manuals/efr32xg1-rm.pdf
- Official datasheet from Silabs: https://www.silabs.com/documents/public/data-sheets/efr32mg1-datasheet.pdf
