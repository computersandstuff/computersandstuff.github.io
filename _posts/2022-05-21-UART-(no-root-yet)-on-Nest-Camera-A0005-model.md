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
=~=~=~=~=~=~=~=~=~=~=~= PuTTY log 2022.05.20 18:28:20 =~=~=~=~=~=~=~=~=~=~=~=

[    0.000000] Linux version 2.6.38.8 (jenkins@dropcam-build.local) (gcc version 4.5.2 (Sourcery G++ Lite 2011.03-41) ) #81 PREEMPT Tue Apr 2 18:49:38 UTC 2019
[    0.000000] CPU: ARMv6-compatible processor [4117b365] revision 5 (ARMv6TEJ), cr=00c5387f
[    0.000000] CPU: VIPT nonaliasing data cache, VIPT nonaliasing instruction cache
[    0.000000] Machine: Coconut
[    0.000000] Memory policy: ECC disabled, Data cache writeback
[    0.000000] Ambarella: AHB	= 0x60000000[0xf0000000],0x01000000 0
[    0.000000] Ambarella: APB	= 0x70000000[0xf1000000],0x01000000 0
[    0.000000] Ambarella: PPM	= 0xc0000000[0xe0000000],0x00200000 9
[    0.000000] Ambarella: BSB	= 0xc8c00000[0xe8c00000],0x00400000 9
[    0.000000] Ambarella: DSP	= 0xc9000000[0xe9000000],0x07000000 9
[    0.000000] Ambarella: HAL	= 0xc00a0000[0xfee00000],0x0000e708 9
[    0.000000] bootmem_init: high_memory = 0xc8a00000
[    0.000000] Built 1 zonelists in Zone order, mobility grouping on.  Total pages: 35052
[    0.000000] Kernel command line: console=ttyS0 ubi.mtd=bak root=ubi0:rootfs rw rootfstype=ubifs rootflags=sync init=/linuxrc hw_rev_adc_val=
[    0.000000] PID hash table entries: 1024 (order: 0, 4096 bytes)
[    0.000000] Dentry cache hash table entries: 32768 (order: 5, 131072 bytes)
[    0.000000] Inode-cache hash table entries: 16384 (order: 4, 65536 bytes)
[    0.000000] Memory: 138MB = 138MB total
[    0.000000] Memory: 135156k/135156k available, 6156k reserved, 0K highmem
[    0.000000] Virtual kernel memory layout:
[    0.000000]     vector  : 0xffff0000 - 0xffff1000   (   4 kB)
[    0.000000]     fixmap  : 0xfff00000 - 0xfffe0000   ( 896 kB)
[    0.000000]     DMA     : 0xfe600000 - 0xfee00000   (   8 MB)
[    0.000000]     vmalloc : 0xc9000000 - 0xe0000000   ( 368 MB)
[    0.000000]     lowmem  : 0xc0000000 - 0xc8a00000   ( 138 MB)
[    0.000000]     modules : 0xbf000000 - 0xc0000000   (  16 MB)
[    0.000000]       .init : 0xc0008000 - 0xc002a000   ( 136 kB)
[    0.000000]       .text : 0xc002a000 - 0xc045e000   (4304 kB)
[    0.000000]       .data : 0xc045e000 - 0xc048e418   ( 194 kB)
[    0.000000] Preemptable hierarchical RCU implementation.
[    0.000000] 	RCU-based detection of stalled CPUs is disabled.
[    0.000000] 	Verbose stalled-CPUs detection is disabled.
[    0.000000] NR_IRQS:224
[    0.000000] sched_clock: 32 bits at 60MHz, resolution 16ns, wraps every 71582ms
[    0.000000] Console: colour dummy device 80x30
[    0.000000] console [ttyS0] enabled
[    0.227682] Calibrating delay loop... 478.41 BogoMIPS (lpj=2392064)
[    0.482911] pid_max: default: 32768 minimum: 301
[    0.488131] Mount-cache hash table entries: 512
[    0.493703] CPU: Testing write buffer coherency: ok
[    0.506690] Enabling TPS3813 HW WDT
[    0.511239] NET: Registered protocol family 16
[    0.522753] Ambarella Dropcam:
[    0.525959] 	chip id:		5100
[    0.528864] 	board type:		4
[    0.532067] 	board revision:		3
[    0.535341] 	chip name:		a5m
[    0.538344] 	HAL version:		176869
[    0.541947] 	reference clock:	24000000
[    0.545856] 	system configuration:	0x0400046a
[    0.550487] 	boot type:		0x00000002
[    0.554122] 	hif type:		0x00000000
[    0.560748] Initializing Quartz Dropcam board (revision 3)
[    2.648489] bio: create slab <bio-0> at 0
[    2.663507] ambarella-i2c ambarella-i2c.0: Ambarella Media Processor I2C adapter[i2c-0] probed!
[    2.674468] ambarella-i2c ambarella-i2c.1: Ambarella Media Processor I2C adapter[i2c-1] probed!
[    2.685221] i2c i2c-0: Added multiplexed i2c bus 2
[    2.690495] ambarella-i2cmux ambarella-i2cmux.0: mux on ambarella-i2c adapter
[    2.701653] Advanced Linux Sound Architecture Driver Version 1.0.23.
[    2.711906] Switching to clocksource ambarella-cs-timer
[    2.720150] Switched to NOHz mode on CPU #0
[    2.789905] ambarella-sd ambarella-sd.0: Slot0 use bounce buffer[0xc8960000<->0xc8b60000]
[    2.798717] ambarella-sd ambarella-sd.0: Slot0 req_size=0x00020000, segs=32, seg_size=0x00020000
[    2.808224] ambarella-sd ambarella-sd.0: Slot1 use bounce buffer[0xc8800000<->0xc8a00000]
[    2.816741] ambarella-sd ambarella-sd.0: Slot1 req_size=0x00020000, segs=32, seg_size=0x00020000
[    2.857569] ambarella-sd ambarella-sd.0: Ambarella SD/MMC[0] has 2 slots @ 25263157Hz, [0x09e130b0:0x00000000]
[    2.868939] NET: Registered protocol family 2
[    2.873870] IP route cache hash table entries: 2048 (order: 1, 8192 bytes)
[    2.882215] TCP established hash table entries: 8192 (order: 4, 65536 bytes)
[    2.890034] TCP bind hash table entries: 8192 (order: 3, 32768 bytes)
[    2.896932] TCP: Hash tables configured (established 8192 bind 8192)
[    2.903639] TCP reno registered
[    2.906937] UDP hash table entries: 256 (order: 0, 4096 bytes)
[    2.913120] UDP-Lite hash table entries: 256 (order: 0, 4096 bytes)
[    2.920342] NET: Registered protocol family 1
[    2.925684] RPC: Registered udp transport module.
[    2.930747] RPC: Registered tcp transport module.
[    2.935651] RPC: Registered tcp NFSv4.1 backchannel transport module.
```
