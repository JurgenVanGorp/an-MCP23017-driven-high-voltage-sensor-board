Guide creation date: 13-Nov-2021

# An MCP23017 driven high voltage sensor board
A schematic and hardware board for an MCP23017 multi-I/O controller driven 230V sensor board.

Previous topic: [Step 5: Building your hardware: an MCP23017 driven relay board.](https://github.com/JurgenVanGorp/an-mcp23017-driven-relay-board)

## Introduction

This guide describes a fairly straightforward way of measuring whether a light, motor, boiler ... has been switched on. The power is captured with a relay that switches the input of an MCP23017 to GND or +5V. The MCP23017 can the be read with a Raspberry Pi over the I2C bus. The solution is a PCB in Eurocard format (10cm x 16cm) with 15 high voltage (230V) relays, and one "Identification" LED output for verifying that the communication is working properly.

Let met start with answering your first question: "why are relays used, and not optocouplers?" Indeed, optocouplers have no moving parts and have a smaller footprint.

The answer is: power dissipation. The (230V) RT1 power relays that are used in this implementation consume 0,75 Watts each. When all 15 inputs are switched on, the board consumes 11.5 Watts, which is acceptable. Optocouplers typically switch on at 20...100 milliamps (TIL111: 100mA, 4N25/4N26/4N27/4N28: 60mA, PC817X series: min 20mA, typ. 50mA). Assuming we want to drive the optocoupler LED with 50mA, a 4.8KOhm series resistor is required (in practice: 4K7 or 5K1). The power consumption in the resistor would then be a whopping 12Watts. With all fifteen inputs switched on, you could easily boil an egg on the PCB.

So, using relays may seem "old school", but it does make sense.

The board needs a 12V power supply. The 12V is converted to 5V on the board, which is also used to power the mini secondary relays. With some modifications you can also use the board with 5V directly, but you would need to bridge the DC/DC converter. I am using 12V for the simple reason that all switches in my house operate on 12V. The translation to higher voltages is only done in the power box.

Four of the fifteen inputs will drive a secondary relay on the PCB. You can use these input sensors to directly drive (or disconnect) another device, e.g. an alarm or a warning light.

**WARNING** Even though the PCB board described below does provide you with physical segregation with relays, BE CAREFUL WITH THE HIGH VOLTAGES USED ON THE BOARD. The PCB design segregates the high voltage from the low voltage lines as much as possible, but won't protect you from accidently touching the back of the PCB. In the repository you can find the design of a cover that matches the PCB and that you can print for yourself with any 3D printer.

## Using and changing the files in this repository

I have created the schematic and PCB in this repository with the open and free [KiCad EDA](https://www.kicad.org/) software. I cannot express enough how much I am grateful to [the developers](https://www.kicad.org/about/kicad/) of this PCB software. As far as I'm concerned the times of too-expensive PCB software are over.

The PCB file looks as follows.

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/PCB_Layout.png)

The GERBER PCB Files are included in the package, so you don't need to compile these for yourself.

There are many companies that will be able to create the PCB files for you. My personal favorite is [Eurocircuits.eu](https://www.eurocircuits.com/), but that's a personal choice of course. You can also create your own PCBs; the PDF file is added in the repository. But let's be honest, ordering the PCB looks a lot more professional and will last longer.

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/SensorPCB.jpg)

## Components used.

This is the list of components used, and where I have found them.

| Quantity | Component    | Value                     | Source                 | ~cost/Piece   |
|:--------:|--------------|--------------------------:|------------------------|--------------:|
| 1        | PCB Board    | See Gerber files | [eurocircuits.eu](https://www.eurocircuits.eu) | € 35.00 | 
| 9 | Resistor | 1K |  | € 0.10 | 
| 6 | Resistor | 1K |  | € 0.10 | 
| 2 | Resistor | 470 Ohm |  | € 0.10 | 
| 1        | Capacitor    | 22nF  | [conrad.com](https://www.conrad.com/p/tru-components-tc-k22nf5-ceramic-capacitor-tht-22-nf-100-v-20-1-pcs-1589451) | € 0.10 | 
| 5        | Capacitor    | 220nF | [conrad.com](https://www.conrad.com/p/kemet-c320c224m5u5ta-ceramic-capacitor-radial-lead-220-nf-50-v-20-l-x-w-x-h-508-x-318-x-584-mm-1-pcs-1420328) | € 0.60 | 
| 2        | Capacitor radial | 220uF | [conrad.com](https://www.conrad.com/p/europe-chemicon-eky-500ell221mj16s-electrolytic-capacitor-radial-lead-5-mm-220-f-50-v-20-x-h-10-mm-x-16-mm-1-pc-1505568) | € 0.50 | 
| 2 | Capacitor | 220uF axial | [conrad.com](https://www.conrad.com/p/vishay-2222-021-36221-electrolytic-capacitor-axial-lead-220-f-25-v-20-x-l-65-mm-x-18-mm-1-pcs-446056) | €1.60 | 
| 2 | Connector | RJ10 | [conrad.com](https://www.conrad.com/p/econ-connect-mjuse44gab-modular-mounted-socket-mjuse44gab-socket-horizontal-mount-number-of-pins-4-black-1-pcs-1311383) | € 0.60 | 
| 1        | Connector 1x2 | DG350-3.5-03P    | [conrad.com](https://www.conrad.com/p/degson-dg350-35-02p-14-00ah-200-screw-terminal-2-mm-number-of-pins-2-green-200-pcs-1595136) | | 7        | 1x6 Spring connector | PTR 54191060051E | [conrad.com](https://www.conrad.com/p/ptr-54191060051e-spring-loaded-terminal-075-mm-number-of-pins-6-pebble-grey-1-pcs-569770) | € 0.36 | 
| 3        | Jumper       | N/A      | e.g. [conrad.com](https://www.conrad.com/p/tru-components-shorting-jumper-contact-spacing-254-mm-pins-per-row2-content-100-pcs-1693950) | € 0.35 | 
| 15 | Relay | RT314730 | [conrad.com](https://www.conrad.be/p/te-connectivity-rt314730-printrelais-230-vac-16-a-1x-wisselcontact-1-stuks-504255) | € 6.22 | 
| 4 | Relay | G5V-1-5DC | [conrad.com](https://eu.mouser.com/ProductDetail/Omron/G5V-1-2-DC5?qs=sGAEpiMZZMsKEdP9slC0YbH1hXJZnuIH7AhUMezYhKg%3D) | € 1.45 | 
| 1        | IC           | MCP23017                  | [mouser.com](https://eu.mouser.com/ProductDetail/Microchip/MCP23017-E-SP?qs=sGAEpiMZZMsVgcksf1EMUq%252Bl%252ByrW%252Br2s)| € 1.35 |
| 2        | 14 pins socket | MCP23017                  | [mouser.com](https://www.conrad.com/p/tru-components-ic-socket-contact-spacing-254-mm-762-mm-number-of-pins-14-1-pcs-1568704) | € 0.40 |
| 1        | LED "IDENT"  | Green 5 mm | [conrad.com](https://www.conrad.com/p/vishay-tlhr-5400-led-wired-super-red-circular-5-mm-10-mcd-30-30-ma-2-v-184389) | € 0.20 | 
| 1        | LED "POWER"  | Red 5 mm | [conrad.com](https://www.conrad.com/p/kingbright-l-7113id-led-wired-red-circular-5-mm-45-mcd-30-20-ma-2-v-180139) | € 0.20 | 
| 1        | 12V to 5V DC | OKI-78SR-5_1.5-W36H-C **(1)** | [mouser.com](https://eu.mouser.com/ProductDetail/Murata/OKI-78SR-5-15-W36H-C?qs=sGAEpiMZZMsbRVlHDoeFZD%252BySXGErvIJc3su7QBo1Is%3D) | € 3.64 | 
| 4       | Diode        | 1N4007 | [conrad.com](https://www.conrad.com/p/diotec-si-rectifier-1n4007-do-204al-1000-v-1-a-162272) | € 0.09 | 
| TOTAL   | | | | ~ € 153 |

**(1)** The OKI-78SR is a 12V DC to 5V DC converter. It is pin-compatible with an [LM7805](https://eu.mouser.com/ProductDetail/Texas-Instruments/LM7805CT?qs=sGAEpiMZZMsFKQfwwdJx%2FxW4Tr%252BxPyoqmeSSFfZw3i4%3D). The current drawn is very low, so feel free to replace it with an LM7805 if you prefer.


The assembled board should look something like the following. The RJ10 connectors may be hard to find, so you may want to solder the wires directly.

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/SensorPCBassembledFront.jpg)

## Protective Cover

The high voltage lines are routed on the PCB such that they are at a distance from the low-voltage lines. However, it is still possible to touch the high voltage pins at the bottom of the PCB. In the "Protection Shield" folder the gcode and stl files can be found to print a protective cover for the PCB. The holes match the holes in the PCB.

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/SensorPCBassembledBack.jpg)

The 3D layout was created with an older version of [Sketchup](https://www.sketchup.com/plans-and-pricing/sketchup-free). The original file is added in the repository.

## Schematic decription

The overall schematic looks as follows. You can [download the shematic in PDF format here](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/PCB%20MCP23017%20High%20Voltage%20sensors/Schematic.pdf).

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/FullSchematic.png)

This overview is a bit crowded, and not easy to follow. Let's highlight a few details here. 

In the top left and top right of the schematic, you can find the connector blocks. The connectors on the right hand are the 230V inputs. The 2 x 15 inputs are paired and directly drive the 230V relays.

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/HighVoltInputsdetail.png)

The connection is straightforward. If a relay is not powered, the MCP23017 pin is connected to GND. If the relay is powered, the pin is connected to +5V.

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/RelayDetail.png)

The connector block on the top left side are secondary relays connected to the GPA0..GPA3 inputs. The NO (Normally Open), NC (Normally Connected) and CO (Common) pins of the relays are routed to the connectors.

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/SecondaryBlockdetail.png)

Here also, the connection is straightforward. The 230V relay not only connects GND or +5V to the MCP23017 pin, but also powers a secondary relay. A diode is placed over the secondary relay to protect the PCB from over-voltage glitches when the relay is switched off.

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/RelayWithSecondary.png)

Finally, let's look at the MCP23017 block in more detail.
* The input pins of both GPA and GPB are routed to the relay blocks. Each input is protected with a 1KOhm resistor to protect the pin in case the MCP23017 pin is accidently configured as an output.
* GPB7 ("pin 15" in Home Assistant) is connected to the "Identification" LED D6 and can be used to identify the board and test the MCP23017 operation.
* The RJ10 connectors J4 and J10 are used to connect the I2C pins to the Raspberry Pi. Remark that both connectors are configured fully parallel, so that the connectors can be used to cascade multiple boards. 
* Jumpers JP1 .. JP3 are used as the address selectors.
* LED D1 is just connected to the +5V output and can be used to verify the +5V power.

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/mcp23017detail.png)

## Mounting the PCB Board

The PCBs are 10cm x 16cm which allows mounting them in a standard EuroRack, as in the image below. If you want to know more about Euroracks, [RS gives a nice overview here](https://nl.rs-online.com/web/generalDisplay.html?id=ideas-and-advice/eurocards-pcb-guide). They also sell different types of enclosures.

![alt text](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board/blob/main/images/RackOverview1.jpg)

An alternate way for mounting the board, is a DIN rail case. The components for a DIN rail mount are e.g. the following.

| Quantity | Component                    | Description            | Source                 | ~cost/Piece   |
|:--------:|------------------------------|------------------------|------------------------|--------------:|
| 2 or 3   | Phoenix Contact UM108-FE     | DIN rail mount         | [conrad.com](https://www.conrad.com/p/phoenix-contact-um108-fe-din-rail-casing-base-1075-plastic-10-pcs-454634) | € 0.70 | 
| 1        | Phoenix Contact UM108-SEFE/R | Right side panel       | [conrad.com](https://www.conrad.com/p/phoenix-contact-um108-sefer-din-rail-casing-side-panel-plastic-10-pcs-453303) | € 1.00 |
| 1        | Phoenix Contact UM108-SEFE/R | Left side panel        | [conrad.com](https://www.conrad.com/search?search=459039&searchType=regular) | € 1.00 |
| 1        | Phoenix Contact UM100-PROFIL 100CM | back plate (1m)  | [conrad.com](https://www.conrad.com/p/phoenix-contact-um100-profil-100cm-din-rail-casing-plastic-1-pcs-456207) | € 30.00 |



Next topic: [An example Home Assistant Installation setup](https://github.com/JurgenVanGorp/an-MCP23017-driven-high-voltage-sensor-board)
