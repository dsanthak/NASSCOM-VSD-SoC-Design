# NASSCOM-VSD-SoC-Design
## Advanced Physical Design using OpenLANE/Sky130
Author : Deepthi Santhakumar

This is my compilation of notes for the [Workshop](https://vsdsquadron.vlsisystemdesign.com/digital-vlsi-soc-design-and-planning/) on SoC design planning using the Google-SkyWater 130nm process node within the [OpenLANE](https://github.com/The-OpenROAD-Project/OpenLane) flow.
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
      	- [Concept of pre-placed cells](#concept-of-pre-placed-cells)
      	- [De-coupling capacitors](#de-coupling-capacitors)
      	- [Power Planning](#power-planning)
      	- [Pin placement and logical cell placement blockage](#pin-placement-and-logical-cell-placement-blockage)
      	- [Steps to run floorplan using OpenLANE and view floorplan layout in Magic](#steps-to-run-floorplan-using-openlane-and-view-floorplan-layout-in-magic)
    - [Library Binding and Placement](#library-binding-and-placement)
        - [Netlist binding and initial place design](#netlist-binding-and-initial-place-design)
        - [Optimize placement using estimated wire-length and capacitance](#optimize-placement-using-estimated-wire-length-and-capacitance)
        - [Congestion aware placement using RePlAce](#congestion-aware-placement-using-replace)
    - [Cell design and characterization flows](#cell-design-and-characterization-flows)
    - [Timing characterization](#timing-characterization)
3. [Design library cell using Magic Layout and ngspice characterization](#design-library-cell-using-magic-layout-and-ngspice-characterization)
    - [Labs for CMOS inverter ngspice simulations](#labs-for-cmos-inverter-ngspice-simulations)
        - [IO placer revision](#io-placer-revision)
        - [SPICE deck creation and simulation for CMOS inverter](#spice-deck-creation-and-simulation-for-cmos-inverter)
        - [Switching Threshold Vm](#switching-threshold-vm)
        - [Static and dynamic simulation of  CMOS inverter](#static-and-dynamic-simulation-of--cmos-inverter)
        - [Lab steps to gitclone vsdstdcelldesign](#lab-steps-to-gitclone-vsdstdcelldesign)
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

DRC: DRC stands for Design Rule Check. The main purpose of DRC is to ensure that the layout adheres to manufacturing guidelines and is compatible with the semiconductor fabrication process. Some of the manufacturing rules are allowable geometries, dimensions, spacing, and other layout parameters. DRC is a critical step in ensuring the manufacturability and reliability of IC designs by detecting and correcting layout errors that could lead to fabrication defects or yield loss during semiconductor manufacturing.

LVS: LVS stands for Layout vs. Schematic. The layout of the chip is compared against the corresponding schematic representation to ensure their consistency and correctness. LVS tools extract netlists from both the layout and schematic representations of the design. The extracted netlists from the layout and schematic are compared to identify any differences. Once LVS clean, the layout is considered verified, and the design can proceed to subsequent stages of the physical design flow.

STA: STA stands for Static Timing Analysis. It evaluates the timing behavior of a digital circuit without considering dynamic factors such as signal transitions and clock skew. It determines whether the design meets setup and hold time constraints, maximum clock frequency, and other timing requirements.

### Introduction to OpenLANE and Strive chipsets
OpenLANE is an open-source digital ASIC (Application-Specific Integrated Circuit) design flow developed by efabless and Google, designed to automate the entire RTL-to-GDSII (Register Transfer Level to Graphic Design System II) flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization.

![Picture19](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/05550570-31bd-4d63-8d10-7b3f1104e6d9)

![Picture20](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/26061efd-cb55-45d7-9128-f3bd356b0272)

The objective of OpenLANE is to produce a clean GDSII (No LVS violations, No DRC violations, and no Timing violations) with no human intervention.

### Introduction to OpenLANE detailed ASIC design flow

![Picture21](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/d2a634e1-1597-4781-a62a-548a9d668d0c)

1. Synthesis Exploration step involves generation of reports showing delay vs area
   
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
1. Define width and height of core and die

   The height and width of the core are determined by the layout of the functional units within the core, while the height and width of the die are determined by the size of the semiconductor wafer on 
   which the chip is fabricated. The core represents the functional unit of the chip, while the die encompasses the entire semiconductor wafer containing multiple copies of the core.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/d5acad57-8e42-4846-99bd-2ee1f97a6246)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c705e6e3-2dbc-4495-81e2-b7f61f548cb1)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/2380f2c4-b2c5-4133-9bd9-6b49c1f26624)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/fab25f37-de02-4238-b85c-f6156b64e350)

   Let's calculate the area occupied by the above netlist on a silicon wafer. Before that the wires are ignored and the flops and combinational logic are combined together to get the total area. So, here, the 
   area will be 4 sq.units.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/5c1ec102-2c0e-458c-8c62-1895777521aa)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/ff65d8fd-5f4d-482a-8d9a-73c82359920c)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/1c6bdd21-b869-4e33-aaeb-efd0226b5b71)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/aa7a2d5c-27fb-465f-a5b9-58b3a01ffb40)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/61a9c207-f244-454e-abf3-f8c2fa96c0e9)

   Utilization factor  = (4 x 1sq.unit) / (2 unit x 2 unit)
                       = 1

    which means the core is completely occupied. In practical scenario, utilization is about 50%

   Aspect ratio = Height / Width

   In this case, height and width are same, so the aspect ratio is 1
   If the aspect ratio is 1 it shows that the chip is square, otherwise it is rectangle.

   Example:

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/877913ad-a5e1-4094-a063-90d6e8c33068)

   Utilization factor  = (4 sq.units) / (8 sq.units)
                       = 0.5

   Aspect ratio = 2 / 4
                = 0.5

### Concept of pre-placed cells
2. Define Locations of Preplaced Cells

   Preplaced cells are reusable complex logic blocks or modules or IPs or macros that is already implemented (memory, clock-gating cell, mux, comparator). The placement of these cells in the core is user-defined 
   and must be done before placement and routing of other cells. The APR tools will not be able to touch and move these preplaced cells so this must be very well defined.

   Consider the following netlist being divided into two set of blocks with connectivity preserved.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/8b8c9aff-f11c-4595-8606-8d00a65c83b8)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/0880716e-72f3-4da5-90d0-fc2c700c2731)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/48bc0a36-1a67-49ca-8869-ac93085128c1)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/451b3211-c192-4142-a89e-d7fdc8516bcb)

   So, if Block1 has to be used by a designer, it can be directly handed over since it is blackboxed. This block can be used across the designs. It means the block can be reused.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/0fb1492a-25c2-4a28-979b-934ad5e9a679)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/3f7505db-df6c-45fd-be9f-a78a96ffdab9)

### De-coupling capacitors
3. Surround pre-placed cells with decoupling capacitors

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/16a8c9d2-0f64-4c02-854c-a7693195e9c4)

   Consider the circuit below as a part of a block. Whenever the circuit switches, there is an amount of current demand. For example, if the AND gate switches from logic0 to logic1, the capacitance has to 
   completely charge. The amount of charge will be sent from the supply voltage. And when the logic switches from logic1 to logic0, the capacitance discharges and it's the responsibility of the Vss to take that 
   discharged current.

   In reality, when the Vdd supplies voltage to the circuit, there is a drop due to resistance, inductance and capacitance of the wire and supplied volatge is Vdd'

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/448c3665-e5c0-45eb-9c34-774f79df49b4)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/77bf30f5-2ec3-4881-bcac-e60d3e4b726a)

   The Vdd' should be within the noise margin range which is from Vih to Voh. If it is present somewhere in the undefined region, then the logic 1 is unstable. This is because of the large physical distance from 
   the main power supply to the circuit.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/5cb88a92-3996-4ddd-940d-035cec7677c4)

   Solution to such problem is the addition of decoupling capacitors. We can consider decoupling capacitor as a huge capacitor completely filled with charge. the equivalent voltage across the capacitor is same 
   as seen across the main supply voltage. The capacitor decouples the circuit from the main supply.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/2a976671-9b4f-4ed8-b9f7-066aea5c8c30)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/710083be-a67c-4ef8-b705-e7acc8684c2c)

### Power Planning
Consider the circuit as a macro and it demands more current. Say, it's used as a driver as well as load, and the load should receive the same signal quality as the driver.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f527580c-8111-40a7-b73f-b5befdb28852)

Decoupling capactor is not feasible to be added all over the chip but only on the critical elements.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/4d3e128a-046d-47b7-8544-53f349b0f8f1)

In the below picture, 1 means the capacitor is charged to V and 0 means the capacitor is discharged. If the wire bus is connected to an inverter, then it means that all the capacitors charged to V will discharge at the same time. Large number of elements switching to logic0 might cause Ground Bounce due to huge amount of current that needs to be sinked at the same time, and switcing to logic 1 might cause Voltage Droop due to insufficient current from the power source to all elements. Ground bounce and Voltage Droop might cause the voltage to not be within the noise margin range. The solution is to have multiple powersource taps (power mesh) where elements can source current from the nearest Vdd and sink current to the nearest Vss tap.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/8d7eeda1-5331-4266-a9c8-24ee1c6147e3)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/244f12b3-3d6a-44d9-9e6b-237c031c27bc)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/79cc70a5-435b-4768-97e0-154ffd0c944a)

Instead of single power supply as before, there are multiple Vdd and Vss lines. If a logic demands current, it can tap current from the nearest power supply.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f31e18e8-9992-4e20-a399-76b26f242412)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/7f046f41-5496-4d1d-b2e2-62746808b28f)

### Pin placement and logical cell placement blockage

Let's take below design as an example to be implemented. The connectivity information between the gates is coded usign VHDL or Verilog language and is called as netlist.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/d5881fd7-d30e-4ce8-afb0-8a50d2351d64)

The input and output ports are placed on the left and right spaces between the core and the die. The placements of the ports depends on where the cells are placed. The clock ports are bigger in size than data ports since the clocks are driving the cells continuously. So, we need the least resistance paths for the clocks. Bigger the size, lesser the resistance.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/a43d93f6-0edc-4642-ad2b-c6610d70d71c)

Once pin/port placement is done, Logical Cell Placement Blockage is created to make sure that the APR tool does not place any cell on the pin locations.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/da74db6f-4736-4974-af7c-69479ca352d8)

Floorplan summary:

![pic1](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/4f819810-21f8-45e4-9c1a-390e06aa5997)

### Steps to run floorplan using OpenLANE and view floorplan layout in Magic
1. Setting configuration variables: Before running floorplan, the configuration variables or switches must be set. These are present in openlane/configuration directory:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/bcb333f7-8303-4126-ae4c-1e94c77efcd5)

The README.md consists of all configuration variables for every stage and the tcl files contain the default OpenLANE settings. 

2. Default parameters are set for floorplan stage in floorplan.tcl in OpenLANE

3. All configurations/switches accepted by the current run are from openlane/designs/[design]/config.tcl

The priority order from highest to lowest is as follows:

- openlane/designs/[design]/sky130A_sky130_fd_sc_hd_config.tcl
- openlane/designs/[design]/config.tcl
- openlane/configuration/floorplan.tcl

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/4b811bb9-f4e9-46d9-812b-3ca988827daf)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/1f37189d-c302-4b1a-96ee-310b1c33900a)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/5449d923-a09c-4efa-8ad9-370dd25b3dca)

In OpenLANE flow, the vertical and horizontal metals are one more than what we specify. If vertical metal is specified as 4, then it'll be 5, same case for horizontal.

4. Run floorplan on OpenLane: % run_floorplan
5. Floorplan output files are generated in this folder openlane/designs/picorv32a/runs/date/results/floorplan/picorv32a.floorplan.def which is a design exchange format, containing the die area and positions.
   The die area in this file is in database units and 1 micron is equivalent to 1000 database units.
   Area of die = (554570/1000) microns * (565290/1000) microns = 311829.1653 um^2

6. To view the layout after floorplan, we use magic tool

   Command: magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/a489217b-1914-46df-837a-de3ba433cf39)

   Press "s" to select whole die then press "v" to center the view

   Point the cursor to a cell then press "s" to select it, zoom into it by pressing "z"

   The IO pins are placed in a random equidistant mode as seen below based on the configuration (FP_IO_MODE = 1) set in openlane/configuration/floorplan.tcl

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/e7c88c04-5747-46a7-ad25-09bd37532ac8)

   The components in the layout can be identified by using the "what" command in tkcon window after selecting it

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f126ef8d-ca86-4bdd-99a4-bc15a7beae35)

   Standard cells are not placed but can be viewed at the bottom left corner of the layout

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/52661703-58b3-4b96-9b22-3189d3b338ff)


## Library Binding and Placement
### Netlist binding and initial place design
1. Bind netlist with physical cells

   Bind the netlist to physical cells with real dimensions. The physical cells come from a library that contains cells which can have different dimensions, various shapes of the cells, and delay 
   information. Bigger cells have lesser resistance while the functionality is the same. This means that the library has many flavors of cells.

2. Placement

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/17afb665-8dfe-496d-9573-7662d7d117ed)

   Placement is done based on connectivity. For example, FF1 is close to Din1 pin and FF2 close to Dout1 pin and combinational cells placed nearer to FF1 and FF2. This is to reduce delay.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/8f957e69-5c3d-48b0-a3af-b8060cc85a59)

### Optimize placement using estimated wire-length and capacitance
This is the stage where we estimate wirelength and capacitance (C=EA/d) and insert repeaters based on that. If the wirelength is more, then to maintain signal integrity, we add repeaters to reduce resistance. Repeaters basically reconditions the original signal and transfers. 

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/8d1c93ed-7b02-4e5b-907e-01be09aa6911)

### Congestion aware placement using RePlAce
Run placement on OpenLane: % run_placement

This commmand is a wrapper which does global placement (by RePlace tool), Optimization (by Resier tool), and detailed placement (by OpenDP tool). 

Placement is done in two stages:

- Global Placement : no legalization takes place and uses Half Perimeter Wirelength (HPWL) reduction model.
- Detailed Placement : legalization happens where the standard cells are placed in stadard rows, and there will be no overlaps of the cells.

The objective of placement is to converge the overflow value. If overflow value progressively reduces during the placement,then it implies that the design will converge and placement will be successful.

After running the placement, output is generated in this folder /openlane/designs/picorv32a/runs/date/results/placement/picorv32a.placement.def

Command: magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/3e9ab0f1-cb06-4b2f-8566-51383c5d8689)


## Cell design and characterization flows
All standards cells (AND, OR, BUFFER, INVERTER, flip-flops etc.) are present in standard cell library. The cells inside the library are of different flavors (different drive strengths, functionality, threshold voltage). If the cell size is more, then the drive strength is high to drive longer wires. If the threshold voltage is high, then it will take more time to switch than the one with lesser threshold voltage.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/07045aca-5d9f-43d8-a110-7beca2453777)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/5a2cfb59-55aa-4368-8937-37364b70f3c4)

Cell design is done as shown in the below picture:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/4c3d9e0b-4de7-46d7-be37-356a40dd3406)

- DRC & LVS Rules: tech files and poly substrate parameters
- SPICE Models: Threshold, linear regions, saturation region equations with added foundry parameters, including NMOS and PMOS parameteres
- User defined Spec: Cell height, cell width (drive strength), supply voltage, pin locations, metal layer requirement

- The standard cell library developer must adhere to the rules given so that when the cell can be used on a real design without any errors
- Circuit design is done modeling the pmos and nmos to meet input library requirement
- Layout design is done using Euler's path and stick diagram on Magic layout tool
  
Summary:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/60d2a376-d8c7-4b91-994c-2c4e81c76bf2)

Steps of characterization flow:

1. Read the spice model files
2. Read the extracted spice netlist
3. Define/Recognise the buffer behavior
4. Read the subcircuits
5. Attach the necessary power sources
6. Apply stimulus
7. provide necessary output capacitance
8. provide simulation command


![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/45406ca7-4cfc-452b-829c-3c301e11dd0e)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/364f6bae-6ac0-4d17-921d-688bfbb90dec)

Steps 1 to 8 are fed in the form of configuration file to the characterization software called "GUNA". 

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/e9dbf76f-c663-4dc0-b3c6-15f62308f6b5)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c3e0ba61-2bfd-4916-aee2-5549f8285e8b)


## Timing characterization
Syntax and semantics of power.lib, timing.lib and noise.lib. These are necessary to understand the GUNA software workflow.

We are taking inverters connected back to back as an example.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/12d86746-05a4-4c7f-9607-df61537061b0)

Timing thershold definitions:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/8a5faedb-e2f4-489f-91ca-830fb9f91897)

Two inverters in series, red is output of first inverter and blue is output of second inverter:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f3199590-dc7c-40a1-9727-6559e45cc0a6)

The red is input waveform and blue is output waveform of the buffer. The left side is rise delay and right side is fall delay.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/b0635552-7e0d-441e-bdb4-e9d7e88ccd20)

propagation delay = ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c363d35c-e6b8-4183-91cc-bd726d1a6412)

transition time = ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/d657c396-1726-43b9-a3c5-13dcf4046df3)

Negative propagation delay is not expected. This means that the output comes before the input so it's important to choose correct threshold point to produce positive delay. Delay threshold is usually 50% and slew rate threshold is usually 20%-80%.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/bd271e0c-2cb2-4abe-9e12-8c131e04c6c9)




## Design library cell using Magic Layout and ngspice characterization
## Labs for CMOS inverter ngspice simulations
### IO placer revision
Configuration settings in OpenLANE can be changed in the shell itself, on the fly. For example, to make IO_mode not to be "random equidistant",

% set ::env(FP_IO_MODE) 2 

The IO pins will not be equidistant in mode 2 (default of 1). On re-running floorplan, we can see that the pins are placed based on of the Hungarian algorithms. The pins are stacked one over the other. However, changing the configuration on the fly will not change the runs/config.tcl, the configuration will only be available on the current session. 

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/613cf32c-f7a5-4f3c-a301-362dac2dacd5)

To echo current value of variable,

echo $::env(FP_IO_MODE)

### SPICE deck creation and simulation for CMOS inverter
SPICE deck comprises of connectivity information about the netlist, inputs to be provided to the simulation, information on tap points at which output will be taken and so on.
Component values in SPICE DECK: For PMOS, W/L (0.375u/0.25u means width is 375nm and lengthis 250nm). PMOS should be wider (atleast 2x or 3x) than NMOS. PMOS hole carrier is slower than NMOS electron carrier mobility, so to match the rise and fall time, PMOS must be wider (less resistance thus higher mobility) than NMOS. But in this case, we are taking same sizes for both PMOS and NMOS. The gate voltage is normally a multiple of length (250nm) (in the example, gate voltage can be 2.5V)

SPICE deck:

- component connectivity
- component values
- identify nodes
- name nodes

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/3e35dadb-ae85-490b-9e84-ed726d58280d)

SPICE deck netlist description:

- Syntax for the PMOS and NMOS: [component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]
- All components are described based on nodes and its values
- .op is the start of SPICE simulation operation where Vin will be sweep from 0 to 2.5 with 0.05V steps
- tsmc_025um_model.mod is the model file containing the technological parameters for the 0.25um NMOS and PMOS

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/8cf32654-851d-46a4-9f94-6933a3860130)

SPICE simulation:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/97b84252-81bd-4898-94fe-5476aafc94df)

The steps to follow for SPICE simulation,

1. Go to ngspice simulator
2. Source the .cir spice deck file
4. Execute the spice file by command: run
5. Execute command: setplot --> allows you to view any plots possible from the simulations specified in the spice deck
6. Select the simulation desired by entering the simulation name in the terminal
7. Execute command: display --> to see which nodes available for plotting
8. Execute command: plot out vs in
We can see the plot for above inputs. In this the width of both PMOS &NMOS is same.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/ec9452b7-483b-46cf-a09d-d6af56565072)


![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/83dec14d-90d5-4880-83e2-cdf37b10aa2b)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/65ea561b-3f8e-4b31-889d-11a30652f5c0)

### Switching Threshold Vm

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c3860c9b-cb7c-412e-aa27-1211dd9d632a)

1. The shapes are almost the same which means that CMOS is a robust device.
2. Parameters that defines the robustness of CMOS
   - switching threshold, Vm. It is the point where the Vin = Vout and both PMOS & NMOS are in saturation region. These will be turned on and there is high chances for leakage. There is a high possibility that the current flows directly from VDD to GND. Due to this, short circuit kind of device is seen.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/55456681-87f5-476c-960a-29930ce22e6c)

   - Propagation delay: rise or fall delay

### Static and dynamic simulation of  CMOS inverter
DC transfer analysis is used for finding switching threshold. Simulation is DC sweep from 0V to 2.5V with 0.05V steps:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/813c19bb-817f-40e6-ab13-b87c857cb4a6)

When a pulse is applied to the CMOS, transient analysis is used to find propagation delay.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/473d0b0a-f0be-481e-8986-a72a1e061708)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/1105af46-2711-4a19-87cb-91a205ccdb22)


### Lab steps to gitclone vsdstdcelldesign
