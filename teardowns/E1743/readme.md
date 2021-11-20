# E1743 

Hocked up and dumped !!

[<img src="E1743.jpg" alt="Back of IKEA TRÅDFRI E1743" width="512">](E1743.jpg)

## Firmware:

OTA image type:  
Data string : IKEA of Sweden, TRADFRI on/off switch, 20190723, E1743  
Version: 2.2.010  
Flash: E1743_Flash.bin  
User Data: E1743_UD.bin  

## Pinout:
| EFR32 pins | Pad | Funtion |
|-|-|-|
| PC10 | | On/Up |
| PB12 | | Off/Down |
| PA0 | | LLB |
| PC11 | | LED |
| RESETn | REST | Reset |
| VDD | VCC | VCC |
| FP1 | TMS | SWDIO |
| FP0 | CLK | SWCLK |
| GND | GND | GND |
| PB14 | TX | TX |
| PB15 | RX | RX |

The User Data E1743_UD.s37 can being used converting E1766 to one E1743 without flashing main flash and boot loader.
