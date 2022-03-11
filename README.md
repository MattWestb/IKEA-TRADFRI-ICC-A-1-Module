# IKEA TRÅDFRI Billy EZSP.

### Loading Silicon Labs EmberZNet Zigbee coordinator firmware on IKEA TRÅDFRI ICC-A-1 Module and transforming it in to a "IKEA Billy EZSP".

[s-hadinger](https://github.com/s-hadinger) have tasmota up running on [Sonoff Zigbee Bridge](https://github.com/arendst/Tasmota/issues/8583) as "tasmota zbbridge" build with help of [mtx512](https://github.com/mtx512) one of our maker of bootoader and NCP firmware with EZSP 6.7.8.0 exlusiv in V8. Have fixing the initial ZB3 network problems and moving from real alphstage to more stabile beta for testing. [Tasmota zbbridge](Tasmota) its "Ikea Billy EZSP compatible".
 
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
| 04         | PC10      | PTI Frame / VCom RX |
| 05         | PC11      | PTI Data / VCom TX |
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
grobasozs NCP looks working well and its made in version 6.6.4.0. 6.7.4.0 and 6.7.8.0. 

If updating the bootloader and flashing NCP your get a very happy Mighty Gecko being transformed to an "Ikea Billy EZPS".

Problem with NCP app loaded but crashing:
Its a very hard one then its not possible connecting with SWD, must reboot in bootloader mode then SWD its working.  

Silabs EZSP is not code compaatible between EFR generations then they is using diferent CPUs but the NCP stack is protocoll compatible, so if using one MG1 or MG2 serie is from the host application transparant.  
For more info of current [firmware files:](Firmware).  


#### Zigbee EmberZNet: ver 6.7.8.0: 

[Release Notes](https://www.silabs.com/documents/public/release-notes/emberznet-release-notes-6.7.8.0.pdf)
Have updating the EZSP protocol to version 8 and Secure EZSP Protocol Version 2 and is not backwards compatible and need the host talking "version 8" or its not working.  
Tasmota and ZHA have adopting v8 and is working out of box and its the prefured firmware for the NCP.  
  
Newer EZSP is not bringing and good for our Billy (mullti PAN and so on) but the 6.7.x.x is very likely being the last good version that is fitting in the device (with 256kb) and do not getting and R23 update from silabs.  
So likely is Silabs doing one "golden relese" befor its closing the platform in some years.  
Gary have coocking one EZSP 6.7.8.0 that is working.  

As of january 2021 the Minimum Longevity is set to December 2027 for all EFR32 first gen.


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
As of 2020.12 the new GU10 RGBW LED1923R5 is having one new Zigbee module made of Silicon Laboratories Finland Oy with model [MGM210LA22JNF2](https://fccid.io/QOQMGM210L/Internal-Photos/Internal-Photos-4211021).
Its based on EFR32MG21A020F1024IM32 (or EFR32MG21A010F1024IM32) -40 to 125 °C secund gen Mighty Gecko chip with 1024 kb flash and 96 kB RAM (Its not knownd if is a A010 or A020 = 10 or 20 dBm TX Power chip inside).  
The RF part is confuguratd as the 5-element match referece for +20 dBm power that many other modules have cutting down for saving componets / costs. Its have possibelity mounting external antenna and disabling the PCB one.   
Good specifications but its not known if IKEA have using the hardware security functions and locked the chip or only protecting it thru signing the firmware.  
No OTA have being relesed and no flash dump of the firmware and data setting.  
In the new GU10 RGBW bulb is mounted on one small PCB with one buck converter on the back side.

[<img src="/teardowns/MGM210L022INF2.jpg" alt="MGM210L022INF2" width="512">](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Module/tree/master/teardowns/MGM210L022INF2.jpg)  
  
And Silverglans have getting SWD pads so very easy looking inside the chhip then i getting time to so that.
![IMG_20210305_131028 (2)](https://user-images.githubusercontent.com/49618193/110117714-8d6bb500-7db9-11eb-964d-9d07c3edbf48.jpg)
![IMG_20210305_131105 (2)](https://user-images.githubusercontent.com/49618193/110117733-92c8ff80-7db9-11eb-9b1e-e70305ea9943.jpg)
Rsett and pairing button is on the upperside opositee from the SWD so easy finding the ports and if i remeber right is the default RX and TX un the underside of the module that looks beig unpopulated and perhaps need some fixing for working ad coordinate with one new firmware.  
I think the 16 pin IC (U1) is one buck converter only for the module 3.3V that is not so bad !!!  
The antenna design is made as Silabs reference dising with good ground plane and no metal in the RF zone.
  
I was getting the Silverglans for 15€ in the recycling corner and the last new GU10 CWS3 in Wien plus one shortcut botton and one black symfonisk so can testing the new ZHA quirks :-))  
  
  Silabs reference design pin out that i shall using for my pinout and firmware:
  ![MGM210LA22JNF2](https://user-images.githubusercontent.com/49618193/157933450-aaead64c-b6ee-47e6-8492-2d5972321959.PNG)

My pinout is the same as Billy: 
  - 1 VDD
  - 2 GND
  - 3 PC01 RX
  - 4 PC00 TX
  - 5 PC04 PTI DATA
  - 6 PC05 PTI SYNC/FRAME
  - 7 PA01 SWCLK
  - 8 PA02 SWDIO
  - 9 PA03 SWO
  - 10 RESET

Force bootloader pin not defined then not needed then using software command and then using WSTK for debricking the chip is only the resert pin needed.
 

### More references and sources of information
- basilfx hacking IKEA TRÅDFRI light bulbs and accessories: https://github.com/basilfx/TRADFRI-Hacking
- Detailed hardware specification from RIOT OS: https://github.com/RIOT-OS/RIOT/blob/master/boards/ikea-tradfri/doc.txt
- Official manual from Silabs: https://www.silabs.com/documents/public/reference-manuals/efr32xg1-rm.pdf
- Official datasheet from Silabs: https://www.silabs.com/documents/public/data-sheets/efr32mg1-datasheet.pdf

