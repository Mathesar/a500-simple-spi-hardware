# a500-simple-spi-hardware
Simple SPI controller for the Amiga 500 with support for SD-cards and Ethernet controller.
This repository contains the hardware CAD files.
For drivers look here [drivers](https://github.com/Mathesar/a500-simple-spi-drivers).


It seems I often need 3 revisions to get things right.
This repository contains rev 1.2 of the hardware.

## Revision 1.0
Revision 1.0 was my first attempt. It used a '688 as the address decoder and did not set MOSI high on reads.
Therefore this version had problems with SD-cards. I fixed it by using A0 instead of D7 to write but this made writes very slow.
Also, the '688 is currently unobtainium. It seems that only simple logic like the '00 or '02 is available in all logic families.

## Revision 1.1
So, revision 1.1 was created that solved all above issues. This is the version you can see on the pictures.
However, even 1.1 had some minor issues. Firstly I made a small mistake in the address decoder. That one is very easy to patch though and after the patch it functions as designed.
Secondly however, the PCB was slightly too large on the top left corner causing the shield to no longer fit. (tested on a revision 5 Amiga 500)

## Revision 1.2
Therefore, revision 1.2 was created that fixes those final issues. I did not built it but it is basically revision 1.1 but then with a slightly resized PCB and the address decoder bug fixed. It needs no patches whatsoever and the metal shield should not interfere.

## Building hints:
This SPI controller is intended to give an Amiga 500 an SD-card (to be used as a harddisk) and ethernet connectivity. 
The ethernet is optional and connected via a pinheader and some dupont jumper cables. The SD-card module is not optional and soldered directly to the interface as can be seen on the pictures. The reason for this is that the SD-card module supplies 3.3V to the logic chips (and to the optional ethernet controller) via it's onboard voltage regulator.
Pictures of the modules I used are in the schematic. It is very important to use exactly the SD-card module as I used. The circuit depends on the pinout and the onboard voltage regulator. The circuit also contain an old-fashioned electrolytic capacitor on the +3.3V rail. This component is mandatory! Without it the voltage regulator on the SD-card module might become unstable.
The pinheader for the SD-card actvity LED and the ethernet module should be right angle types or straight angle types bend over. Otherwise the whole setup becomes to high when the dupont cables are mounted. With straight angle pins everything fits nicely under the metal shield.
The pinheader that connects the controller to the 68000 socket on the A500 main board should be "turned" types with round pins. The thin part plugs into the Amiga, the thicker part is soldered on the SPI controller. This is very important as the wrong pins might damage your 68000 socket.

## Compatiblity with other hardware
The simple SPI hardware uses a fixed address space in the upper region of the ZORRO-II IO space. Additional hardware like the A590 sidecard should end up in the lower region of the ZORRO-II IO space so in theory they should not interfere. This is untested though.

A picture from the front of revision 1.1:

![Front](/pictures/simple_spi_front.jpg)

And a picture from the back of revision 1.1:

![Back](/pictures/simple_spi_back.jpg)
