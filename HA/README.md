### Home Assistant
ICC-A-1 is up and running on HA as of 0.115.0 with EZSP ver 6.6.4.0 and 6.7.6.0.  
Use 115200 as comport speed and if you like use [tasmoat as TCP UART](https://github.com/MattWestb/IKEA-TRADFRI-ICC-A-1-Module/tree/master/Tasmota).  
Funny thing is that HA in some viues is saying its a deCONZ coordinator (HA bug then some of my devices was in my deCONZ before).  
   
[<img src="ICC-A-1HA6640C.png" alt="HA and ICC-A-1 EZSP v 6.6.4.0" width="512">](ICC-A-1HA6640C.png)  

The devs have rebuilding the bellows for supporting diferent command sets / protocoll versions for more flexibility and easyer maintaining the EZSP stacks in thr future.  
The main commit  was +10,266 −1,279 lines lines and more is comming then more testing and fintuning of the code is being done.
Its looks very prommesing and if all is going well its one deCONZ killer !!

### Large thanks to [Alexei](https://github.com/Adminiuga) ho have refactoring tones of code in zigpy / bellows for geting ZHA working.
