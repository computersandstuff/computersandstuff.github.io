---           

title: How to fix D-LINK DIR615 (E1) Firmware (solid orange light)
date: 2020-05-13 20:00:15 UTC
updated: 2020-05-13 20:00:15 UTC
comments: false
categories: embedded howto
tags: unbricking router
---
The D-Link DIR615 Firmware problem is common when the firmware is corrupted or the router was powered off during a firmware update.

![D-Link DIR615]({{ site.url }}/images/blogger/2020-05-13/2020-05-13-image1.jpg)

  
  
  

  

Steps to fix:

1.  Download the correct firmware for your router:  
    (Safe Mega downloads)  
    DIR615 type E1: http://bit.ly/DIRFIRMWAREE1  
    DIR615 type E4: http://bit.ly/DIRFIRMWAREE4
2.  Get PC ready for sending  
    You must use an old pc/laptop for it to send. VM’s may work and I will test it in my free time. I used an old laptop circa 2003 that runs Windows XP.
3.  Set up IP  
    Go to network settings in control panel. Go to adapter settings and right click your ethernet adapter. (Note: you cannot send the file via wifi) In the properties of your adapter select Internet Protocol Version 4 or Internet Protocol in Windows XP. Select "Use the following IP Address" and type in these IP’s:  
    IP Address: 192.168.0.2  
    Subnet Mask: 255.255.255.0 (Should fill in automatically)  
    Default Gateway: 192.168.0.1  
      
    Preferred DNS server: 192.168.0.1  
    (leave alternate DNS blank)  
      
    Press OK on all the dialog boxes.
4.  Connect the router to the computer via an ethernet cable. Make sure to connect the computer to one of the LAN ports on the router and not to the INTERNET port.
5.  Unplug the router if it is not already.
6.  Get a pencil, paperclip, or anything small enough to press the reset button. While the router is off hold down the small reset button next to the power input with the pencil. While still holding down the reset button, plug in the router. Continue holding the button until the orange power light starts flashing slowly and one of the LAN indicators turns green. You can stop pressing the button now.
7.  Assuming that you did all the steps right, didn’t skip a step, and kept your computer turned on and plugged into the router you can now open a web browser. I would recommend Internet Explorer or some generic browser instead of Chrome. The older the browser (and the older the computer) the better. Type 192.168.0.1 in the address bar. If anything pops up like "No internet connection" or "oH mY gOsH yOu’Re nOt cOnNeCtEd tO tHe iNtErNeT!!!!" just click "connect" or "continue".
8.  If you did everything right then you should be looking at the DIR615 firmware recovery page. Click browse for file and select your firmware file.
9.  If you are using the correct OS, Browser, and computer then you should get a loading screen and the percentage increases. I do not believe the percentage is correct, but is just a timer making sure the user does not disconnect the computer before it is done sending the firmware. If it all worked out the router’s orange light should turn off for a little bit. Then it should turn back on as a working router!
10.  Resetting your computer. Open the adapter settings in control panel again and select the network adapter. Select your tcp/ip adapter and select automatically configure ip. Then click automatically configure DNS. Pres ok on all dialog boxes and close out. Disconnect the ethernet cable.  
      
    You should be good to go! The router should work now and you can configure it in 192.168.0.1. Please note that there are other tutorials on this, but they are old. In this one I tell you that you must use an old computer to get this to work.