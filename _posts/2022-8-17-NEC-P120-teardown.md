---           
title: NEC P120 Teardown - What makes up a 90s cell phone?
date: 2022-08-17 21:36:31 UTC
updated: 2022-08-17 21:36:31 UTC
comments: false
categories: teardown
tags: device-specs retro-tech phones
---
Phones have come a long way from being bricks with minimal functionality to devices in which the "phone" function is not the most used. This NEC P120 is a look at the past and how you could make phone calls away from home.

![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145259_anon.png)


## Disassembly

![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145748_anon.png)
*After removing the battery there are 4 weird screws you need to remove*


![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145443_anon.png)
*I don't know what kind of screws these are, but I was able to remove them with tweezers*


![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145417_anon.png)
*After removing the top cover you can see the daughterboard for the cellular transmission*


![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145355_anon.png)
*You can remove two more of those weird screws and one phillips to remove the daughterboard*


![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145336_anon.png)
*You can now remove the front cover*


![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_144901_anon.png)
*Using the clips on the side you can remove the button pad*

## The parts

#### Main Board
(For the datasheets I got the closest I could find to the exact chip. Most are not exact)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_144929_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_144935_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_144945_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_144950_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145002_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145009_anon.png)
*[NEC TC53100CF-E678](https://pdf1.alldatasheet.com/datasheet-pdf/view/115078/TOSHIBA/TC531001.html) This seems to be the ROM*

![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145014_anon.png)
*NEC JS500008 and [Toshiba T9945](https://www.alldatasheet.com/datasheet-pdf/pdf/31161/TOSHIBA/T9947S.html)*

![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145024_anon.png)
*[NEC D75304BGF 020](https://www.datasheet4u.com/datasheet-pdf/NEC/D75304/pdf.php?id=666043)*
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145036_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145040_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145048_anon.png)

#### Cellular Daughterboard 
Unfortunately I couldn't find any info on these parts, probably due to their making being solely for NEC's cell phone industry
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145111_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145120_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145122_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145123_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145125_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145129_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145135_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145140_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145150_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145154_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145157_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145200_anon.png)

#### Case and Screws
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145436_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145443_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145502_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145315_anon.png)
![](..\..\assets\2022-8-17-NEC-P120-teardown\images\20220811_145748_anon.png)


## Potential Future Uses and Projects
This phone uses the ancient first generation phone technology and fortunately it is pretty hackable (hence the next 4 revisions in cell phone tech). They use analog signals, so tricking them into thinking that you're an official cell tower is relatively easy provided you have the right equipment.
I have found [numerous articles](https://hackaday.com/tag/1g/) on hacking together a 1g service. I don't currently have an SDR, but I will get an RTL-SDR soon, but I'll need a device with transmission capability such as LimeSDR which is unfortunately seems unavailable at the time of writing. There is also a scanner mode that I might try messing with. The scanner mode was what made these so insecure back in the day, essentially everyone's conversation was able to be listened into like a radio.
The phone itself works, but the battery has a very short charge after all these years and I don't have an antenna for it.


*References and Links*
Mobile Collectors - https://www.mobilecollectors.net/phone/5203/nec-p120
FCC ID - https://fccid.io/A98MP5A1B1-1A

Patents:
https://patents.google.com/patent/US4954951
https://patents.google.com/patent/US4942516
https://patents.google.com/patent/US4896260
https://patents.google.com/patent/US4829419
https://patents.google.com/patent/US4825364
https://patents.google.com/patent/US4686622
https://patents.google.com/patent/US4531182
https://patents.google.com/patent/US4396976
https://patents.google.com/patent/US4371923
https://patents.google.com/patent/US4121284
https://patents.google.com/patent/US4120583
https://patents.google.com/patent/US4435732
https://patents.google.com/patent/US4471385
https://patents.google.com/patent/US4672457
https://patents.google.com/patent/US4739396


