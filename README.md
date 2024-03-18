# NASSCOM-VSD-SoC-Design
## Advanced Physical Design using OpenLANE/Sky130
## Contents
1. [Inception of open-source EDA, OpenLANE and Sky130 PDK](#inception-of-open-source-eda-openlane-and-sky130-pdk)
    - [How to talk to computers?](#how-to-talk-to-computers)
        - [Introduction to QFN-48 Package, chip, pads, core, die and IPs](#introduction-to-qfn-48-package-chip-pads-core-die-and-ips)
        - [Introduction to RISC-V](#introduction-to-risc-v)
        - [From Software Applications to Hardware](#from-software-applications-to-hardware)
    - [SoC design and OpenLANE](#soc-design-and-openlane)
        - [Introduction to all componenets of open-source digital asic design](#introduction-to-all-componenets-of-open-source-digital-asic-design)
        - [Simplified RTL2GDS flow](#simplified-rtl2gds-flow)
        - [Introduction to OpenLANE and Strive chipsets](#introduction-to-openlane-and-strive-chipsets)
        - [Introduction to OpenLANE detailed ASIC design flow](#introduction-to-openlane-detailed-asic-design-flow)
    - [Getting familiar to open-source EDA tools](#getting-familiar-to-open-source-eda-tools)
        - [OpenLANE Directory structure in detail](#openlane-directory-structure-in-detail)
        - [Design Preparation Step](#design-preparation-step)
        - [Review files after design prep, run synthesis, and characterize synthesis results](#review-files-after-design-prep-run-synthesis-and-characterize-synthesis-results)
2. [Good floorplan vs bad floorplan and introduction to library cells](#good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
    - [Chip FLoor planning considerations](#chip-floor-planning-considerations)
      	- [Utilization factor and aspect ratio](#utilization-factor-and-aspect-ratio)
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
OpenLANE is an open-source digital ASIC (Application-Specific Integrated Circuit) design flow developed by efabless and Google, designed to automate the entire RTL-to-GDSII (Register Transfer Level to Graphic Design System II) flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization.

![Picture19](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/05550570-31bd-4d63-8d10-7b3f1104e6d9)

![Picture20](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/26061efd-cb55-45d7-9128-f3bd356b0272)

The objective of OpenLANE is to produce a clean GDSII (No LVS violations, No DRC violations, and no Timing violations) with no human intervention.

### Introduction to OpenLANE detailed ASIC design flow

![Picture21](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/d2a634e1-1597-4781-a62a-548a9d668d0c)

1. Synthesis Exploration step involves generation of reports
   
![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/79396b9b-555c-440b-85f5-b72f21bfa208)

2. Design Exploration step is used to sweep the design configuration and it's useful to find best configuration for any given design. A sample of generated report is shown below.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/6100faa1-fb04-4cd4-9ef5-211768c81247)

3. OpenLANE Regression Testing

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/2b11ad32-a1dd-4f19-8632-551a49d4ddd1)

4. Design for Test (DFT)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/be412047-18dc-470c-8279-41080d61977f)

5. Physical verification (DRC & LVS)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/3501185b-24c1-42ac-8983-f8042a0dfde8)

6. Logic Equivalence Check (LEC) checks the logic synchronisation between physical implementation and the netlist

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/465268de-fd43-43c5-8cf5-45b708d4fecb)

7. Dealing with Antenna Rules violations

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/bd7caa6f-0352-4300-b8e5-ecae41ea00a8)
![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/b60fdc8c-f3fe-465c-b6ce-9e8ad3818c41)
![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/a1508a04-a792-4d55-b12d-6a9115674e9c)

8. Static Timing Analysis (STA) ensures that the design meets timing requirements. STA evaluates the timing behavior of a digital circuit without considering dynamic factors such as signal transitions and clock skew. It determines whether the design meets setup and hold time constraints, maximum clock frequency, and other timing requirements. The input to STA includes the synthesized netlist of the design, and timing constraints.




## Getting familiar to open-source EDA tools
### OpenLANE Directory structure in detail
The Open Lane working directory has the following folders and sub-folders. 

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/e82817dd-66ab-4fbf-9ebf-218584d15207)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f5b15f41-266e-4e43-a874-d67efe3df4c2)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/a91c4669-95f1-4297-993a-285a86b60d1f)

- open-pdks contains scripts that makes the commerical PDK to also be compatible with the open-source EDA tool
- sky130A pdk variant is made especially compatible for open-source tools. It contains libs.ref and libs.tech
    libs.ref contains all the process or technology specific files, example sky130_fd_sc_hd : Sky130nm Foundry Standard Cell High Density
    libs.tech has files specific for the tool (klayout,netgen,magic...) 
- skywater-pdk contains all Skywater 130nm PDKs

### Design Preparation Step
OpenLane can be invoked using docker command using the interactive session.
./flow.tcl -interactive runs the openlane in interactive mode.
% package require openlane 0.9 retrives all dependencies for running v0.9 of OpenLANE

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/6a9ca57e-a2df-4947-8f1a-f604be1e2cd8)

OpenLane has multiple designs and we are working on picorv32a
Inside a specific design folder there is a config.tcl which overrides the default settings on OpenLANE. These configurations are specific to a design (e.g. clock period, clock port, verilog files). The priority order for the OpenLANE settings:
sky130_xxxxx_config.tcl in OpenLane/designs/[design]/
config.tcl in OpenLane/designs/[design]/
default values in OpenLane/configuration/

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/0e400d78-1dcc-4c3c-8943-1480f1386f43)

Setup the design stage for the flow and to begin with synthesis of a design
prep -design picorv32a
The above command when executed, sets up the filesystem where the OpenLANE tools can dump the outputs. This creates a run/ folder inside the picorv32a directory which contains the command log files, results, and the reports dumped by each tool. The folder will be empty for now except for lef files generated by this design setup stage. The cell LEF files .lef and technology LEF files .tlef merge to generate merged.lef inside run/tmp/

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/d3f11002-f2e1-441a-baaf-5a247ba46ad8)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/b2220156-3fc2-4489-9f17-a2ba9bcaf9f7)

### Review files after design prep, run synthesis, and characterize synthesis results
Run synthesis with the command: run_synthesis
It runs yosys RTL synthesis, ABC scripts (for technology mapping) and openSTA.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/05fdd25c-a699-4c03-b758-01079de8dd25)

Results after synthesis is as follows.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/2f50f11f-a2aa-461f-86e7-8b8b6b6a95b9)

Here the DFF count: sky130_fd_sc_hd__dfxtp_2 = 1613

And total number of cells = 14876

D flip-flop ratio  = count of DFFs / total number of cells
	     = 1613 / 14876
	     = 0.108429685 (OR) 10.8429 %

After running synthesis, inside the runs/[date]/results/synthesis is picorv32a_synthesis.v which is the mapping of the netlist to standard cell library using ABC. The runs/[date]/reports/synthesis will contain synthesis statistic reports and STA reports. The runs/[date]/synthesis/logs contains log files.

![picture22](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/5a8e6524-0d2e-4ea1-b578-f2f9dfd2cc69)

![picture23](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/00bbf639-d6f1-486c-ac90-f973a1077270)

![picture24](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/0c3ba4a4-859a-469e-b5e6-937a06f1c906)




## Good floorplan vs bad floorplan and introduction to library cells
## Chip Floor planning considerations
### Utilization factor and aspect ratio
