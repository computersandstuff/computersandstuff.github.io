---           
title: UART (no root yet) on Nest Camera A0005 model
date: 2022-05-21 18:20:31 UTC
updated: 2022-05-21 18:20:31 UTC
comments: false
categories: embedded rooting
tags: nest smart-device-hacking
---
 In my constant search of rootable devices I came across an indoor nest camera. Personally I think the cameras have good quality and nice features, but my problem with them is that they will only work through google's servers. So I cracked one open to see what I could find inside.

Make sure to check out the references and resources at the bottom of the page for a lot more information on this camera.

Hardware pics
-------------

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153106.jpg)

Main PCB

  

  

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153115.jpg)

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153132.jpg)

WiFi Chip: Qualcomm Atheros AR6233X-AM2D

And a chip that says g1 810 03003r

  

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153154.jpg)

Wolfson wm89746

  

  

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153155.jpg)

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153208.jpg)

Small chip is maybe flash?

  

  

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153237.jpg)

Bluetooth controller? Skyworks SKY66117-11

  

  

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153246.jpg)

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153252.jpg)

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153259.jpg)

  

  

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153811.jpg)

  

  

  

  

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153833.jpg)

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153851.jpg)

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153900.jpg)

  

  

  

[![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_153939.jpg)

CPU: Ambarella A5s66-CO-RH

528MHz ARM11

  

  

  

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-20220515_154029.jpg)

Ram: CHIPSIP CT49248DD486C1

  

UART
----

I tested for UART which I was successful with. Here is the location and pinout:

![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-AVvXsEhwb9kVM_LlEGIm2d3ocpHF3m)


![]({{ site.url }}/images/blogger/2022-05-21/2022-05-21-AVvXsEhPzAFGZtKcNl9oKNOfT0mt3c)
  

  
  

You may use any of the outside connectors for ground including the USB port. The UART runs at **115200 baud.** 

After the device boots you get a login prompt. I tried a few passwords, but none worked. Unfortunately the bootloader doesn't even output to the UART so you can't change any boot options. It seems that the first line outputted is about linux and not the bootloader also it has a delay after plugging in which shouldn't happen to the bootloader. The bootloader starts immediately. 

I tried different pins for another UART to no avail. If anyone could find the password or other UART to unlock the bootloader please let me know. 

Hopefully this post sheds a little light on this device's inner workings.

References and Resources
------------------------

FCC info: [https://fccid.io/ZQANC11](https://fccid.io/ZQANC11)

Teardown instructions: [https://www.ifixit.com/Device/Nest\_Cam](https://www.ifixit.com/Device/Nest_Cam)

Reversing the dropcam, great blog posts about rooting dropcams (before Nest) :

[https://blog.includesecurity.com/2014/03/reversing-the-dropcam-part-1-wireless-and-network-communications/](https://blog.includesecurity.com/2014/03/reversing-the-dropcam-part-1-wireless-and-network-communications/)

[https://blog.includesecurity.com/2014/04/reversing-the-dropcam-part-2-rooting-your-dropcam/](https://blog.includesecurity.com/2014/04/reversing-the-dropcam-part-2-rooting-your-dropcam/)

[https://blog.includesecurity.com/2014/08/reversing-the-dropcam-part-3-digging-into-complied-lua-functionality/](https://blog.includesecurity.com/2014/08/reversing-the-dropcam-part-3-digging-into-complied-lua-functionality/)

Dropcam rooting for defcon: [https://www.slideshare.net/cisoplatform7/defcon-22colbymoorepatrickwardlesynackdrop-cam](https://www.slideshare.net/cisoplatform7/defcon-22colbymoorepatrickwardlesynackdrop-cam)

  

### UART log: 

```console

```
