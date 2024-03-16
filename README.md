# NASSCOM-VSD-SoC-Design
## Advanced Physical Design using OpenLANE/Sky130
## Contents
1. [Inception of open-source EDA, OpenLANE and Sky130 PDK](#inception-of-open-source-eda-openlane-and-sky130-pdk)
    - [How to talk to computers?](#how-to-talk-to-computers)
        - [Introduction to QFN-48 Package, chip, pads, core, die and IPs](#introduction-to-qfn-48-package-chip-pads-core-die-and-ips)
    - SoC design and OpenLANE
    - Getting familiar to open-source EDA tools
2. Good floorplan vs bad floorplan and introduction to library cells
    - Chip FLoor planning considerations
    - Library Binding and Placement
    - Cell design and characterization flows
    - General timing characterization parameters
3. Design library cell using Magic Layout and ngspice characterization
    - Labs for CMOS inverter ngspice simulations
4. Pre-layout timing analysis and importance of good clock tree
5. Final steps for RTL2GDS using tritonRoute and openSTA

## Inception of open-source EDA, OpenLANE and Sky130 PDK
### How to talk to computers?
#### Introduction to QFN-48 Package, chip, pads, core, die and IPs
Arduino is a common electronics platform which is taken as an example here. The illustration shown below is the Arduino Leonardo microcontroller board based on the ATmega32u4. A quick summary on the microcontroller is that it features 20 digital input/output pins, out of which 7 can be used as PWM outputs and 12 as analog inputs. It incorporates a 16 MHz crystal oscillator, a micro USB connection, a power jack, an ICSP header, and a reset button, encompassing all necessary components to facilitate the microcontroller's operations. To initiate, one can seamlessly connect it to a computer using a USB cable or power it through an AC-to-DC adapter or battery.

![Picture1](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/7c190d7b-f48b-43a5-8697-a2271a28e9aa)

The below picture describes about the processor (high level view is circled in yellow in the aboove picture) and it's connectivity. 

![Picture2](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c3176cb9-34db-4581-a858-aeab397be237)

