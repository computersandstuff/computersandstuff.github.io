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
[    2.956565] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[    2.963845] JFFS2 version 2.2. (NAND) © 2001-2006 Red Hat, Inc.
[    2.971873] msgmni has been set to 263
[    2.980904] io scheduler noop registered
[    2.985010] io scheduler deadline registered
[    2.989903] io scheduler cfq registered (default)
[    2.996655] ambarella-uart.0: ttyS0 at MMIO 0x70005000 (irq = 9) is a ambuart
[    3.037582] ambarella-uart.1: ttyS1 at MMIO 0x7001f000 (irq = 25) is a ambuart
[    3.081017] brd: module loaded
[    3.100098] loop: module loaded
[    3.106869] NAND device: Manufacturer ID: 0xad, Chip ID: 0xf1 (Hynix NAND 128MiB 3,3V 8-bit)
[    3.116060] ambarella_nand_config_flash: 0x02e00140, 0x02c00140
[    3.123835] ambarella-nand ambarella-nand: ambarella_nand_probe: Partition infomation found!
[    3.132837] Creating 10 MTD partitions on "ambnand":
[    3.138153] 0x000000000000-0x000000020000 : "bst"
[    3.147201] 0x000000020000-0x000000520000 : "ptb"
[    3.156371] 0x000000520000-0x000000a20000 : "bld"
[    3.165726] 0x000000a20000-0x000000f20000 : "hal"
[    3.175062] 0x000000f20000-0x000001520000 : "pri"
[    3.184569] 0x000001520000-0x000001b20000 : "sec"
[    3.194024] 0x000001b20000-0x000004620000 : "bak"
[    3.203327] 0x000004620000-0x000004920000 : "dsp"
[    3.212699] 0x000004920000-0x000007420000 : "lnx"
[    3.222139] 0x000007420000-0x000007c20000 : "adc"
[    3.233754] UBI: attaching mtd6 to ubi0
[    3.238081] UBI: physical eraseblock size:   131072 bytes (128 KiB)
[    3.244594] UBI: logical eraseblock size:    126976 bytes
[    3.250407] UBI: smallest flash I/O unit:    2048
[    3.255299] UBI: VID header offset:          2048 (aligned 2048)
[    3.261736] UBI: data offset:                4096
[    3.469342] UBI: max. sequence number:       41883
[    3.485481] UBI: attached mtd6 to ubi0
[    3.489536] UBI: MTD device name:            "bak"
[    3.494525] UBI: MTD device size:            43 MiB
[    3.499718] UBI: number of good PEBs:        344
[    3.504532] UBI: number of bad PEBs:         0
[    3.509225] UBI: number of corrupted PEBs:   0
[    3.513841] UBI: max. allowed volumes:       128
[    3.518708] UBI: wear-leveling threshold:    4096
[    3.523607] UBI: number of internal volumes: 1
[    3.528275] UBI: number of user volumes:     1
[    3.532889] UBI: available PEBs:             0
[    3.537577] UBI: total number of reserved PEBs: 344
[    3.542649] UBI: number of PEBs reserved for bad PEB handling: 3
[    3.548941] UBI: max/mean erase counter: 417/125
[    3.553750] UBI: image sequence number:  1815888556
[    3.558909] UBI: background thread "ubi_bgt0d" started, PID 671
[    3.565305] console [netcon0] enabled
[    3.569199] netconsole: network logging started
[    3.575511] mousedev: PS/2 mouse device common for all mice
[    3.584917] using rtc device, ambarella-rtc, for alarms
[    3.590525] ambarella-rtc ambarella-rtc: rtc core: registered ambarella-rtc as rtc0
[    3.601062] ambarella-wdt ambarella-wdt: Ambarella Media Processor Watch Dog Timer[ambarella-wdt].
[    3.614741] lp5521 0-0035: lp5521 programmable led chip found
[    3.632729] ambarella-i2c ambarella-i2c.0: ambarella_i2c_stop[1] from 7 to 9
[    3.640175] ambarella-i2c ambarella-i2c.0: ambarella_i2c_irq in wrong state[0x9]
[    3.647874] ambarella-i2c ambarella-i2c.0: status_reg[0x40]
[    3.653667] ambarella-i2c ambarella-i2c.0: control_reg[0x3]
[    4.627506] ambarella-i2c ambarella-i2c.0: No ACK from address 0x66, 0:1!
[    4.634589] ambarella-i2c ambarella-i2c.0: I2C state 0x9, please check address 0x66!
[    4.663639] lp5523 0-0033: LP5523 Programmable led chip found
[    4.673821] LP5523 status:
[    4.677114] 	LP5523_REG_ENABLE: 0x6a
[    4.681406] 	LP5523_REG_CONFIG: 0x7e
[    4.685597] 	LP5523_REG_OP_MODE: 0x0
[    4.689883] 	LP5523_REG_ENABLE_LEDS_MSB: 0x0
[    4.694794] 	LP5523_REG_ENABLE_LEDS_LSB: 0x0
[    4.719716] ALSA device list:
[    4.722829]   No soundcards found.
[    4.726363] oprofile: hardware counters not available
[    4.731954] oprofile: using timer interrupt.
[    4.736686] TCP cubic registered
[    4.740237] NET: Registered protocol family 17
[    4.745009] Registering the dns_resolver key type
[    4.754221] =====RTC ever lost power=====
[    4.770835] ambarella-rtc ambarella-rtc: setting system clock to 2004-01-10 13:37:04 UTC (1073741824)
[    4.782152] UBIFS: parse sync
[    4.815963] UBIFS: recovery needed
[    4.943046] UBIFS: recovery completed
[    4.947093] UBIFS: mounted UBI device 0, volume 0, name "rootfs"
[    4.953572] UBIFS: file system size:   41394176 bytes (40424 KiB, 39 MiB, 326 LEBs)
[    4.961652] UBIFS: journal size:       9023488 bytes (8812 KiB, 8 MiB, 72 LEBs)
[    4.969324] UBIFS: media format:       w4/r0 (latest is w4/r0)
[    4.975389] UBIFS: default compressor: lzo
[    4.979704] UBIFS: reserved for root:  0 bytes (0 KiB)
[    4.986014] VFS: Mounted root (ubifs filesystem) on device 0:13.
[    4.992559] Freeing init memory: 136K
[    5.753465] UBI: attaching mtd9 to ubi1
[    5.757594] UBI: physical eraseblock size:   131072 bytes (128 KiB)
[    5.764115] UBI: logical eraseblock size:    126976 bytes
[    5.769801] UBI: smallest flash I/O unit:    2048
[    5.774700] UBI: VID header offset:          2048 (aligned 2048)
[    5.781008] UBI: data offset:                4096
[    5.824686] UBI: max. sequence number:       7859
[    5.840109] UBI: attached mtd9 to ubi1
[    5.844024] UBI: MTD device name:            "adc"
[    5.849138] UBI: MTD device size:            8 MiB
[    5.854128] UBI: number of good PEBs:        64
[    5.858956] UBI: number of bad PEBs:         0
[    5.863591] UBI: number of corrupted PEBs:   0
[    5.868296] UBI: max. allowed volumes:       128
[    5.873108] UBI: wear-leveling threshold:    4096
[    5.878066] UBI: number of internal volumes: 1
[    5.882691] UBI: number of user volumes:     1
[    5.887298] UBI: available PEBs:             0
[    5.891975] UBI: total number of reserved PEBs: 64
[    5.896959] UBI: number of PEBs reserved for bad PEB handling: 2
[    5.903268] UBI: max/mean erase counter: 139/123
[    5.908125] UBI: image sequence number:  432526639
[    5.913134] UBI: background thread "ubi_bgt1d" started, PID 739
UBI device number 1, total 64 LEBs (8126464 bytes, 7.8 MiB), available 0 LEBs (0 bytes), LEB size 126976 bytes (124.0 KiB)
umount: can't umount /mnt/dropcam: Invalid argument
[    6.019277] UBIFS: recovery needed
[    6.055795] UBIFS: recovery completed
[    6.059948] UBIFS: mounted UBI device 1, volume 0, name "adc"
[    6.066039] UBIFS: file system size:   6094848 bytes (5952 KiB, 5 MiB, 48 LEBs)
[    6.073755] UBIFS: journal size:       1396736 bytes (1364 KiB, 1 MiB, 11 LEBs)
[    6.081417] UBIFS: media format:       w4/r0 (latest is w4/r0)
[    6.087534] UBIFS: default compressor: lzo
[    6.091803] UBIFS: reserved for root:  0 bytes (0 KiB)
rm: can't remove '/root/*.pid': No such file or directory
rm: can't remove '/root/*.log': No such file or directory
Populating dropcam using udev: Done
[    7.491834] Loading modules backported from Linux version v3.14-0-g455c6fd
[    7.499098] Backport generated by backports.git v3.14-1-0-g369d49a
[    7.761917] cfg80211: Calling CRDA to update world regulatory domain
[    7.779462] cfg80211: World regulatory domain updated:
[    7.787248] cfg80211:     (start_freq - end_freq @ bandwidth), (max_antenna_gain, max_eirp)
[    7.805488] cfg80211:     (2402000 KHz - 2472000 KHz @ 40000 KHz), (300 mBi, 2000 mBm)
[    7.814475] cfg80211:     (2457000 KHz - 2482000 KHz @ 20000 KHz), (300 mBi, 2000 mBm)
[    7.822920] cfg80211:     (2474000 KHz - 2494000 KHz @ 20000 KHz), (300 mBi, 2000 mBm)
[    7.831883] cfg80211:     (5140000 KHz - 5360000 KHz @ 40000 KHz), (N/A, 3000 mBm)
[    7.839938] cfg80211:     (5460000 KHz - 5860000 KHz @ 40000 KHz), (N/A, 3000 mBm)
[    8.191129] Switching to clocksource jiffies
[    8.191129] Switching to clocksource ambarella-cs-timer
[    7.591005] i2c /dev entries driver
[    7.656341] cfg80211: World regulatory domain updated:
[    7.661617] cfg80211:     (start_freq - end_freq @ bandwidth), (max_antenna_gain, max_eirp)
[    7.677617] cfg80211:     (2402000 KHz - 2472000 KHz @ 40000 KHz), (300 mBi, 2000 mBm)
[    7.685958] cfg80211:     (2457000 KHz - 2482000 KHz @ 40000 KHz), (300 mBi, 2000 mBm)
[    7.694319] cfg80211:     (2474000 KHz - 2494000 KHz @ 20000 KHz), (300 mBi, 2000 mBm)
[    7.702535] cfg80211:     (5170000 KHz - 5250000 KHz @ 40000 KHz), (300 mBi, 2000 mBm)
[    7.711419] cfg80211:     (5735000 KHz - 5835000 KHz @ 40000 KHz), (300 mBi, 2000 mBm)
[    7.746523] ambarella-spi ambarella-spi.0: ambarella SPI Controller 0 created 
[    7.763927] ambarella-spi ambarella-spi.1: ambarella SPI Controller 1 created 
[    8.125771] wm8974_amb.c :: __init wm8974_modinit(void)
[    8.131150] 
[    8.131162] wm8974 i2c add +
[    8.139543] 
[    8.139567] wm8974 i2c add -
[    8.144135] 
[    8.144156] wm8974 init done
[    8.188739] wm8974 audio codec is provided by amba Wu Dongge
[    8.214501] asoc: wm8974-hifi <-> ambarella-i2s.0 mapping ok
[    8.334859] input: AmbInput as /devices/virtual/input/input0
[    8.350752] ambarella_gpio_irq_set_wake: irq[99] = girq[35] = 1
[    8.372971] ambarella-input ambarella-input: AmbInput probed!
[    8.502860] ambarella_udc: version 2 August 2011
[    8.542229] mmc1: queuing unknown CIS tuple 0x01 (3 bytes)
[    8.573360] ambarella-crypto ambarella-crypto: Ambarella Media Processor Cryptography Engine probed(polling mode).
[    8.598157] mmc1: queuing unknown CIS tuple 0x1a (5 bytes)
[    8.621913] mmc1: queuing unknown CIS tuple 0x1b (8 bytes)
[    8.643483] mmc1: queuing unknown CIS tuple 0x14 (0 bytes)
[    8.671185] mmc1:attach sdio array 1 
Regdomain US
[    8.705683] mmc1: queuing unknown CIS tuple 0x80 (1 bytes)
[    8.723757] mmc1: queuing unknown CIS tuple 0x81 (1 bytes)
[    8.737312] mmc1: queuing unknown CIS tuple 0x82 (1 bytes)
[    8.748077] mmc1: new high speed SDIO card at address 0001
[    8.958183] MAC from EEPROM 00:03:7F:05:81:91
[    8.992011] Soft MAC 64:16:66:7B:25:04
[    9.027512] ath6kl: temporary war to avoid sdio crc error
[    9.542472] ath6kl: ar6003 hw 2.1.1 sdio fw 3.4.0.225.SMARTPHONE api 4
[    9.553459] ath6kl: Current ath6kl driver version is: 3.4.0.225
[    9.789738] cfg80211: Calling CRDA for country: US
[    9.804106] cfg80211: Regulatory domain changed to country: US
[    9.823179] cfg80211:     (start_freq - end_freq @ bandwidth), (max_antenna_gain, max_eirp)
[    9.852696] cfg80211:     (2402000 KHz - 2472000 KHz @ 40000 KHz), (300 mBi, 2700 mBm)
[    9.874153] cfg80211:     (5170000 KHz - 5250000 KHz @ 40000 KHz), (300 mBi, 1700 mBm)
Initializing random number generator... [    9.892938] cfg80211:     (5250000 KHz - 5330000 KHz @ 40000 KHz), (300 mBi, 2000 mBm)
[    9.922885] cfg80211:     (5490000 KHz - 5600000 KHz @ 40000 KHz), (300 mBi, 2000 mBm)
[    9.931030] cfg80211:     (5650000 KHz - 5710000 KHz @ 40000 KHz), (300 mBi, 2000 mBm)
[    9.962920] cfg80211:     (5735000 KHz - 5835000 KHz @ 40000 KHz), (300 mBi, 3000 mBm)
done.
Starting system message bus: done
```
