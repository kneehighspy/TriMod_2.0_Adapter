# TriMod 2.0 Adapter

![](https://github.com/DL2DW/TriMod_2.0_Adapter/blob/main/Images/TriMod_Adapter_2.0.jpg)



## Introduction

With the help of this adapter a Commodore 1541 floppy drive can be turned into a Commodore 2031LP. The difference between the two drives is the interface to the outside world.

While the 1541 was built with the CBM bus for connection to the Commodore C64 & C128, the 2031LP can be connected to the "big" computers from Commodore of the PET and CBM series. For this purpose, this drive has a parallel IEEE-488 (aka GPIB) interface.

The idea of turning a Commodore 1541 into a 2031LP originally came from André Fachat. There were also instructions for a corresponding adapter in the 80's in the German computer magazine "64er".

I developed this idea further and so the TriMod adapter was created.

The special thing about it is that it is not only switchable, but it also provides a parallel cable for floppy speeders like SpeedDOS.

The first version of the TriMod adapter was still discretely built with TTL devices and could "only" switch between the CBM bus and IEEE-488.

With the TriMod adapter 2.0, I have replaced the TTL components with a CPLD, as well as the possibility to be able to use SpeedDOS, etc. again.

To the best of my knowledge, the TriMod Adapter 2.0 was not only the first working adapter based on a CPLD but also the first to offer a parallel cable connection for floppy speeders.

![](https://github.com/DL2DW/TriMod_2.0_Adapter/blob/main/Images/TriMod_Adapter_2.0_complete_view.jpg)



![](https://github.com/DL2DW/TriMod_2.0_Adapter/blob/main/Images/TriMod_Adapter_2.0_top_view.jpg)



## Background information

I had created a replica of the motherboard of the Commodore Floppy 1541 in early 2019. This I had done because I had some drives where the board was no longer in good condition, partly hairline cracks and also many traces had to be repaired by hand.

So I had sat down, and rummaged through many circuit diagrams (There is unfortunately not a single correct circuit diagram that has no errors) and measured many connections. So the first version was created in spring 2019.

At the same time I was developing my first TriMod adapter. At some point I got the idea to combine the whole thing. 

I now had a working schematic of the 1541 in electronic form. So I decided to "pimp" the circuit board of the 1541 a bit.

In the course of development, I then also encountered the problem that the board naturally became larger and larger. Not that there wasn't enough space in the 1541, but the price that was called for the production shot up more and more.

So the thought was to pack the whole thing into a CPLD. 

So that I did not always have to make a large mainboard in case of error, I first developed an adapter that can be plugged into a 1541.

Thus, the first TriMod 2.0 adapter was created in the fall of 2019.

![](https://github.com/DL2DW/TriMod_2.0_Adapter/blob/main/Images/TriMod_Adapter_CPLD_Version.jpg)



This adapter was therefore never designed for anything other than to develop a new system board for the 1541, so I didn't look for it to be as small as possible, or to fit well in the 1541, or to close the case, etc.

After I published the files for the first adapter here on GitHub, I got many requests to publish a smaller version.

With this adapter I would like to fulfill this request.



## Technical information

Because of the many I/Os required, a larger CPLD was needed in the form of the Xilinx XC95144XL-10TQ100. Unfortunately, this TQFP-100 device is so large that it no longer fits between a DIP-40 socket. In addition the two driver components SN75160 and SN75161 are needed, which are not necessarily tiny even in SMD.

And unfortunately the last two components are too big to fit underneath a DIP-40 socket.

Furthermore I didn't want to use SMD pin headers, because many people have problems soldering them. In addition I find these also somewhat expensive in the version with turned precision pins.

I therefore kept everything as compact as possible and placed the components quite close to each other.

Then I replaced the standard IDC headers with MicroMatch connectors. They are a bit smaller, but use the same ribbon cable. 

And so the new TriMod 2.0 adapter was created. 



## Features

- 3 interfaces - CBM bus, connection for parallel cable and IEEE-488

- Switchable between CBM bus, incl. parallel cable and IEEE-488

- Set Device ID is the same for both modes

- When switching or changing the Device ID, a reset of the floppy is automatically executed

- If the floppy is in IEEE-488 mode, the CBM bus cable can still remain connected without disturbing other CBM bus devices. Practical if for example a printer is also connected to the bus. If you then switch to IEEE-488 mode, nothing needs to be changed in the cabling. The floppy is addressed via IEEE-488, while the printer is addressed via the CBM bus.

- Automatic Kernal switching

- Alternative Kernal possible. In both modes it is possible to switch to another kernel (Is not yet enabled) in the current firmware).

- LEDs for status display

  

# Firmware flashing

To upload the firmware, I have provided a 6-pin header (on the back of the board, the pins are labeled accordingly).

The easiest way to do this is with the tool xc3sprog and a FTDI FT232H adapter. You can get this adapter from China for about $7. Here in Germany this adapter costs about 15,- Euro.

A very well described tutorial how to connect the adapter and flash the JED file with xc3sprog can be found at https://github.com/1c3d1v3r/neatPLA/tree/master/programming.

The pin assignment is as follows:

![](https://github.com/DL2DW/TriMod_2.0_Adapter/blob/main/Images/TriMod_Adapter_2.0_pin_header.jpg)

The pins for Kernal 2, or 2nd Kernal are currently not active. I have provided them in case you need another Kernal in both modes. For example once SpeedDOS and once Original to switch. 

The lower pin header is double rowed, the right side is always GND. So if necessary jumpers can be plugged to switch the mode or to change the device ID.

Attention: As soon as you change the device ID or the mode, the 1541 is automatically reset. So this should not be done while the floppy is still writing.

Kernal 1 is simply connected to a 2564 Kernal adapter. The signal is LOW when in CBM bus mode and HIGH in GPIB (IEEE-488) mode.

To switch on the IEEE-488 mode, just connect the "Mode" pin to GND or set a jumper at the corresponding position.

The device switching is as usual. For Device 8 both jumpers must be closed or both pins must be connected to GND. 

I have planned to be able to request also the solder jumpers present on the 1541. This is currently not activated, because I personally find these solder jumpers very impractical. However, if this is desired, this can also still be activated in the firmware.



## BOM

Here is the list of components. 

| Quantity | Designator                                 | Manufacturer       | Manufacturer  Part Number | Description                                                  |
| -------- | ------------------------------------------ | ------------------ | ------------------------- | ------------------------------------------------------------ |
| 10       | C1,  C2, C5, C6, C7, C8, C9, C10, C11, C12 | KEMET              | C0603C104J3RACTU          | KEMET     C0603C104J3RACTU       SMD Multilayer Ceramic  Capacitor, 0603 [1608 Metric], 0.1 F, 25 V,   5%, X7R, C Series |
| 2        | C3,  C4                                    | KEMET              | C0805C106K8RACTU          | KEMET  - C0805C106K8RACTU - CAP, 10µF, 10V, 10%, X7R, 0805   |
| 2        | CN1,  CN2                                  | Preci-Dip          | 830-10-020-10-001101      | Conn  Header Vert 20POS 2MM                                  |
| 1        | CN3                                        | MPE-Garry          | 369-1-010-0-NTX-KT0       | Female Header Micro Match 1,27 mm Wire to Board Connector Series 369, 10 pos. |
| 1        | CN4                                        | MPE-Garry          | 369-1-024-0-NTX-KT0       | Female Header Micro Match 1,27 mm Wire to Board Connector Series 369, 24 pos. |
| 1        | CN5                                        | Sullins            | PRPC003SFAN-RC            | CONN  HEADER .100" SNGL STR 3POS                             |
| 1        | CN6                                        | Sullins            | PRPC006SFAN-RC            | CONN  HEADER VERT 6POS 2.54MM                                |
| 1        | CN7                                        | Sullins            | PRPC005SFAN-RC            | CONN  HEADER VERT 5POS 2.54MM                                |
| 1        | D1                                         | Wurth  Electronics | 150060BS55040             | LED  BLUE DIFFUSED 0603 SMD                                  |
| 1        | D2                                         | Wurth  Electronics | 150060BS55040             | LED  BLUE DIFFUSED 0603 SMD                                  |
| 1        | D3                                         | Wurth  Electronics | 150060SS55040             | LED  RED DIFFUSED 0603 SMD                                   |
| 1        | D4                                         | Wurth  Electronics | 150060YS55040             | LED  YELLOW DIFFUSED 0603 SMD                                |
| 2        | Q1,  Q2                                    | Infineon           | BCR135                    | BCR135  Series NPN 50 V 100 mA SMT Silicon Digital Transistor - SOT-23-3 |
| 4        | R1,  R2, R3, R4                            | Yageo              | RC0603FR-071KL            | RES  SMD 1K OHM 1% 1/10W 0603                                |
| 1        | RN1                                        | Yageo              | TC124-FR-0710KL           | RES  ARRAY 4 RES 10K OHM 0804                                |
| 1        | U1                                         | Exar               | SPX5205M5-L-3-3/TR        | LDO  Regulator Pos 3.3V 0.15A 5-Pin SOT-23 T/R               |
| 1        | U2                                         | Texas  Instruments | SN75160BDWR               | TEXAS  INSTRUMENTS     SN75160BDWR.      GENERAL-PURPOSE INTERFACE  BUS ((NW)) |
| 1        | U3                                         | Texas  Instruments | SN75161BDWR               | TEXAS  INSTRUMENTS     SN75161BDWR                           |
| 1        | U4                                         | Xilinx             | XC95144XL-10TQ100C        | IC  CPLD 144MC 7.5NS 100TQFP                                 |

If you liked my project, I would be very happy about a small coffee donation.

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/R6R62T6RN)



# License

The contents of this repository, with the exception of the Docs and Software directories, are released under the following license:

- the "Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License" (CC BY-NC-SA 4.0) full text of this license is included in the [LICENSE.CC-NC-BY-SA-4.0](https://github.com/DL2DW/TriMod_Adapter/blob/main/LICENSE.CC-NC-BY-SA) file and a copy can also be found at https://creativecommons.org/licenses/by-nc-sa/4.0/

