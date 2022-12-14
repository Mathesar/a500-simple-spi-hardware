# a500-simple-spi-hardware
Simple SPI controller for the Amiga 500 with support for SD-cards and Ethernet controller. Tested on a rev. 5 Amiga 500. It might also work on other revisions and Amiga models but that has not been tested. The design is licensed under CERN OHL-S V2.0
This repository contains the hardware CAD files. For drivers look [here](https://github.com/Mathesar/a500-simple-spi-drivers).
This SPI controller is designed to be as simple as possible so that anyone can build it, hence the "simple" SPI controller.
The design uses no programmable logic chips but only jellybean 3.3V 74LVCXX logic chips and a jellybean quad analog switch.
To make it so simple compromises had to be made. The design therefore does not contain any shifting logic. This is a "bitbanging" design that can only receive or send 1 bit at a time. All the shifting is thus done by the driver software. It is not at bad as it seems though as the driver does not need to generate the SCLK signal.
The SCLK signal is generated automatically by the hardware whenever a bit is read or written. Therefore, using some optimised assembly, this controller can do about 60Kbytes/sec of raw throughput on a plain Amiga 500 with no FAST ram.

It seems I often need ~~3~~ 4 revisions to get things right :-). 

## Revision 1.0
Revision 1.0 was my first attempt. It used a 74HCT688 as the address decoder and did not set MOSI high on reads.
Therefore this version had problems with SD-cards. I fixed it by using A7 instead of D7 to write but this made writes very slow.
Also, the '688 is currently unobtainium. It seems that only simple logic chips like the '00 or '02 are available in all logic families.

## Revision 1.1
So, revision 1.1 was created that solved all above issues. This is the version you can see on the pictures. It no longer uses the 74HCT688 but a combination of OR, NOR and NAND gates.
However, even 1.1 had some minor issues. Firstly I made a small mistake in the address decoder. That one was very easy to patch though and after the patch everything functions as designed. Secondly however, the PCB was slightly too large at the top left corner causing the metal shield to no longer fit. (tested on a revision 5 Amiga 500)

## Revision 1.2
Therefore, revision 1.2 was created that fixes those final issues. I did not built it but it is basically revision 1.1 wwith a slightly resized PCB and the address decoder bug fixed. See [here](https://github.com/Mathesar/a500-simple-spi-hardware/tree/40606e6f381aa33d11fc7373534e6168bc126c18) for all the files for this rev 1.2.

**UPDATE 5-12-2022:**
The setup time on MOSI is marginal during reads. This is because of the ~DECODE signal feeding into U7A. To fix this, pin 2 of U7 should be lifted from the PCB so that it longer makes contact with the pad. This pin should then be connected to pin 3 of U7. On my system the hardware actually worked without the modification but some SD cards can get unreliable if this mod is not done

## Revision 1.3
This is the latest revision incorporating the MOSI setup fix. This version has been built and verified.

## Building hints:
This SPI controller is intended to give an Amiga 500 an SD-card (to be used as a harddisk) and ethernet connectivity. 
The ethernet module is optional and connected via a pinheader and some dupont jumper cables. The SD-card module is definately not optional and soldered directly to the interface as can be seen on the pictures. The reason for this is that the SD-card module supplies 3.3V to the logic chips (and to the optional ethernet controller) via it's onboard voltage regulator.
Pictures of the modules I used are in the schematic and in this repository. It is very important to use exactly the SD-card module as I used. 

![SD-card module](/pictures/sd_module.png) ![Ethernet module](/pictures/ethernet_module.png)

The circuit depends on the pinout and the onboard voltage regulator. The circuit also contain an old-fashioned electrolytic capacitor on the +3.3V rail. This component is also mandatory! Without it the voltage regulator on the SD-card module might become unstable.
The pinheader for the SD-card actvity LED and the ethernet module should be right angle types or straight angle types bend over. Otherwise the whole setup becomes too high when the dupont cables are mounted. With right angle pins everything fits nicely under the metal shield.
The pinheader that connects the controller to the 68000 socket on the A500 main board should be "turned" types with round pins. The thin part plugs into the Amiga, the thicker part is soldered on the SPI controller. This is very important as the wrong pins might damage your 68000 socket.
The ethernet module does not really fit internally to the Amiga 500. I have used some long dupont cables that go through to the side expansion slot and the ethernet module is thus hanging outside. Maybe I build something like [this](https://www.thingiverse.com/thing:4830638) someday.

## HDD led
I hotglued a LED to the normal floppy LED and connected that to the LED pins on the simple SPI controller. A bit like done [here](https://www.youtube.com/watch?v=9V46ZEBu338) except that I used a single orange LED. Hotglue is easily removed with some IPA (isopropyl alcohol) if I ever change my mind. It looks really good:

![HDD led](/pictures/hdd_led.jpg)

## Compatiblity with other hardware
The simple SPI hardware uses a fixed address space in the upper region of the ZORRO-II IO space. Additional hardware like the A590 side-car should end up in the lower region of the ZORRO-II IO space so in theory they should not interfere. This is untested though.

A picture from the front of revision 1.1:

![Front](/pictures/simple_spi_front.jpg)

And a picture from the back of revision 1.1:

![Back](/pictures/simple_spi_back.jpg)
