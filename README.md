# NASSCOM-VSD-SoC-Design
## Advanced Physical Design using OpenLANE/Sky130
## Contents
1. [Inception of open-source EDA, OpenLANE and Sky130 PDK](#inception-of-open-source-eda-openlane-and-sky130-pdk)
    - [How to talk to computers?](#how-to-talk-to-computers)
        - [Introduction to QFN-48 Package, chip, pads, core, die and IPs](#introduction-to-qfn-48-package-chip-pads-core-die-and-ips)
        - [Introduction to RISC-V](#introduction-to-risc-v)
        - [From Software Applications to Hardware](#from-software-applications-to-hardware)
    - [SoC design and OpenLANE](#soc-design-and-openlane)
        - Introduction to all componenets of open-source digital asic design
        - Simplified RTL2GDS flow
        - Introduction to OpenLANE and Strive chipsets
        - Introduction to OpenLANE detailed ASIC design flow
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
## How to talk to computers?
### Introduction to QFN-48 Package, chip, pads, core, die and IPs
Arduino is a common electronics platform which is taken as an example here. The illustration shown below is the Arduino Leonardo microcontroller board based on the ATmega32u4. A quick summary on the microcontroller is that it features 20 digital input/output pins, out of which 7 can be used as PWM outputs and 12 as analog inputs. It incorporates a 16 MHz crystal oscillator, a micro USB connection, a power jack, an ICSP header, and a reset button, encompassing all necessary components to facilitate the microcontroller's operations. To initiate, one can seamlessly connect it to a computer using a USB cable or power it through an AC-to-DC adapter or battery.

![Picture1](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/7c190d7b-f48b-43a5-8697-a2271a28e9aa)

The below picture describes about the processor (high level view is circled in yellow in the aboove picture) and it's connectivity. 

![Picture2](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c3176cb9-34db-4581-a858-aeab397be237)

The below picture is referred to as a package. The pin placements of this package are entirely determined by the Arduino board under development. The connectivity between the chip and the package is illustrated through wires, enabling the transmission of signals in and out of the chip.

![picture4](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/2a6aea71-7930-4686-b37d-45846cbdc6d4)

#### Components:

![picture5](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/43dea6d2-d2d6-4a15-9cf2-0b03809e7b21)

- Pads: These are structures designed to facilitate the connection between the integrated circuit (IC) and the outside world. These pads serve as interfaces for electrical signals to enter or exit the IC package. They are strategically placed around the perimeter of the IC to align with the package's pins or solder balls, allowing for efficient electrical connection during assembly and usage.
- Core: It is a segmented unit of logic designed to carry out all processor functions, housing all essential digital logic building blocks within it.
- Die: It is a single, individual semiconductor chip that is fabricated on a silicon wafer. After fabrication on the silicon wafer, the wafer is diced into individual dies, each representing a separate unit that can be packaged and used as a discrete electronic component in various electronic devices.
- Macros: These refer to a pre-designed and pre-verified block of logic that performs a specific function and can be reused across multiple chip designs. Macros are typically larger and more complex than standard cells. Macros can include a wide range of functionalities, such as arithmetic units, memory blocks, communication interfaces, analog blocks, and specialized processors. They are designed to be easily integrated into larger chip designs and provide designers with ready-made solutions for common or complex functions.
- Foundary IP's: It refers to pre-designed and pre-verified blocks of reusable digital or analog circuitry provided by semiconductor foundries to their customers for integration into custom integrated circuits (ICs) or System-on-Chip (SoC) designs. These IP blocks can include various components such as standard cell libraries, memory blocks, input/output (I/O) cells, analog-to-digital converters (ADCs), digital-to-analog converters (DACs), interface controllers, and specialized processors.

While foundry IP is a broader concept encompassing various types of reusable circuit blocks provided by semiconductor foundries, macros specifically refer to pre-designed functional blocks intended for reuse within chip designs. Macros are a subset of foundry IP and are typically larger, more complex, and designed to perform specific functions within a chip.

### Introduction to RISC-V
In order to execute a C-program on a chip with a particular layout, first it has to be compiled into an assembly language program like RISC-V. Then it is converted into machine language, which is nothing but binary format to be understood by the computer hardware. The RISC-V specifications have to be translated to hardware description language (HDL) to pass on to the layout. So we need to implement these RISC-V specifications using RTL. So from the user's point of view, C-program is executed and that should get automatically executed by the chip of the hardware to get the required output.

![picture6](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/73c4f227-7223-4ac0-bd66-06225a40e129)

### From Software Applications to Hardware
Wondering how the apps run on a click? So, this is how...
This software application is processed within system software, where it undergoes conversion into binary language. System software consists of the operating system (OS), compiler, and assembler. The operating system manages memory, handles I/O operations, and allocates resources. Subsequently, the operating system produces small functions in languages such as C/C++/Java, which are then compiled into executable files (.exe) by the corresponding compiler. The syntax of these instructions depends on the available hardware architecture, such as Intel, ARM, MIPS, or RISC-V. The assembler's role is to translate these instructions into binary code comprehensible by the machine. These binary instructions are then transmitted to the hardware, where they are interpreted, and the desired outputs are generated according to the underlying logic.

![picture7](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/ebb18a92-6fef-4558-858a-817ee632938f)
Flow:StopWatch App to Hardware

![picture8](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/b496f214-0b5b-4dbc-b3bc-a0ae3ddff95b)
Flow: ISA->HDL->Netlist->Physical design




## SoC design and OpenLANE
### Introduction to all componenets of open-source digital asic design
To design an ASIC, we mainly require RTL designs, EDA tools, and PDK data. 

![picture8](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f5a94933-4faf-49e2-a414-b15b2ecf3f3b)

- RTL IPs: Pre-designed and pre-verified building blocks written in a Hardware Description Language (HDL). They describe how the circuit operates at a functional level.
- EDA Tools: Electronic Design Automation Tools are software tools used to design, verify, simulate, and analyze designs. Popular EDA tools include Yosys, OpenSTA, OpenROAD etc...
- PDK: Process Design Kit is a collection of files provided by a foundry that specifies the rules and limitations of their manufacturing processes. Some of the key things found in a PDK are Process Design Rules, Device Models, Digital Standard Cell Libraries, IO Libraries

![picture9](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/43ce28ef-a976-4862-9dad-f0e67b24d3c4)
![picture10](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/6be1afbd-ea04-4891-9bc5-4dcb24055848)

### Simplified RTL2GDS flow

![picture11](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/63bdce98-6070-4d4d-af7c-ae3d4f14463b)

1. Synthesis refers to the process of converting a hardware description language (HDL) such as Verilog or VHDL, into a gate-level representation. This gate-level representation consists of interconnected logic gates, flip-flops, and other standard cells from a technology library.

![picture13](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/4701a800-8af9-4353-898b-4fecdabe1d35)

2. Floor and Power Planning

Floor planning involves the initial layout and placement of various functional blocks, components, and macros. It determines the spatial arrangement of these elements to optimize chip performance, area utilization, and connectivity while meeting design constraints. The main objectives of floor planning include minimizing wire lengths, reducing signal delays, optimizing power distribution, and ensuring efficient chip utilization.

![picture14](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/7f007389-abc1-4fa8-8b20-81a1b7a7247f)

Power planning involves the distribution of power supplies and the design of power distribution networks (PDN) to ensure stable and reliable power delivery to all components. The primary objective of power planning is to minimize voltage drop and noise, reduce power distribution network (PDN) resistance and capacitance, and ensure uniform power distribution across the design. Typically, the chip is powered by multiple VDD and VSS power pads. The power pads are connected to all components through rails of vertical and horizontal metal straps. Such parallel structures reduce resistance and hence the IR drop. These are on upper metal layers since they are thicker than lower metal layers and hence have low resistance.

![Picture15](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/2266a022-c823-409b-a462-3e501d80aeb8)

3. Placement refers to the process of determining the physical locations of various components, such as standard cells, macros, and I/O pads

![picture16](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/d64fd0a2-d348-4848-bc57-3f1f6f441b0c)

4. Clock Tree Synthesis is done before signal routing, as the clock needs to be routed by creating the clock distribution network that delivers clock to all the sequential blocks

![Picture17](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/b80958fd-a24b-435a-b9bc-f56550ca058e)

5. Routing refers to the process of determining the physical paths of interconnections between various components. The router uses available metal layers as defined by PDK. For each metal layer, the PDK defines the thickness, the pitch etc. The Sky-130 PDK defines 6 routing layers.

![picture18](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/544708f5-bafc-4b3f-9cbc-3ffc2e41a852)

6. Sign Off

Physical verification like DRC, LVS, and timing verification like STA

### Introduction to OpenLANE and Strive chipsets
