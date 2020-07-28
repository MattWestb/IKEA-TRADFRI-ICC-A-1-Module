### Home Assistant
ICC-A-1 is up and running on HA with EZSP ver 6.6.4.0 and 6.7.4.0.  
Use 115200 as comport speed and if you like [tasmoat as TCP UART](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Module/tree/master/Tasmota).  
Funny thing HA saying its a deCONZ coordinator (HA bug then some of my devices was in my deCONZ before).  
Current status its that its being problems with real Zigbee 3 devices its leaving the network (all ZB3 updated Ikea devices).  
The devs its digging deeper and deeper so very soon they finding the problem.  
[<img src="ICC-A-1HA6640B.png" alt="HA and ICC-A-1 EZSP v 6.6.4.0" width="512">](ICC-A-1HA6640B.png) 
[<img src="ICC-A-1HA6640.png" alt="HA and ICC-A-1 EZSP v 6.6.4.0" width="512">](ICC-A-1HA6640.png)  
Commits its made for 6.7.6.0 v8 framing and looks working the samw way as the older version and having the same problems with real Zigbee 3 devices.
