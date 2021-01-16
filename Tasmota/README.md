## IKEA Billy Zigbee Bridge
  
IKEA Billy Zigbee Bridge with tasmota on one ESP.  

  
Its looks like early version of EZSP having bugs with seting up pull controll on secund gens EFR32 (as sonoff is) and resulting sleepig end devices like IKEA. Smartthings and Philips HUE controllers isd draining the batrries very fasat. Therfor use 6.7.8.0 or newer !!

Grabbing the [dayly dev builds](https://github.com/arendst/Tasmota/tree/firmware/firmware/tasmota) with the name "tasmota-zbbridge.bin" and flashing your ESP with it and configurating WiFi and MQTT.  

Inportant if using the primary RX and TX pins the EZSP they must being disconnected or the power for the EZSP during the flash of the ESP or it failing (USB UART, ESP UART and EZSP UART its all talking at the same time = no go) and connecting the back then doing the repower after flashing of the ESP its done.
If using the altenativ UART for the EZSP (RX GIPO 13 and TX GIPO 15) its no problem flashing the ESP thru USB but dont forgeth doing the repower after flashing its done.  
  
For normal UART use Template:   
``` {"NAME":"IKEA Billy Zigbee Bridge","GPIO":[255,165,157,166,255,255,0,0,255,255,255,255,255],"FLAG":15,"BASE":18} ```  

For alatinate UART use Templete:  
``` {"NAME":"IKEA Billy Zigbee Bridge","GPIO":[255,255,157,255,255,255,0,0,255,166,255,165,255],"FLAG":15,"BASE":18} ```  
  
  
Disable loging to UART so EZSP can using hardware UART.   
In tasmota console: ``` "SerialLog 0" ```  
  
Reboot and use RX GIPO03 and TX GIP01 for normal UART and RX GIPO13 (D7) and TX GIPO15 (D8) for alatinate UART EZSP com. 

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
   
 Your Billy EZSP its up and running on Tasmota Zigbee EZSP.  
 
 [<img src="Z2T05.PNG" alt="Tasmota and ICC-A-1 EZSP v 6.7.6.0" width="512">](Z2T05.PNG)
 
### Great thanks to [Stefan](https://github.com/s-hadinger) for making it possible for the ICC-A-1 to working with tasmota zbbridge !!
 
### IKEA Billy EZSP TCP UART
  
IKEA Billy EZSP on one ESP with tasmota as TCP UART for ZHA and other HA systems.  

Grabbing the [dayly dev builds](https://github.com/arendst/Tasmota/tree/firmware/firmware/tasmota) with the name "tasmota-zbbridge.bin" and flashing your ESP with it and configurating WiFi and MQTT.  

Inportant if using the primary RX and TX pins the EZSP must being disconnected or the power fot the EZSP during the flash of the ESP or it failing (USB UART, ESP UART and EZSP UART its all talking at the same time = no go) and connecting the back then doing the repower after flashing of the ESP its done.
If using the altenativ UART for the EZSP (RX GIPO 13 and TX GIPO 15) its no problem flashing the ESP thru USB but dont forgeth doing the repower after flashing its done.
  
For normal UART run in the tasmota console:  
``` backlog rule1 on system#boot do TCPStart 8888 endon ; rule1 1 ; template {"NAME":"IKEA Billy EZSP TCP UART","GPIO":[255,208,157,209,255,255,0,0,255,255,255,255,255],"FLAG":15,"BASE":18} ```

For alatinate UART run in the tasmota console:
``` backlog rule1 on system#boot do TCPStart 8888 endon ; rule1 1 ; template {"NAME":"IKEA Billy EZSP TCP UART","GPIO":[255,255,157,255,255,255,0,0,255,209,255,208,255],"FLAG":15,"BASE":18} ```

Disable loging to UART so EZSP can using hardware UART.   
In tasmota console: ``` "SerialLog 0" ```  
  
Reboot and use RX GIPO03 and TX GIP01 for normal UART and RX GIPO13 (D7) and TX GIPO15 (D8) for alatinate UART EZSP com.

In [ZHA](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Module/tree/master/HA) put ```socket://<your bridge IP>:8888``` as manually comport and ```115200``` as port speed.

### One more thanks to [Stefan](https://github.com/s-hadinger) !!!
