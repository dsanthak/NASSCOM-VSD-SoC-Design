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
    - [Inception of Layout CMOS fabrication process](#inception-of-layout-cmos-fabrication-process)
    - [Lab introduction to Sky130 basic layers layout and LEF using inverter](#lab-introduction-to-sky130-basic-layers-layout-and-lef-using-inverter)
    - [Sky130 Tech File Labs](#sky130-tech-file-labs)
4. [Pre-layout timing analysis and importance of good clock tree](#pre-layout-timing-analysis-and-importance-of-good-clock-tree)
    - [Timing modelling using delay tables](#timing-modelling-using-delay-tables)
      - [Lab steps to convert grid info to track info](#lab-steps-to-convert-grid-info-to-track-info)
      - [Lab steps to convert magic layout to std cell LEF](#lab-steps-to-convert-magic-layout-to-std-cell-lef)
      - [Introduction to timing libs and steps to include new cell in synthesis](#introduction-to-timing-libs-and-steps-to-include-new-cell-in-synthesis)
      - [Delay tables](#delay-tables)
      - [Lab steps to configure synthesis settings to fix slack](#lab-steps-to-configure-synthesis-settings-to-fix-slack)
    - [Timing analysis with ideal clocks using openSTA](#timing-analysis-with-ideal-clocks-using-opensta)
      - [Setup timing analysis and introduction to flip-flop setup time](#setup-timing-analysis-and-introduction-to-flip-flop-setup-time)
      - [Introduction to clock jitter and uncertainty](#introduction-to-clock-jitter-and-uncertainty)
      - [Lab steps to configure OpenSTA for post-synth timing analysis](#lab-steps-to-configure-opensta-for-post-synth-timing-analysis)
      - [Lab steps to optimize synthesis to reduce setup violations](#lab-steps-to-optimize-synthesis-to-reduce-setup-violations)
    - [Clock Tree Synthesis TritonCTS and signal integrity](#clock-tree-synthesis-tritoncts-and-signal-integrity)
      - [Clock tree routing and buffering using H-Tree algorithm](#clock-tree-routing-and-buffering-using-h-tree-algorithm)
      - [Crosstalk and clock net shielding](#crosstalk-and-clock-net-shielding)
      - [Lab steps to run and verify CTS using TritonCTS](#lab-steps-to-run-and-verify-cts-using-tritoncts)
    - [Timing analysis with real clocks using openSTA](#timing-analysis-with-real-clocks-using-opensta)
      - [Setup timing analysis using real clocks](#setup-timing-analysis-using-real-clocks)
      - [Hold timing analysis using real clocks](#hold-timing-analysis-using-real-clocks)
      - [Lab steps to analyze timing with real clocks using OpenSTA](#lab-steps-to-analyze-timing-with-real-clocks-using-opensta)
      - [Lab steps to execute OpenSTA with right timing libraries](#lab-steps-to-execute-opensta-with-right-timing-libraries)
5. [Final steps for RTL2GDS using tritonRoute and openSTA](#final-steps-for-rtl2gds-using-tritonroute-and-opensta)
    - [Routing and design rule check (DRC)](#routing-and-design-rule-check-drc)
      - [Introduction to Maze Routing](#introduction-to-maze-routing)
      - [Design Rule Check](#design-rule-check)
    - [Power distribution network and routing](#power-distribution-network-and-routing)
      - [Lab steps to build power distribution network](#lab-steps-to-build-power-distribution-network)
      - [Lab steps from power straps to std cell power](#lab-steps-from-power-straps-to-std-cell-power)
      - [Basics of global and detail routing and configure TritonRoute](#basics-of-global-and-detail-routing-and-configure-tritonroute)
    - [TritonRoute features]

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
OpenLane can be invoked using `docker` command using the interactive session.
`./flow.tcl -interactive` runs the openlane in interactive mode.
`% package require openlane 0.9` retrives all dependencies for running v0.9 of OpenLANE

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/6a9ca57e-a2df-4947-8f1a-f604be1e2cd8)

OpenLane has multiple designs and we are working on picorv32a
Inside a specific design folder there is a config.tcl which overrides the default settings on OpenLANE. These configurations are specific to a design (e.g. clock period, clock port, verilog files). The priority order for the OpenLANE settings:
sky130_xxxxx_config.tcl in OpenLane/designs/[design]/
config.tcl in OpenLane/designs/[design]/
default values in OpenLane/configuration/

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/0e400d78-1dcc-4c3c-8943-1480f1386f43)

Setup the design stage for the flow and to begin with synthesis of a design

`prep -design picorv32a`

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

4. Run floorplan on OpenLane: `% run_floorplan`
5. Floorplan output files are generated in this folder openlane/designs/picorv32a/runs/date/results/floorplan/picorv32a.floorplan.def which is a design exchange format, containing the die area and positions.
   The die area in this file is in database units and 1 micron is equivalent to 1000 database units.
   Area of die = (554570/1000) microns * (565290/1000) microns = 311829.1653 um^2

6. To view the layout after floorplan, we use magic tool

   Command: `magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &`

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
Run placement on OpenLane: `% run_placement`

This commmand is a wrapper which does global placement (by RePlace tool), Optimization (by Resier tool), and detailed placement (by OpenDP tool). 

Placement is done in two stages:

- Global Placement : no legalization takes place and uses Half Perimeter Wirelength (HPWL) reduction model.
- Detailed Placement : legalization happens where the standard cells are placed in stadard rows, and there will be no overlaps of the cells.

The objective of placement is to converge the overflow value. If overflow value progressively reduces during the placement,then it implies that the design will converge and placement will be successful.

After running the placement, output is generated in this folder /openlane/designs/picorv32a/runs/date/results/placement/picorv32a.placement.def

Command: `magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &`

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

`% set ::env(FP_IO_MODE) 2 `

The IO pins will not be equidistant in mode 2 (default of 1). On re-running floorplan, we can see that the pins are placed based on of the Hungarian algorithms. The pins are stacked one over the other. However, changing the configuration on the fly will not change the runs/config.tcl, the configuration will only be available on the current session. 

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/613cf32c-f7a5-4f3c-a301-362dac2dacd5)

To echo current value of variable,

`echo $::env(FP_IO_MODE)`

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
Instead of designing an inverter from scratch, a github repository where this is already done is cloned to observe the layout.
1. Clone custom inverter standard cell design from [github repository](https://github.com/nickson-jose/vsdstdcelldesign)
2. Change directory to openlane: cd ~/Desktop/work/tools/openlane_working_dir/openlane
3. Clone the repository with custom inverter design: git clone https://github.com/nickson-jose/vsdstdcelldesign
4. Copy tech file to the vsdstdcelldesign directory: cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/
5. Open custom inverter layout in magic: magic -T sky130A.tech sky130_inv.mag &

Snippet of commands executed:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f103856c-febe-4182-9a64-a5c758dc1414)

Snippet of sky130_inv:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/ef7f9cb4-7a41-4f98-95a0-6757aa8fea16)


## Inception of Layout CMOS fabrication process

CMOS Fabrication Process (16-Mask CMOS Process):

1. Select a substrate

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/cc2da528-6b95-4ebe-a0cd-ebf7855e1903)

2. Create active region for transistors

   - Deposit Silicon Dioxide (SiO2) on the substrate
   - Deposit Silicon Nitride (Si3N4). It is a protection layer to prevent SiO2 layer to grow during oxidation
   - Deposit a layer of photoresist
   - Deposit mask-1 layer on top of photoresist. It covers the photoresist layer that must not be etched away (protects the two transistor active regions)
   - UV light is applied to remove unmasked portions
   - Remove mask-1 and photoresist layers
   - Place in furnace to grow the oxide in the other areas
   - Remove the Si3N4 layer using hot phosphoric acid to have only p-substrate and Sio2

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f77a9df7-05ca-4e84-8141-e59de31cab37)

3. N-Well and P-Well formation

   - Deposit photo resist layer to define the areas to protect
   - Deposit mask-2. Mask 2 protects the N-Well (PMOS side) while P-Well (NMOS side) is being fabricated then Mask 3 protects P-Well while N-Well is being formed
   - UV light is applied, and the exposed area of photoresist will be removed
   - Boron is used to form P-Well
   - Phosporus is used to form N-well
   - Place in furnace to diffuse boron and phosphorous to form wells. This process is called Twintub process.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/9ede667d-dce9-49b0-ac8a-1392f286cc77)

4. Formation of gate terminal

   Gate terminal is where the threshold voltage is controlled.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/db8cc5a9-16dd-4626-a4a1-349c8ea75295)

   - Deposit photo resist layer to define the areas to protect
   - Deposit mask-4
   - UV light is applied, and the exposed area of photoresist will be removed
   - Implant low energy boron at the surface of p-well using mask-4 to control the threshold
   - Implant phosphorous/arsenic for n-well using mask-5
   - Fix the oxide which is damaged by implantation steps by removing extra SiO2 using the hydroflouric acid and re-grow high quality SiO2 on p-substrate to contol the oxide thickness
   - Add polysilicon film
   - Add mask-6 and etch using photolithography
   - Etch off to form the gate terminal

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c13e82e6-092c-4e35-a2d8-1da5cb3f4aa0)

5. Lightly Doped Drain (LDD) formation

   Two reasons for LDD: hot electron effect & short channel effect

   - Mask 7 for NMOS (lightly doped N-type)
   - Mask 8 for PMOS (lightly doped P-type)
   - Heavily doped impurity (N+ for NMOS and P+ for PMOS) is for the actual source and drain but the lightly doped impurity will help maintain spacing between the source and drain and 
     prevent hot electron effect and short channel effect.
   - To protect the lightly doped regions, add SiO2 and create spacers using plasma anisotropic etching

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/898818da-7ec1-4d9f-8ed1-c8655d265658)

6. Source and Drain formation

   - Thin screen oxide is formed to avoid channeling. Channeling is when implantations dig too deep into substrate.
   - Mask-9 is for N+ implantation and Mask-10 for P+ implantation
   - The side wall spacers maintain the N-/P- while implanting the N+/P+
   - High temperature annealing is done
  
   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/8207632b-287e-4d4f-a5c8-b570abeba412)

7. Local interconnect formation

   - Contacts and interconnects are important to control the electrical characteristics by the designer.
   - Remove thin screen oxide to open up the source, drain and gate for building the contacts.
   - Titanium has less resistance and hence used
   - TiSi2 is used for local interconnects
   - Mask 11 is formed and TiN is etched off using RCA cleaning to create first level contact

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/e851f228-149a-41f6-9d7a-cad7ff863034)

8. Higher level metal formation

   - CMP (Chemical Mechanical Polishing) technique to planarize the surface
   - Create contact holes using photolithograhy process
   - Mask 12 is for first contact hole
   - Mask 13 is for first Aluminum contact layer
   - Mask 14 is for second contact hole
   - Mask 15 is for second Aluminum contact layer
   - Mask 16 is for making contact to topmost layer

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/377ec3d4-ce5a-402e-8cc2-85dd676789be)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/20bd9dd0-87b9-46d0-8a84-e717aef9245d)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/551b341e-6c35-420e-8aef-8bd0fbfa8916)


## Lab introduction to Sky130 basic layers layout and LEF using inverter

![picc1](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/14fd56d9-f1b6-4b4d-87a7-8b8b8f372a72)

In sky130A, the first layer is local-interconnect layer or local-i and then the m1, m2 and so on. Power and Ground lines are in m1. When polysilicon crosses ndiffusion the it is NMOS and if polysilicon crosses pdiffusion then it is PMOS is created. The output of the layout is the LEF file. It is used by the router in APR to get the location of standard cell pins to route them properly. So it is basically the abstract form of layout of a standard cell. 

Commands in tkcon window for spice extraction of the custom inverter layout:

1. `extract all`
2. `ext2spice cthresh 0 rthresh 0`  --> this extracts the parasitic information
3. `ext2spice`

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f1c001f2-ade4-4866-ab6e-8077d6964251)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/11b15341-3441-4cec-aeac-7d8bdbc9fdea)

Snippet of created spice file:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/23c913c2-2dad-4b71-9295-c714fb50b41c)



## Sky130 Tech File Labs

1. Lab steps to create final SPICE deck using Sky130 tech

   The default SPICE deck file using Sky130 is as seen in the previous section. Now we modify the file to plot a transient response. The final SPICE deck file is as below.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/00c47949-53cf-427e-83b0-3a6b0fde0d55)

   Command to load spice file for simulation in ngspice:

   `ngspice sky130A_inv.spice`

   Generate a graph using:

   `plot y vs time a`

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/cf5e9b25-c029-4ba2-b739-d66bb72499b2)

2. Lab steps to characterize inverter using sky130 model files

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/b175ae32-7e3d-4eb2-93ab-92b7847bb259)

   Using the above transient plot, we will now characterize the slew rate and propagation delay:

   Maximum voltage = 3.3V
   80% of maximum voltage = 2.64V
   20% of maximum voltage = 0.64V
   50% of maximum voltage = 1.65V

   - Rise Transition (output transition time from 20% to 80%):
     - Tr_r = 2.20278ns - 2.15946ns = 0.04332ns
       
       ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/194495b1-6c75-42e6-9680-f1bea4f111f0)

   - Fall Transition (output transition time from 80% to 20%):
     - Tr_f = 4.06818ns - 4.04073ns = 0.02745ns
       
       ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c5d35b66-fd83-4d3e-a318-00e74a64a971)

   - Rise Delay (delay between 50% of input and 50% of output) that is time taken for output to rise to 50% and time taken for input to fall to 50%:
     - D_r = 2.18381ns - 2.15003ns = 0.03378ns
       
       ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/2599bcf8-c7d8-4935-b1ca-174083e6193d)

   - Fall Delay (delay between 50% of input and 50% of output) that is time taken for output to fall to 50% and time taken for input to rise to 50%:
     - D_f = 4.05402ns - 4.0501ns = 0.00392ns
       
       ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c1f552bf-a103-4b7b-9796-4c92c3c1625c)

[Magic Tutorial](http://opencircuitdesign.com/magic/)

3. Lab introduction to Sky130 pdk's and steps to download labs

   Commands to download and view the corrupted skywater process magic tech file and other files to perform drc corrections:

   - Command to download the lab files: `wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz`
   - Extract it: `tar xfz drc_tests.tgz`
   - Change directory into the lab folder: `cd drc/drc_tests`
   - List all files: `ls -al`
   - Command to open magic tool: `magic -d XR`
   
4. Lab introduction to Magic and steps to load Sky130 tech-rules

   Useful websites:

   [Magic Technology File Format Manual](http://opencircuitdesign.com/magic/techref/maint2.html) - This site explains about tech files. All technology specific information comes from a 
   technology file. This file includes information as layer types, electrical connectivity, design rules, rules for mask generation, rules for extracting netlists etc.

   [Rules for SkyWater SKY130 PDK](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#rules-periphery--page-root)

   Steps:

   - Open magic with met3.mag as input

     ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/85697e45-1cd4-48e8-bbcd-010b0b701535)

   - In this view, we see a number of independent layouts containing some DRC errors

     ![picc2](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/261249dc-dcf1-460d-8485-011f50269748)

5. Lab exercise to fix poly.9 error in Sky130 tech-file

   - In tkcon window: `load poly`

     ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/18d5ca00-06e6-4861-a1e9-0bd3c354d701)

   - Let's look at rule poly.9
     As described in [Rules for SkyWater SKY130 PDK](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#rules-periphery--page-root), Poly resistor spacing to poly or spacing 
     (no overlap) to diff/tap should be atleast 0.48um

     ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/27c73e99-dd7b-447e-a64f-4cff4722a241)

     ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/1cbaf61d-60ec-4a95-8ca4-0db455c857e6)

     That's not the case here, so we have to fix the tech file to include this DRC.

   - Open sky130A.tech file in drc_tests directory. The included rules for poly.9 are only for the spacing between the n-poly resistor with n-diffusion and the spacing between the p-poly 
     resistor with diffusion. We will now add new rules for the spacing between the poly resistor with poly non-resistor. Highlighted in green below are the two newly added rules. First 
     one is the rule for the spacing between the p-poly resistor with poly non-resistor and the next one is the rule for spacing between n-poly resistor with poly non-resistor. The 
     allpolynonres is a macro under alias section of techfile.

     ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/643faa77-a7e9-4fff-ba12-08d5647e7c55)

     ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/63b02add-7d73-4f8c-9c7e-fbff2f005479)

   - In tkcon window: tech load sky130A.tech
     to check drc in tkcon window: drc check

     The new DRC rules will now take effect.

     ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/56631851-0977-4972-9e7c-c348cbf65980)

6. Lab exercise to implement poly resistor spacing to diff and tap

   To fix what is hown in below pic, modify the tech file to include not only the spacing between npolyres with N-substrate diffusion in poly.9 but also between npolyres and all types of 
   diffusion. 

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/7aafe008-ba80-41c1-b6be-6397e56180c1)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/00dfcc07-49b0-4353-85ec-97ecd60db094)




## Pre-layout timing analysis and importance of good clock tree
## Timing modelling using delay tables
### Lab steps to convert grid info to track info
sky130_inv.mag contains all information like PG information, port information, logic etc. OpenLANE is a PnR tool and a PnR tool does not require all the information present in .mag file. The only information that we'll be needing is the boundary, power and ground rails, and the inputs & outputs. This is the reason of using .lef files. So the objective is to extract the LEF file from Magic file and plug into picorv32a design.

From PnR point of view, there are few guidelines to be followed while making standard cell,
- The input and output ports lies at the intersection of the horizontal and vertical tracks (ensure the routes can reach that ports).
- The width of the standard cell must be odd multiple of the tracks horizontal pitch and height must be odd multiples of tracks vertical pitch

Tracks refer to the horizontal and vertical metal layers on which routing occurs. The grid formed by the intersection of horizontal and vertical tracks creates a routing grid, also known as a routing matrix. The ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/tracks.info contains track information.

Before changing grid:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/ab0effdf-91f0-4219-a4bb-051db697ffcc)

After changing grid values:

![picc3](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/d8b70a3e-7253-435b-9148-d0767108f9fa)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/1a66df50-f391-4406-bacb-526d97f4c17a)

This satisfies the first guideline mentioned above.

![picc4](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f9797453-c3de-4d67-b6f4-b0a5a8261467)

Second requirement is satisfied in below picture.

![picc5](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/1e39713b-8d62-4591-be00-b6d6acb506c7)

### Lab steps to convert magic layout to std cell LEF
LEF (Library Exchange Format) file is a standard file format used to describe the physical layout and characteristics of standard cell libraries or macro libraries. LEF files contain detailed information about the geometric shapes, sizes, layers, and other physical properties of individual cells or macros within the library. The instructions to set the port definitions are in this [site](https://github.com/nickson-jose/vsdstdcelldesign#create-port-definition)

Next, save the .mag file with a new filename. 
In the tcon terminal: `lef write `

It will generate a LEF file with the new filename.

LEF file:

![picc6](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/1c5bd87f-165b-438a-9f4f-dae39cc61c3f)

### Introduction to timing libs and steps to include new cell in synthesis

Inside pdks/sky130A/libs.ref/sky130_fd_sc_hd/lib/ are the liberty timing files for SKY130 PDK which contains the timing and power parameters for each cell needed in STA. It can either be slow, typical, fast with different supply voltages (1v80, 1v65, 1v95). These are called PVT corners. The library name sky130_fd_sc_hd__ss_025C_1v80 describes the PVT corner as slow-slow (delay is maximum), 25° Celsius temperature, at 1.8V power supply. Timing and power parameter of a cell is obtained by simulating the cell in a variety of operating conditions (different corners) and these data are represented in the liberty file. The liberty file characterizes all cells and is used during ABC mapping during synthesis stage which maps the generic cells to the actual standard cells available in the liberty file. 

1. Copy the extracted lef file sky130_vsdinv.lef and the liberty files sky130*.lib from /openlane/vsdstdcelldesign/libs to the src directory of picorv32a.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/97ee9fb9-59d8-4680-85ce-138544293099)

2. Add the folowing to config.tcl inside the picorv32a:

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/d56d0fb4-b410-40e0-81f6-f8dfe2c19c80)

   This sets the liberty file that will be used for ABC mapping of synthesis (LIB_SYNTH) and for STA (_FASTEST,_SLOWEST,_TYPICAL) and also the extra LEF files (EXTRA_LEFS) for the 
   customized inverter cell.

3. Run docker and prepare the design picorv32a. Plug the new lef file to the OpenLANE flow.

   `docker`
   
   `./flow.tcl -interactive`
   
   `package require openlane 0.9`
   
   `prep -design picorv32a`
   
   `set lefs [glob $::env(DESIGN_DIR)/src/*.lef]`
   
   `add_lefs -src $lefs`

5. Next run_synthesis. sky130_vsdinv cell is successfully included in the design

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/62316c7a-6b01-489d-8100-aa65439036e8)

6. However timing is not met, so it has to be fixed

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f638c9e7-82ec-47f8-ba2a-a1da768d6681)

### Delay tables
Whenever the enable pin is 1, only then the CLK will propogate to Y in case of AND gate and whenever the enable pin is 0, only then the CLK will propogate to Y in case of OR gate, as shown below. When the enable is 1, the CLK will not propagate and there won't be any short circuit power consumption and switching power consumption when such elements are used in clock tree. This method is referred to as the clock gating technique.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/b1cbcbdd-5b2c-4df5-81e6-36ab656db359)

Consider the below clock tree structure.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/0a109fbf-4a2d-4295-9e1d-a252816dc090)

Buffers on different levels have different capacitive loads and buffer sizes but as long as they have the same loads and sizes in the same level, the total delay for each clock tree path will be the same thus skew will remain zero. Practically, different levels can have varying input transition and output capacitive load and hence varying delay.

Delay tables are used to capture the timing model of each cell and is included inside the liberty file. The main factor in delay is the output slew. The output slew depends on capacitive load and input slew. The input slew is a function of previous stage buffer's output capacitive load and input slew and has its own transition delay table.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/e07ed080-fb22-4ceb-868f-f54d42193ebd)

At level 2, both the buffers have identical delays with same transition times, load capacitances, and buffer sizes. Consequently, the skew is maintained at 0.If this is not the case, then the skew will be negative leading to timing violations. While these considerations may seem insignificant when analyzing the delay of just two buffers, their significance is high in designs featuring millions of cells. Failing to adhere to these guidelines during clock tree creation can lead to numerous timing-related complications.

Terminologies:

- CTS is the process of designing a clock distribution network to minimize skew and ensure synchronous operation of the circuit
- Skew refers to the variation in clock signal arrival times
- Latency is the delay experienced by the clock signal
- Slew rate is the rate of change of the signal's voltage over time

### Lab steps to configure synthesis settings to fix slack
Currently,
tns = -711.59
wns = -23.89
Chip area for module picorv32a = 147712.9184

Next step is to see if synthesis can be more timing-driven.

1. Check synthesis strategy and other timing related variables and modify accordingly

   SYNTH_STRATEGY of delay 0 means the tool will focus more on optimizing the delay, index can be 0, 1, 2, or 3 where 3 is the most optimized for timing at the cost of area. 
   SYNTH_BUFFERING of 1 ensures buffer will be used on high fanout cells to reduce wire delay.
   SYNTH_SIZING of 1 will enable cell sizing where cell will be upsized or downsized as needed to meet timing.
   SYNTH_DRIVING_CELL is the cell used to drive the input ports and is vital for cells with a lot of fan-outs since it needs higher drive strength.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/615557e1-6573-450a-ada1-9d5a7537e09a)

2. Run synthesis again and it is seen that area is increased but there is no negative slack

   tns = 0
   wns = 0
   Chip area for module picorv32a = 209181.872

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f6676075-1c12-4aff-8c2b-40aca90bfd9c)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/e2811e5d-6ed0-4163-a3ed-630118795bc6)

3. Run floorplan and placement

   If any error comes related to macro placement, temporary solution is to comment basic_macro_placement inside the OpenLane/scripts/tcl_commands/floorplan.tcl (this is okay 
   since we are not adding any macro to the design).

   (or)

   `init_floorplan`
   
   `place_io`
   
   `global_placement_or`
   
   `detailed_placement`
   
   `tap_decap_or`
   
   `detailed_placement`

   After successful run, runs/[date]/results/placement/picorv32a.placement.def will be created.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/73a2ddfb-55c6-4351-90fe-d59c06521e1b)

   Search for instance of cell sky130_vsdinv inside the DEF file after placement stage: cat picorv32a.placement.def | grep sky130_vsdinv

   Select a single sky130_vsdinv cell instance from the list dumped by grep (e.g. _41096_). On tkcon, command % select cell _41096_ then ctrl+z to zoom into that cell. As shown below, 
   our customized inverter cell sky130_myinverter is sucessfully placed. Use expand on tkon to show the footprint of the cell and notice how the power and ground of sky130_vsdinv 
   overlaps the power and ground pins of its adjacent cells.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/504b1c65-9fd4-4e5a-ac0f-456eb2ffc70c)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/ad7bc9b8-df5b-4e83-bc8e-8d608f30b573)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/d2cea45e-b4a7-45fd-a9d0-62d7867e9dac)


## Timing analysis with ideal clocks using openSTA
### Setup timing analysis and introduction to flip-flop setup time
Consider an ideal clock where clock tree is not built and perform timing analysis to understand the parameters. Later the same can be done using real clocks.
Specifications are as mentioned in the picture. Clock frequncy (F) is 1GHz and clock period (T) is 1ns.

![picc7](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/cfc68179-ec92-46ee-b5d8-4d2418ed36a7)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/5d13fa8b-826c-42ad-84a0-1e0260526f6a)

Setup timing analysis equation is:

`Θ < T - S`

Θ = Combinational delay which includes clk to Q delay of launch flop and internal propagation delay of all gates between launch and capture flop

T = Time period, also called the required time

S = Setup time. As demonstrated below, signal must settle on the middle (input of Mux 2) before clock tansists to 1 so the delay due to Mux 1 must be considered, this delay is the setup time.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/82478661-cce9-428f-9dbc-043f0d520975)

### Introduction to clock jitter and uncertainty
Clock is beign created by PLL (Phase Locked Loop). So, this clock source is expected to send clock signal at 0, T, 2T etc. Even these clock sources might or might not be able to provide a clock exactly at Tns because of its own in-built variation. That is called as the jitter. Jitter can be manifested as short-term fluctuations in the timing of signal transitions, resulting in deviations from the expected clock or data timing.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/43f51267-64c5-465e-b440-448dcc7cfcb4)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/5d9f55ec-e81a-433b-98f0-dee0bd3e8d6f)

So, a more realistic equation for setup time is,

`Θ < T - S - SU`

SU = Setup uncertainty due to jitter which is temporary variation of clock period. This is due to non-idealities of PLL/clock source.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/79ea0309-f621-4b18-a625-ee0bf9625623)

### Lab steps to configure OpenSTA for post-synth timing analysis
STA can either be single corner which only uses the LIB_TYPICAL library which is the one used in pre-layout(pos-synthesis) STA or multicorner which uses LIB_SLOWEST(setup analysis, high temp low voltage),LIB_FASTEST(hold analysis, low temp high voltage), and LIB_TYPICAL libraries.

1. Confiuration file on which we'll be doing pre layout analysis:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/6dd1bcc6-ea6f-4c90-bfc4-2fe3eb37c625)

2. SDC file

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/52371573-303c-42a1-a8ed-42303dab7877)

`create_clock` command creates clock for the port with specified time period. 

`set_input_delay` and `set_output_delay`defines the arrival/exit time of an input/output signal relative to the input clock. This is the delay of the signal coming from an external block and internal delay of the signal to be propagated to external ports. This adds a delay of Xns relative to clk to all signals going to input ports, and delay of Yns relative to clk to all signals going to output ports.

`set_max_fanout` specifies maximum fanout count for all output ports in the design.

`set_driving_cell` models an external driver at the input port of the current design.

`set_load`  sets a capacitive load to all output ports.

3. Execute `sta pre_sta.conf` and check timing.

### Lab steps to optimize synthesis to reduce setup violations
To reduce negative slack, focus on large delays. Net with big fanout might cause delay increase. Use `report_net -connections <net_name>` to display connections. First thing we can do is to go back to OpenLane and reduce fanouts by setting ::env(SYNTH_MAX_FANOUT) 4 then run_synthesis again.

To further reduce the negative slack, we can also try upsizing the cell with high fanout so that bigger driver will be used. High fanout results in high load cap which then results in high delay. Since we cannot change the load capacitance, we can change the cell size to drive large cap load for less delay.

This can be done iteratively until the desired slack is reached, and this is called Timing ECO (Engineering Change Order).



## Clock Tree Synthesis TritonCTS and signal integrity
### Clock tree routing and buffering using H-Tree algorithm
Consider the clock port that goes to the flip-flops highlighted in the picture. The purpose is to connect the port to the clock pins of the flip-flops based on the connectivity information. If we blindly connect as shown in the picture below, then `t2>t1` and the difference `t2-t1` is nothing but the skew. Clock skew refers to the variation in arrival times of the clock signal at different points within a synchronous digital system. In simpler terms, it is the difference in propagation delay experienced by the clock signal as it travels along different paths within the system. Clock skew can occur due to various factors such as differences in wire lengths, variations in signal routing paths, variations in buffer delays, and other physical and environmental factors. These variations can lead to some parts of the system receiving the clock signal earlier or later than others. Minimizing clock skew is essential to ensure proper synchronization of signals and reliable operation of the digital circuit. Ideally, the skew should be zero. 

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/49505f81-562b-4b90-902d-3847d836cca0)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/659f09ec-d4fa-4ce0-bf28-2e05b77ec863)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/ca25ff80-897c-43ab-978d-99d2757791f1)

In the above scenario, the skew is not less/zero, so it is a bad tree. 

H-Tree is the solution. It analyses the clock route by calculating the distance from the source to all the endpoints and deciding on a midpoint to start building tree from that point. In this case, the clock reaches at all the endpoints at almost the same time. 

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/206e5e7f-84c1-4427-a3c1-8081d56d3827)

We expect that whatever input is provided, that should be reproduced at the output. However, due to the inherent resistance and capacitance in physical wires, the signal may experience attenuation or distortion, hindering its proper transmission to the output. To address this, repeaters or buffers are inserted along the path to ensure signal integrity and reliable transmission.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/f3fa47c3-5223-4b5c-a708-5adb6a30685f)

The key difference between repeaters used in clock paths and those used in data paths lies in their rise and fall times. Clock buffers have same rise and fall times, ensuring uniform signal propagation throughout the clock distribution network. In contrast, data buffers exhibit varying rise and fall times, which may differ based on the characteristics of the data being transmitted and the components involved in processing it.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/3c2f3620-5d4d-4802-80e1-aaa4694b65bf)

### Crosstalk and clock net shielding
Clock nets are critical nets in the design because clock tree is built is such a fashion that the skew is zero. There is a phenomenon called crosstalk where a signal transmitted on one channel unintentionally interacts with or interferes with signals on adjacent channels leading to distortion, noise, timing errors etc. If this happens on clock routes, then the clock tree structure will be deteriorated. So all the clock nets are shielded. By shielding, the clock nets are protected. If there is a wire adjacent to such shields, then there exists a huge coupling capacitance cauign two issues. One is glitch and the other is delta delay.

Whenever there is a switching activity happening on the aggressor, the coupling capacitance is so strong that it directly affects the net sitting close to it called the victim net. the victim net is without any shielding. As a result, there is a dip in the voltage, resulting in glitch. 

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/100e788a-444a-40d0-884a-f97e6c61974f)

Shielding basically protects the victim nets by breaking the coupling capacitance between the aggressor and the victim. These shielding nets are either Vdd or Vss. The shields do not switch, so the victim will not switch.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/9d2f9e22-618d-41cc-8cb4-a2dd7a58aea4)

### Lab steps to run and verify CTS using TritonCTS
After ECO of cell sizing, currently the timing is as follows.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/7d8e1210-fd6c-4f0a-a471-d4d92f4dc9e9)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/35edc0ef-f043-4220-aaf2-312d157d37f2)

The slack might increase or decrease as we move forward in the PnR flow. For OpenLANE to use the current netlist,

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/76c46087-be99-4076-8d6a-d7d0fe9ac7b0)

`write_verilog filename` overwrites the current verilog file in the specified location. 

Then,

`run_floorplan`

`run_placement`

Then run cts using the command `run_cts`. Before that we need to check the default setting s that CTS uses.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/fface768-df3f-4732-9bd3-39219969cd66)

In CTS stage, clock buffers get added. 

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/2ccd0ae7-751c-45ff-ad6e-1eb0629c111d)

OpenLANE takes the procs from `~/Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands`. These procedures will then call OpenROAD to run the actual tool.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/8e5d751b-7f40-4ba2-9c2b-5b3482db8419)

For example, run_cts can be found in the file `/OpenLane/scripts/tcl_commands/cts.tcl`, this tcl procedure will call OpenROAD and will call `/OpenLane/scripts/openroad/cts.tcl` which contains the OpenROAD commands to run TritonCTS.

Inside the `/OpenLane/scripts/openroad/cts.tcl` contains the configuration variables for CTS such as:

CTS_CLK_BUFFER_LIST = list of clock buffers used in clock tree branches (sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8)

CTS_ROOT_BUFFER = clock buffer used for the root of the clock tree and is the biggest clock buffer to drive the clock tree of the whole chip (sky130_fd_sc_hd__clkbuf_16)

CTS_MAX_CAP = maximum capacitance of the output port of the root clock buffer

## Timing analysis with real clocks using openSTA
### Setup timing analysis using real clocks
Now the clock tree is built and timing analysis is done on real clocks.

![picc8](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c6d747bb-c8f3-4f2b-999e-0bb32e0f4d57)

delta1 = launch flop clock network delay
delta2 = capture flop clock delay

![picc9](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/9da42c43-00ca-4ca6-a215-4143648b9481)

Any design satisfying `Slack = Data required time - Data arrival time` is ready to work in the given frequency. If this equation is violated , then slack will become negative. We expect slack to be 0 or positive.

### Hold timing analysis using real clocks
Hold analysis refers to the delay/time required by the MUX2 model within the flip-flop to transfer data outside. It denotes the duration during which the launch flop must retain data before it reaches the capture flop. Unlike setup analysis, which spans two rising clock edges, hold analysis occurs on the same rising clock edge for both the launch and capture flops. A hold violation occurs when the path is too fast, impacted by factors including combinational delay, clock buffer delays, and hold time. Notably, parameters such as time period and setup uncertainty hold no significance, as both launch and capture flops receive identical rising clock edges during hold analysis.

![picc10](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/e63ac298-c719-4e56-b920-a9d6a87c5a3e)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/932beb5c-ab46-44db-b157-f81ab419d9ef)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/983e3318-11c2-4619-8ecb-e1b5931132e6)

![picc11](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/8f625763-19d1-418e-8ab1-5707ae931e4d)

`Skew = Launch Clock Network Delay - Capture Clock Network Delay`

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/9eba30d8-e711-4d42-b364-77eb84bcff95)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/acf959e6-46db-45b3-b427-d83a0fac1617)


### Lab steps to analyze timing with real clocks using OpenSTA
The objective is to analyse the clock tree. Entering into `openroad` instead of invoking a separate OpenSTA tool. In openroad, timing analysis is done in a different way, where a db is created from lef & def and used.

1. To create the db, read lef

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/913058dd-a7c0-4069-a2a7-829dedfee996)

2. Read def from cts stage

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/76b2e3f8-11dd-4165-a18a-199db758c48f)

3. Create db

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/6fc84067-e219-49b0-bc69-5aacfac8db5e)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/e5a1f2b0-19f1-42f0-9adc-77d71209c817)

4. Read the db, verilog file, libraries, sdc

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/6a538765-0336-40fc-b9c7-4e124c37b485)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/759ecb62-6298-431c-b853-6a9240fb5c9c)

5. Set propagated clock, it will calculate the actual delay in the clock path.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/50fece02-40dd-4b53-a147-c1c193ce222e)

6. Check timing

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/80ef7290-cfdb-4d65-8395-847cde01da16)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/69f809dd-63ce-4d76-b5c1-c0cc6875695d)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/5544d953-43e6-401f-8810-7ffd8a90ac03)


### Lab steps to execute OpenSTA with right timing libraries
TritonCTS is built to optimise based on one corner but the libraries that are included in the previous section for timing analysis are min and max corners. This kind of analysis is not accurate. So, exit and re-enter openroad and check timing only for typical corner.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/614d93a3-de87-4b37-b29e-4cc4b9eeec6c)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/e961bf62-df57-4282-b951-159fcc31bb64)

In this typical scenario, slack is met in both setup and hold analysis.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/888fa348-0b7f-48fd-bb68-a4649ae42702)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/fa467cae-6911-4cde-8fa3-3e3c48817115)

When CTS is built, skew values is tried to be met by inserting buffers from the CTS_CLK_BUFFER_LIST. We can also modify this list based on requirements.

When TritonCTS is building the clock tree, it tries to use each buffer listed in `$::env(CTS_CLK_BUFFER_LIST) (sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8)` from smallest to largest until the target skew is met. Target skew is stored in `$::env(CTS_TARGET_SKEW)`. The STA result shows that sky130_fd_sc_hd__clkbuf_1 is the mostly used buffer, we can also change the `$::env(CTS_CLK_BUFFER_LIST)` to use other buffers and observe the effect on STA and area.

Use tcl `lreplace` command to modify `$::env(CTS_CLK_BUFFER_LIST)`

Example:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/1c439227-f6f6-47cd-be46-ba299a02c41a)

The `$::env(CURRENT_DEF)` used by CTS is the DEF file of the previously run CTS, but the DEF file we want for CTS is the placement's DEF file. So change the `$::env(CURRENT_DEF)` to point to placement DEF file then `run_cts`.

Observe the resulting post-CTS STA compared to previous run since we modified the clock buffer. Only buf_2 clock buffer is used now compared to buf_1 used in previous run. The WNS is better now since we used bigger clock buffers.




## Final steps for RTL2GDS using tritonRoute and openSTA
## Routing and design rule check (DRC)
### Introduction to Maze Routing
Routing is to find the best possible connection/route between two points. There are many routing algorithms like Steiner Tree algorithm, Line Search algorithm etc. and one such is Maze Routing - Lee's Algorithm (Lee 1961)

Consider and example of connecting two points 1 & 2. Point 1 will act as a source and 2 will act as a target. The requirement is to find the best possible path or the shortest possible path to connect 1 & 2 will less or no zig-zag routes. Mostly the routes are L-shaped. From algorithmic point of view, the software has to search and connect the two points. From physical designer point of view, it is a physical path/wire establishment for signals to travel between components.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/2ec9e7b7-1e47-4840-b3bb-ded148a17f43)

Lee's maze routing algorithm, is a popular pathfinding algorithm used in maze routing, which is a type of routing problem where the goal is to find a path from a source to a destination in a maze-like grid. The Lee algorithm is particularly well-suited for routing on grids or mesh-based structures in integrated circuit design.

Algorithm steps:

1. Initialization: The algorithm starts by initializing a routing grid or matrix representing the maze. Each cell in the grid can be one of several states: obstacle, empty, source, 
   destination, or visited. The source cell is marked with a value of 0, indicating that it is the starting point.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/9cd5ac82-b595-49a1-88fd-3433a10dfc9d)

2. Wave Expansion: The algorithm performs a wave expansion from the source cell, spreading outwards in all directions. At each step, the algorithm examines neighboring cells (up, down, 
   left, and right) and assigns them a value one greater than the minimum value of their neighboring cells (excluding obstacles). This process continues until the destination cell is 
   reached or until no more cells can be visited.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/54b7cb66-6aa0-4a1e-bfaa-1d3ba365b73c)

3. Backtracking and Path Reconstruction: Once the destination cell is reached, the algorithm traces back the path from the destination to the source by following the values in each cell. 
   This results in the shortest path from the source to the destination. There might be multiple paths but the best path that the tool will choose is one with less bends. The route 
   should not be diagonal and must not overlap any blockage/obstruction such as macros or HIPs.

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/3dd24e5c-c6b4-4ada-953e-f723dabf5a17)

Another example:

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/a103d13c-2da7-4455-8df8-5ad9b1872db4)

### Design Rule Check
When routing, it's not merely about connecting two points; we must also adhere to specific rules. These rules, for example, mention that when constructing two wires, there must be a minimum spacing or distance between them, minimum wire width, minimum wire pitch etc. Hence, DRC cleaning is done to ensure the=at the routes can be fabricated and printed in silicon faithfully.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/a693fa58-d18c-4a04-8aa2-234eca10038d)

Signal short is also one of the critical issues as it causes functionality failure. It can be eliminated by moving the route to next layer with vias. This can lead to more DRCs (via width, via spacing, higher metal layer must be wider than lower metal layer etc.).

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/92bfd888-5840-4583-a4db-ce54e7fbdd45)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/e9aec875-786b-4e5f-aec9-11bd58aadd6d)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/6634dafd-41e9-4408-8c1d-a74177ef35c8)


## Power Distribution Network and routing
### Lab steps to build power distribution network

1. Go to openlane directory
2. `docker`
3. `./flow.tcl -interactive`
4. `package require openlane 0.9`
5. `prep -design picorv32a -tag 19-03_16-40` (this is the folder till cts has been done)
6. `echo $::env(CURRENT_DEF)`
   /openLANE_flow/designs/picorv32a/runs/19-03_16-40/results/cts/picorv32a.cts.def
7. To generate PDN: `gen_pdn`

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/c9dc6e7a-4d0c-49e4-9219-ce4922c05316)

   ![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/bfb1bd29-f365-4a81-8358-a68f79f2dba7)

### Lab steps from power straps to std cell power
The power and ground rails have a pitch of 2.72um and hence the reason reason why the custom inverter cell has a height of 2.72um, else the power and ground rails will not be able to power the cell. Looking at the LEF file `runs/[date]/tmp/merged.lef`, it is noticed that all cells are of height 2.72um and only width differs.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/00ce451a-408f-4b9b-a35c-9485c75673ca)

As shown below, power/ground pads -> power/ground ring-> power/ground straps -> power/ground rails to power up the standard cells

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/629141e5-4b0b-42b4-a672-db6cc2658027)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/68861381-09c0-47f7-8176-0ba72697fae9)

### Basics of global and detail routing and configure TritonRoute
TritonRoute is the engine that is used for routing. `run_routing` command does routing in OpenLANE.

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/59e0191f-e04b-4a84-a18b-27ddbc2fb081)

![image](https://github.com/dsanthak/NASSCOM-VSD-SoC-Design/assets/163589731/3f466947-0553-4432-90f0-482885747c42)

In the VLSI flow, the routing stage is highly critical and can be executed using either open-source or commercial tools. This stage is divided into two phases:

1. Global Route / Fast Route:
   - This is accomplished using fast routing techniques where the area to be routed is partitioned into tiles or rectangles. Global routing establishes the initial framework for routing paths.

2. Detail Route:
   - This phase involves meticulous tracking routing techniques to complete the routing process. Detailed routing fine-tunes and finalizes the paths to ensure proper connectivity and compliance with design constraints.

## TritonRoute features
