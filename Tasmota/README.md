## Tasmota Zigbee EZSP
  
ICC-A-1 Zigbee Bridge  

Grabbing on of the [dayly dev builds](https://github.com/arendst/Tasmota/tree/firmware/firmware/tasmota) with the name "tasmota-zbbridge.bin" and flashing your ESP with it and configurating WiFi.  
  
Use Template:   
``` {"NAME":"ICC-A-1 Zigbee Bridge","GPIO":[255,165,157,166,255,255,0,0,255,255,255,255,255],"FLAG":15,"BASE":18} ```  
  
  
Disable loging to UART so EZSP can using hardware UART.   
In tasmota console: ``` "SerialLog 0" ```  
  
Reboot for hardware UART take change.  
  
Use RX GIPO03 and TX GIP01 for EZSP com.  

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
 
### Tasmota as TCP UART:

Run in the tasmota console:  
``` backlog rule1 on system#boot do TCPStart 8888 endon ; rule1 1 ; template {"NAME":"ICC-A-1 TCP UART","GPIO":[255,208,157,209,255,255,0,0,255,255,255,255,255],"FLAG":15,"BASE":18} ```

Reboot and use RX GIPO03 and TX GIP01 for EZSP com.

In ZHA put ```socket://<your bridge IP>:8888``` as manualy comport and ```115200``` as port speed.
