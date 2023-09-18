# IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130

This project was completed as part of the "Advanced Physical Design using OpenLANE/Sky130" course by VLSI System Design Corporation. It encompasses the entire RTL to GDSII flow for the PicoRV32a System-on-Chip (SoC) using the OpenLANE toolchain with the Skywater130nm Process Design Kit (PDK). In addition to the standard flow, custom-designed standard cells within the Sky130 PDK were integrated, timing optimizations were performed to meet design constraints, slack violations were rectified, and DRC (Design Rule Check) was rigorously verified.

## Table of Contents
- [Software Installation](#software-installation)
- [Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1-inception-of-open-source-eda-openlane-and-sky130-pdk)
- [Day 2: Good floorplan vs bad floorplan and introduction to library cells](#Day-2-good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
- [DAY 3: Design a Library Cell using Magic Layout and Ngspice Characterization](#day-3-design-a-library-cell-using-magic-layout-and-ngspice-characterization)
- [DAY 4: Pre-layout Timing Analysis and Importance of Good Clock Tree](#day-4-pre-layout-timing-analysis-and-importance-of-good-clock-tree)
- [DAY 5: Final Steps for RTL2GDS using TritonRoute and OpenSTA](#day-5-final-steps-for-rtl2gds-using-tritonroute-and-opensta)
- [Acknowledgements](#acknowledgements)

## Software Installation
### Step 1:
To install OpenLANE on Ubuntu, use the following command in the terminal:

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt install -y build-essential python3 python3-venv python3-pip make git
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt update
$ sudo apt install docker-ce docker-ce-cli containerd.io
$ sudo docker run hello-world
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ sudo reboot
```

After Reboot
```bash
docker run hello-world
```
### Step 2:
```bash
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
```
Note: The make command compiles and builds the OpenLane tools, and make test runs tests to ensure that the build is successful and that the tools work as expected.

# Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK

## Table of Contents:
- [Overview](#overview)
- [Inception of Open-Source EDA](#inception-of-open-source-eda)
  - [How to Talk to Computers?](#how-to-talk-to-computers)
  - [Components of Open-Source Digital ASIC Design](#components-of-open-source-digital-asic-design)
  - [Process Design Kit](#process-design-kit)
- [SoC Design & OpenLANE](#soc-design-and-Openlane)
- [Simplified RTL2GDS Flow](#simplified-rtl2gds-flow)


## Overview

OpenLANE is a powerful open-source tool and flow designed for open-source tape-outs. This versatile flow integrates a variety of essential tools, including Yosys, ABC, OpenSTA, Fault, OpenROAD app, Netgen, and Magic. Its primary purpose is to harden chips and macros, ultimately generating the final GDSII layout from the initial design RTL (Register-Transfer Level). The overarching objective of OpenLANE is to achieve a seamless and fully automated process, eliminating the need for extensive human intervention. It's noteworthy that OpenLANE is optimized for compatibility with the Google-Skywater130 Open Source Process Design Kit (PDK).

## Inception of Open-Source EDA

### How to Talk to Computers?
To understand the significance of open-source EDA, it's crucial to grasp the fundamental process of communicating with computers. The RISC-V Instruction Set Architecture (ISA) serves as the language to interact with computers whose hardware is based on RISC-V cores. When a user intends to run a specific application software on a computer, the corresponding program written in languages like C, C++, or Java must first be translated into instructions by a compiler. These compiler-generated instructions are inherently hardware-dependent. They serve as inputs to the assembler, which in turn produces binary code. This binary code is what the hardware logic within the chip layout comprehends. Depending on the received bits, the digital logic, composed of gates, performs the necessary functions as required by the user of the application software.

![Screenshot 2023-09-08 191119](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/acdad48c-201b-4e84-9e18-08a5cfb7eac8)
![Screenshot 2023-09-08 191337](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/2eb17a6d-078c-4ea5-9d84-93b736846a59)

### Components of Open-Source Digital ASIC Design:

#### Foundry IP Blocks and Macro Blocks:

Foundry IP Blocks: These are blocks such as ADC (Analog-to-Digital Converter), DAC (Digital-to-Analog Converter), PLL (Phase-Locked Loop), and SRAM (Static Random-Access Memory). They are designed by foundries and often contain analog components.
Macro Blocks: These are pure digital logic blocks like RISC-V SoCs (System-on-Chips) and SPI (Serial Peripheral Interface) controllers. They do not typically contain analog parts.

![Screenshot 2023-09-08 191359](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/8614c949-4432-4125-8eb8-a00c466ef5d0)

#### Open-Source Components:

RTL Designs: These are digital designs at the Register Transfer Level (RTL) available from open-source platforms like GitHub (github.com), LibreCores (librecores.org), and OpenCores (opencores.org).

EDA Tools (Electronic Design Automation): Open-source EDA tools play a crucial role in the design flow. Examples include OpenROAD, OpenLANE, and QFlow, which automate various stages of chip design.

![Screenshot 2023-09-09 152218](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/3eda8d5f-e7da-4782-b9a9-88e65ea05283)

PDK (Process Design Kit): The PDK serves as the interface between the chip designer and the fabrication process. It includes essential data files and documents, such as cell libraries, IO libraries, and design rules (DRC, LVS, etc.).

### Process Design Kit

A Process Design Kit (PDK) is a collection of essential data and information used in semiconductor chip design. It provides designers with details about the manufacturing process, including the properties of various components, design rules, and guidelines needed to create custom integrated circuits. PDKs serve as a bridge between chip designers and semiconductor foundries, ensuring that designs are compatible with the fabrication process.

The collaboration between Google and SkyWater Technology involves the development and release of an open-source Process Design Kit (PDK) specifically tailored for the SkyWater 130nm semiconductor manufacturing process. Here's a breakdown of this collaboration:

#### Google and SkyWater Technology:

- Google: A multinational technology company known for its internet-related products and services.

- SkyWater Technology: A semiconductor foundry specializing in providing access to advanced semiconductor manufacturing technologies.

#### SkyWater 130nm Process:

- The SkyWater 130nm process refers to a specific semiconductor manufacturing process offered by SkyWater Technology.
-  It denotes the technology used to fabricate integrated circuits with a feature size of 130 nanometers.

![Screenshot 2023-09-09 152200](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/c2e11272-f744-4307-912c-2cb21dd533d2)
![Screenshot 2023-09-09 152125](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/67ecef1a-8e67-4fbd-8d71-d92c4166b563)
  
#### PDK (Process Design Kit) Details:

- Cell Libraries: These contain predefined digital and analog cells that designers can use in their custom ICs.
IO Libraries: These define the input and output characteristics of the chip's pins.
- Design Rules (DRC, LVS, etc.): These documents specify constraints and criteria that must be followed to ensure proper chip functionality and manufacturability.
- Open-source digital ASIC design combines these components to enable cost-effective and accessible custom chip creation while leveraging the collaborative efforts of the open-source community and utilizing established design and fabrication standards.

![Screenshot 2023-09-09 183613](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/5bf530f3-c1e4-4cac-b5e8-3e23e1b896bc)


## SoC Design and OpenLANE

System-on-Chip (SoC) design often involves integrating multiple IPs (Intellectual Properties) onto a single chip.
OpenLANE, an open-source EDA tool, is useful for designing SoCs by automating the chip design process.

![Screenshot 2023-09-09 150652](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/02234a49-e95e-4a21-8141-ba42eaa224c4)


### Components of Open-Source Digital ASIC Design:

![Screenshot 2023-09-09 143356](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/7d919b51-af23-49e8-9ae3-a2c71844e6be)

#### Opensource RTL Designs:

- RTL IPs (Resistor Transistor Logic Intellectual Property) are essential building blocks for digital ASICs.
- They can be sourced from open repositories such as GitHub, LibreCores, and OpenCores.

 #### Opensource EDA Tools:

- EDA (Electronic Design Automation) tools are software applications that facilitate chip design.
- Open-source EDA tools like QFlow, OpenROAD, and OpenLANE automate various design stages, making ASIC design accessible.

#### Opensource PDK Data:

- PDK (Process Design Kit) data is crucial for interfacing with semiconductor fabrication foundries.
- Open-source PDK data, such as the Google Skywater130 PDK, provides the necessary process information for chip manufacturing.

## Simplified RTL2GDS Flow

![Screenshot 2023-09-09 152300](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/d28d8fae-22a1-44fd-b592-a281fc13085f)
![Screenshot 2023-09-10 113239](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/42d404b7-fc8c-42d7-8c55-74c338d67762)

The RTL-to-GDSII (Register Transfer Level to Graphic Data System II) flow is a crucial process in integrated circuit (IC) design that involves transforming a high-level RTL description of a digital design into a final layout in GDSII format, which is the file format used for manufacturing semiconductor devices. This flow typically consists of several key steps, which I'll document in more detail:

Synthesis:

- Objective: Convert RTL (Register Transfer Level) design into a gate-level netlist using standard cell libraries (SCL).
- Description: During this phase, the RTL code, written in a hardware description language like VHDL or Verilog, is synthesized into a gate-level representation. This involves mapping the RTL constructs into specific standard cells available in the chosen library. The output is a gate-level netlist.

![Screenshot 2023-09-10 104354](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/4bce7439-b48b-4d65-b8fe-7945c01d05db)
![Screenshot 2023-09-10 104421](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/39881f8a-7fc8-44ee-a162-06fe11a01437)

Floor & Power Planning:

- Objective: Plan the silicon area and ensure robust power distribution.
- Description: In this step, the chip's physical floorplan is established. This includes determining the location of various functional blocks, I/O pads, and power distribution networks. Careful floor planning is essential to optimize the use of silicon area and ensure efficient power distribution to all parts of the chip.

![Screenshot 2023-09-10 104558](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/c46af752-4e82-498b-99f7-f7f8490787e2)
![Screenshot 2023-09-10 104447](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/ba0f3d7e-8dc7-47d9-b8d8-7519a3cfc0f8)

Placement:

- Objective: Place cells on floorplan rows aligned with predefined sites.
- Description: The synthesized gate-level cells are placed onto the chip's floorplan. These placements are typically aligned with rows and sites specified by the technology library. The goal is to achieve good utilization of the available area and minimize wirelength for better performance.

![Screenshot 2023-09-10 105323](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/288496f3-1d43-4bed-b748-129cc11b6dbc)

Global Placement:

- Objective: Optimize the initial placement of cells for better overall performance.
- Description: After the initial placement, a global placement algorithm is applied to optimize the positions of cells. This optimization considers factors such as timing, power, and area. The goal is to find an optimal arrangement that meets design constraints.

Detailed Placement:

- Objective: Refine cell positions to ensure legal placements and meet design rules.
- Description: Detailed placement fine-tunes the positions of individual cells to ensure they meet legal placement constraints and adhere to design rules. This step helps to further optimize timing, area, and power.

![Screenshot 2023-09-10 105341](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/25a76118-8efb-4676-a69a-a4141dd98e94)

Clock Tree Synthesis (CTS):

- Objective: Distribute the clock signal effectively to all flip-flops in a chip.

- Description: Clock Tree Synthesis (CTS) is the process of creating a structured network of clock distribution to ensure that the clock signal reaches all flip-flops with minimal skew. Typically designed as a tree-like structure, CTS optimizes the placement of buffers and routing to minimize clock skew, ensuring synchronous operation and preventing timing issues in digital integrated circuits.

![Screenshot 2023-09-10 105607](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/d7f0f47c-c49a-46d5-bf53-022d7c689d36)

Routing:

- Objective: Create valid patterns for connecting wires between cells.
- Description: Routing involves the creation of metal interconnects (wires) that connect the outputs of one cell to the inputs of another. The routing process must adhere to design rules to avoid issues like short circuits and timing violations. Various routing algorithms and techniques are used to achieve this.

![Screenshot 2023-09-10 105956](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/484bd0c2-f3b0-4d7b-bda2-a44bded8afb0)
![Screenshot 2023-09-10 105940](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/62f93708-168f-4945-a118-6ca15070403c)

Signoff:

- Objective: Perform physical and timing verifications to ensure design correctness.
- Description: The final step involves thorough verification of the physical design. This includes Design Rule Checking (DRC) to ensure that layout adheres to manufacturing rules, Layout vs. Schematic (LVS) checks to verify consistency between the layout and the netlist, and Static Timing
Analysis (STA) to ensure that timing constraints are met.

![Screenshot 2023-09-10 112410](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/2e3d7df6-60e0-464b-b956-479297e4c15f)

Once all these steps are completed successfully, the output of the RTL-to-GDSII flow is a GDSII file that contains the physical layout of the integrated circuit. This GDSII file is then used in the semiconductor manufacturing process to produce the actual ICs.

## OpenLANE

![Screenshot 2023-09-10 112715](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/6a5c13a7-1a78-45c7-93b5-42f7e2d1f7bd)

OpenLane is indeed an open-source ASIC (Application-Specific Integrated Circuit) development flow reference designed for use with the Sky130 PDK (Process Design Kit) and also compatible with the OSU 130nm process. It offers a comprehensive set of open-source tools that enable the entire RTL (Register-Transfer Level) to GDSII (Graphic Data System II) flow for ASIC design. Below, I'll provide more details about the various stages of the flow and address the issue of Antenna Rules Violation.

### Open-Source EDA Tools Used in OpenLANE:



| EDA Tool                    | Task                                                         |
|-----------------------------|--------------------------------------------------------------|
| **RTL Synthesis & Technology Mapping** |                                                              |
| yosys                       | Performs RTL synthesis.                                     |
| abc                         | Performs technology mapping.                                |
| **Floorplan & Power Distribution Network (PDN)** |              |
| init_fp                     | Defines the core area, rows (for placement), and tracks (for routing). |
| ioPlacer                    | Places macro input and output ports.                        |
| pdn                         | Generates the power distribution network.                   |
| tapcell                     | Inserts welltap and decap cells in the floorplan.          |
| **Placement**               |                                                              |
| RePLace                     | Performs global placement.                                  |
| Resizer                     | Performs optional optimizations on the design.               |
| OpenDP                      | Performs detailed placement to legalize the globally placed components. |
| **Static Timing Analysis**   |                                                              |
| OpenSTA                    | Performs static timing analysis on the resulting netlist to generate timing reports. |
| **Clock Tree Synthesis**    |                                                              |
| TritonCTS                   | Synthesizes the clock distribution network (the clock tree). |
| **Routing**                 |                                                              |
| FastRoute                   | Performs global routing to generate a guide file for the detailed router. |
| CU-GR                       | Another option for performing global routing.              |
| TritonRoute                 | Performs detailed routing.                                  |
| **SPEF Extraction**         |                                                              |
| SPEF-Extractor              | Performs SPEF (Standard Parasitic Exchange Format) extraction. |
| **GDSII Generation**        |                                                              |
| Magic                       | Streams out the final GDSII layout file from the routed DEF (Design Exchange Format). |
| Klayout                     | Streams out the final GDSII layout file from the routed DEF as a backup. |
| **Checks**                  |                                                              |
| Magic                       | Performs DRC (Design Rule Check) Checks and Antenna Checks.   |
| Klayout                     | Performs DRC Checks.                                         |
| Netgen                      | Performs LVS (Layout vs. Schematic) Checks.                   |
| CVC                         | Performs Circuit Validity Checks.                            |


### OpenLANE Design Stages:

![Screenshot 2023-09-10 113826](https://github.com/akhiiasati/Akhil_IIITB/assets/43675821/7d852f8f-9214-4db5-826d-8cc88af1fe91)

OpenLANE's ASIC design flow is divided into several stages, each involving the use of specific EDA tools:

- Synthesis: RTL synthesis using yosys, technology mapping using abc, and static timing analysis using OpenSTA.
- Floorplan and PDN: Defines core area, places macro input and output ports, generates the power distribution network, and inserts welltap and decap cells.
- Placement: Global placement using RePLace, optional optimizations using Resizer, and detailed placement to legalize globally placed components using OpenDP.
- CTS (Clock Tree Synthesis): Synthesizes the clock distribution network using TritonCTS.
- Routing: Global routing using FastRoute or CU-GR as an alternative option, and detailed routing using TritonRoute.
- SPEF Extraction: Extracts SPEF information.
- GDSII Generation: Generates the final GDSII layout file using Magic and Klayout.
- Checks: Performs DRC (Design Rule Check) checks, antenna checks, LVS checks using Netgen, and circuit validity checks using CVC.

### OpenLANE Files Structure:

The file structure for OpenLANE typically includes the following directories:

skywater-pdk: Contains Process Design Kit (PDK) files provided by the foundry.
open_pdks: Contains scripts to set up PDKs for open-source tools.
sky130A: Contains Sky130 PDK files, which are specific to the SkyWater foundry's 130nm process.

### To invoke OpenLANE and prepare a design:

```bash
cd OpenLane &&  # Navigate to the OpenLane directory
make Mount &&   # Set up the environment
./flow.tcl -interactive &&  # Start an interactive session with OpenLANE
package require openlane 0.9 &&  # Specify the OpenLANE version (0.9)
prep -design picorv32a  # Prepare the "picorv32a" design
```

This single line of code performs each step in the sequence:
1. Navigates to the OpenLane directory.
2. Sets up the environment with make Mount.
3. Starts an interactive session with OpenLANE using ./flow.tcl -interactive.
4. Specifies the desired OpenLANE version (0.9) with package require openlane 0.9.
5. Prepares the "picorv32a" design using prep -design picorv32a.

![Screenshot 2023-09-15 191632](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/c90b98bc-f2a7-469e-93f1-f1041c88182e)

#### Please note that these steps assume that you have Docker installed and configured on your system and that you have the necessary OpenLANE and design files set up in your environment.

In OpenLane, configuration settings for ASIC designs follow this priority order:

1. sky130_xxxxx_config.tcl in OpenLane/designs/[design]/: Design-specific settings, if available.
2. config.tcl in OpenLane/designs/[design]/: Secondary design-specific settings if the first file is missing.
3. Default values in OpenLane/configuration/: General default settings used if no design-specific configurations are provided.

#### Review of files & Synthesis step

1. Folder Generation: A "runs" folder is generated within the "picorv32a" folder.

2. Merged File Creation: During the merging operation in the picorv32a design preparation, a merged file is created by merging the LEF (Library Exchange Format) and TECHLEF (Technology Library Exchange Format) files.

3. Synthesis Execution: The synthesis of the picorv32a design is initiated within the openlane interactive terminal using the command:

```bash
run_synthesis
```

or alternatively:

```bash
openlane -placement -run_synthesis
```

![Screenshot 2023-09-15 232347](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/8096bb50-0989-4701-bf91-63f702cb8c17)

4. RTL to Gate Level Conversion: The Yosys and ABC tools are employed for the conversion of RTL (Register Transfer Level) design to a gate-level netlist.

5. Flop Ratio Calculation:

Flop ratio is calculated using the formula:

```mathematica
Flop ratio = (Number of D Flip flops) / (Total Number of cells)
```

Given values:
```mathematics
Number of D Flip flops (dfxtp_4): 1613
Total Number of cells: 14876
Flop ratio = 1613 / 14876 = 0.1084, which is equivalent to 10.84%.
```

6. Synthesis Step Verification: The success of the synthesis step can be verified by checking the synthesis folder for the synthesized netlist file, typically having a ".v" file extension.

7. Synthesis Statistics Report: The synthesis statistics report is accessible within the "reports" directory. It is typically the last Yosys-generated file, as files are usually listed chronologically based on the date of modification.


8. Synthesis Timings Report: The synthesis timings report, which provides insights into the time taken for the synthesis process, should be documented. However, specific details about the timing report are not provided in the text.

![Screenshot 2023-09-15 233747](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/47c5253d-dabd-4ec7-bf56-ffdeea5979db)

9. Synthesis Power Report: The Synthesis Power Report is a vital document generated during the synthesis phase of digital integrated circuit design. This report offers critical insights into the power consumption characteristics of the synthesized design.

![Screenshot 2023-09-16 003301](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/21be8ad6-2c44-45a5-8639-516447a95255)

# Day 2: Good floorplan vs bad floorplan and introduction to library cells

- [Chip Floor planning considerations](#chip-floor-planning-considerations)
  - [Utilization Factor & Aspect Ratio](#utilization-factor-&-aspect-ratio)

## Chip Floor planning considerations

### Utilization Factor & Aspect Ratio

Two critical parameters play a pivotal role in floorplanning: the Utilization Factor and the Aspect Ratio. They are defined as follows:

### Utilization Factor:

The Utilization Factor is a metric that quantifies how efficiently the available core area of the chip is used by the netlist, which represents all the interconnected components of the IC.

```mathematics
                         Area occupied by netlist
Utilisation Factor =  ------------------------------
                            Total area of core      
```
#### Significance:

- A Utilization Factor of 1 indicates 100% utilization, meaning that the entire core area is occupied by the netlist, leaving no room for additional components or routing.

- In practical IC design, the Utilization Factor is typically in the range of 0.5 to 0.6, as it's important to leave space for buffer cells, routing wires, and layout flexibility.

### Aspect Ratio:

The Aspect Ratio of a chip describes the geometric shape of the chip and is defined as the ratio of its height to its width.

```mathematics
                    Height 
Aspect Ratio =  --------------
                    Width   
```
#### Significance:

- An Aspect Ratio of 1 implies that the chip is square-shaped, where the height and width are equal.
- Any value other than 1 indicates a rectangular chip, where one dimension (height or width) is greater than the other.
- The choice of Aspect Ratio can impact the chip's layout, area efficiency, and ease of routing signals between components.

In practical IC design, finding the right balance between maximizing resource utilization (Utilization Factor) and accommodating the chip's specific requirements or constraints (Aspect Ratio) is essential. Chip designers must consider factors such as signal integrity, manufacturability, and overall performance when making decisions regarding these parameters.

#### Examples:

![Screenshot 2023-09-16 121218](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/bc2650db-e401-412d-9293-bc33dbd39a5b)

![Screenshot 2023-09-16 155207](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/78aab7fd-c3f7-427b-959f-572ae0142a9b)

### Pre-placed Cells
After determining the Utilization Factor and Aspect Ratio, the next step is defining the positions of pre-placed cells. Pre-placed cells are intellectual properties (IPs) consisting of substantial combinational logic and maintain a fixed position once placed. They are strategically positioned in the chip layout before the placement and routing phases, hence their name, pre-placed cells.

![Screenshot 2023-09-16 160203](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/8c6301de-de7e-4f46-b5f3-9afdf80361f7)
![Screenshot 2023-09-16 160630](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/b37d5d9c-52a6-4718-8e9d-84057618d921)
![Screenshot 2023-09-16 160651](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/e71049f6-ebea-426f-bc90-81a8e72ec934)

![Screenshot 2023-09-16 161751](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/c6644f78-0098-4435-a074-fc8fefd1d229)

### Decoupling Capacitors
Pre-placed cells must be surrounded by decoupling capacitors (decaps). The resistance and capacitance introduced by long wire connections can cause a significant drop in the power supply voltage before reaching the logic circuits. This voltage drop can push the signal into an undefined range, outside the noise margin. Decaps are large capacitors charged to the power supply voltage and positioned in proximity to the logic circuitry. Their primary purpose is to decouple the circuit from the power supply by providing the necessary current. This action prevents crosstalk, stabilizes the voltage, and enables local communication within the circuit.

![Screenshot 2023-09-16 162816](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/df2575f0-1a59-46f1-8b55-3e2c00041d8c)
![Screenshot 2023-09-16 162823](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/6112cecc-a80e-4854-b1ba-0967f8144f41)
![Screenshot 2023-09-16 162839](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/579186ae-5aa5-4e83-9b20-f52443935f26)
![Screenshot 2023-09-16 162912](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/0922ae66-875b-442b-a7c8-17cfc2f61a38)
![Screenshot 2023-09-16 162929](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/f54646ab-500c-4cde-a2c5-8d047904b6ac)

### Power Planning
While pre-placed macros can have dedicated decoupling capacitors, it is not practical to provide each block on the chip with its own decaps. Instead, effective power planning is crucial. Power planning ensures that each block has access to dedicated VDD (power) and VSS (ground) pads, which are connected to the horizontal and vertical power and ground lines forming a power mesh. This network of power and ground lines helps distribute power uniformly across the chip, minimizing voltage drops and ensuring that each block receives a reliable power supply.

![Screenshot 2023-09-16 165941](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/5c24e8a7-66c3-44b7-806c-2ff004a8b19e)
![Screenshot 2023-09-16 170012](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/405e01ac-e5a1-4d07-9945-e2e318d9e100)
![Screenshot 2023-09-16 170054](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/9e30d909-9779-40d6-8bae-3cb1fc28515a)
![Screenshot 2023-09-16 170111](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/7a4f0583-aa8f-4730-aae3-b90ac4027d25)
![Screenshot 2023-09-16 170155](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/41d76cf9-4325-4633-ac41-f7c15c81b00f)
![Screenshot 2023-09-16 170210](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/d5893532-cfee-4516-a604-bca370aed1d0)

### Pin Placement

The netlist serves as the blueprint for establishing connections between different logic gates. Within the designated space between the core and die of the integrated circuit (IC), I/O pins are strategically placed. This positioning of I/O pads is determined by the connectivity details specified in VHDL or Verilog code. Following this, a deliberate placement strategy is employed to block out areas for pre-placed macros, ensuring a clear distinction between the macro and pin regions.

![Screenshot 2023-09-16 172415](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/7ce89903-d8a4-4e67-902d-60986692d1d3)

#### Floorplan in OpenLANE: To run a floorplan in OpenLANE and view it in Magic

- Importance Files (Increasing Priority Order):

1. ```floorplan.tcl``` - Contains system default environment variables for the floorplan process.
2. ```config.tcl``` - General configuration settings for the ASIC design.
3. ```sky130A_sky130_fd_sc_hd_config.tcl``` - Specific configuration for the SKY130 process technology.

- Floorplan Environment Variables (Switches):

1. ```FP_CORE_UTIL``` - Specifies the desired core utilization in the floorplan, indicating how much of the core area should be occupied.
2. ```FP_ASPECT_RATIO``` - Defines the aspect ratio (width-to-height ratio) of the floorplan, which affects the physical layout.
3. ```FP_CORE_MARGIN``` - Sets the margin area between the core of the ASIC and the die boundary, allowing space for various purposes.
4. ```FP_IO_MODE``` - Determines pin configurations. A value of 1 typically represents equidistant pin placement, while 0 indicates non-equidistant pin placement.
5. ```FP_CORE_VMETAL``` - Specifies the vertical metal layer used for routing signals within the core.
6. ```FP_CORE_HMETAL``` - Specifies the horizontal metal layer used for signal routing.

Note: In practice, the values for vertical and horizontal metal layers (FP_CORE_VMETAL and FP_CORE_HMETAL) may often be one more than what's specified in the files due to conventions or indexing.

![183446309-a0714ec5-0619-4327-bdfe-890c19cc97e0](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/b8bf7aa4-4366-4d1b-b647-c53ab7f04a7b)

To run the Picorv32a floorplan in OpenLANE, you can simply use the following command:

```bash
run_floorplan
```
![Screenshot 2023-09-16 194146](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/af252eb6-af92-4ad8-8c18-fa3942e341a3)

This command will initiate the floorplanning process for your ASIC design using the Picorv32a core.

After running the floorplan in OpenLANE:

- Locate the ```.def``` file in results/floorplan.
- Review floorplan configurations in ```floorplan.tcl```, overridden by settings in ```config.tcl``` and ```sky130A_sky130_fd_sc_hd_config.tcl```.
- To view the floorplan in Magic, navigate to the results/floorplan directory and use this command:

```bash
magic -T ~/vsdstdcelldesign/libs/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &
```
- Ensure the path is correct and points to your .def file.

![Screenshot 2023-09-16 211057](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/53acc47e-a0e5-473a-bd86-bc6e1351988d)

- You can zoom in the Magic layout by selecting an area with a combination of mouse clicks (left and right) and then pressing the "z" key.
- To identify components, simply select them in the layout and use the "what" command in the tkcon window.
- Zooming in also reveals the presence of decoupling capacitors (decaps) within the Picorv32a chip.
- You'll typically find the standard cell located in the bottom left corner of the layout.

![Screenshot 2023-09-16 225620](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/20a6b7fc-c888-4ac1-8839-5ce008609dcb)

![Screenshot 2023-09-16 225805](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/b5881522-971a-4696-9d8e-4e306ccd1d33)

### Placement
In the OpenLANE ASIC flow, the placement step is a crucial stage in designing an integrated circuit.

#### Placement Optimization

![Screenshot 2023-09-17 120544](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/0216db6d-83d6-4bd8-9e24-7f0b5b71cfd7)

Placement optimization in design refers to the process of strategically positioning logic cells and components on a silicon die to achieve specific design goals. This optimization occurs in two main stages:

1. Global Placement:

- During global placement, an attempt is made to find an initial, optimal position for all cells in the circuit, even if it results in some illegal placements where cells may overlap.
- The primary optimization objective is to minimize the total wire length or half parameter wire length, which helps improve the overall performance of the design.

2. Detailed Placement:

- After global placement, the position of cells is adjusted to ensure they comply with legal placement constraints.
- Legalization is necessary to meet timing requirements and to ensure that the design is manufacturable.

#### Running Placement in OpenLANE and Viewing in Magic:

To run the placement stage in OpenLANE and visualize the placement results in Magic, you can use the following command:

```bash
run_placement
```

This command initiates the placement process, optimizing the initial cell positions to achieve legality and meet timing constraints. After running this command, you can view the placement in Magic to assess the physical layout of your integrated circuit.

![Screenshot 2023-09-17 001745](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/ac6a4fad-dc83-4f38-8a15-5ebe35c206d0)

The main objective of the placement stage in ASIC design is to achieve the convergence of the overflow value. A decreasing overflow value during the placement run indicates that the design is gradually converging, and this is a positive sign that the placement process is progressing successfully.

Once the placement is completed, you can visualize the design using the Magic layout tool. To view the design, navigate to the `results/placement` directory and use a command like the following:

```bash
magic -T /home/akhilasati/vsdstdcelldesign/libs/sky130A.tech lef read /home/akhilasati/OpenLane/designs/picorv32a/runs/RUN_2023.09.16_18.33.33/tmp/merged.max.lef def read picorv32.def &
```
This command invokes Magic with the specified technology library and reads in the `LEF (Library Exchange Format) file` and the `.def (Placement) file` to visualize the placement and layout of the `Picorv32a` chip. The ability to view the design in Magic is crucial for verifying the correctness and quality of the placement results.

![Screenshot 2023-09-17 001531](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/75d33109-3fb4-41dd-9e4d-048e5d6b2c9c)

Zoomed-in views of the standard cell placement:

![Screenshot 2023-09-17 001601](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/830a376a-dad5-4aa7-b982-cf93033177a2)

Note: In many ASIC design flows, the generation of the Power Distribution Network (PDN) is traditionally included as part of the floorplan step. However, it's important to clarify that in the openLANE flow, the PDN is not generated during the floorplan step. Instead, the sequence of steps is floorplan, placement, Clock Tree Synthesis (CTS), and then PDN generation.

### Library Characterization

![Screenshot 2023-09-17 120714](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/2422b339-12f1-41a9-b45e-0288fecce19d)
![Screenshot 2023-09-17 121125](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/c2f2a98d-d31a-4cef-8381-c91761450706)

Library Characterization in the ASIC design process involves creating reliable building blocks, such as standard cells, for chip designs.Here's a summarized overview of the process:

#### Input Sources:

- Utilizes data from libraries containing standard cells, macros, IPs, and decaps.
- These libraries offer various cell flavors with different characteristics, like size, delays, and threshold voltages.
- Larger cell sizes provide higher drive strength for longer and thicker wires, but larger threshold voltages result in slower switching speeds (slower clock).

#### Cell Design Flow Inputs:

![Screenshot 2023-09-17 121205](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/d06613c4-9e70-40e0-be91-58d0643eeb6b)
![Screenshot 2023-09-17 121150](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/8294fdc4-b52c-456a-92a0-1c03cf4c6466)


Obtains inputs from the foundry's Process Design Kits (PDK):
- DRC & LVS Rules, including tech files and poly substrate parameters.
- SPICE Models, specifying threshold, linear regions, saturation region equations, and foundry-specific parameters for NMOS and PMOS transistors.
- User-Defined Specs, including cell height, cell width (based on drive strength), supply voltage, and metal layer requirements.

#### Design Process:

- Ensures that library cell development adheres to input rules to prevent errors when used in actual designs.
- The cell design flow consists of:
  - Circuit Design, which defines the cell's logical function using a circuit design language (CDL).
  - Transistor Modeling, where PMOS and NMOS transistors are sized to meet library requirements.
  - Layout Design, optimizing the cell's physical layout for area efficiency, often using tools like the Magic layout tool.

#### Outputs:
Produces critical outputs:
- `GDSII:` The layout file in GDSII format.
- `LEF:` Defines the cell's width and height for placement and routing.
- `Extracted SPICE Netlist (.cir):` Includes parasitic elements (resistance, capacitance) for circuit simulations.

![Screenshot 2023-09-17 134514](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/bcd6effd-4dc8-458c-9e47-c857ff15da52)
![Screenshot 2023-09-17 134809](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/ad227879-dacd-4bb7-b94b-1f808c0f8078)
![Screenshot 2023-09-17 134832](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/e0043aa2-6745-4400-96f2-fa95f81fca7f)
![Screenshot 2023-09-17 134751](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/27a2197b-db9c-4934-b852-c122443f5df3)

#### Characterization:

- Characterizes the library cell's behavior and performance using tools like GUNA software.
- Outputs include timing, noise, and power models, ensuring the cell meets design specifications.

Library Characterization is an essential step in ASIC design, providing reliable building blocks for creating complex and efficient digital circuits.

![Screenshot 2023-09-17 135258](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/5fc97700-826b-4430-b4fd-4ef3be9f4845)
![Screenshot 2023-09-17 135311](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/44f97991-eb96-48ff-84b2-247f81df0507)

### General timing characterization parameters

![Screenshot 2023-09-17 135341](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/2698ca58-b1e8-466e-ac0e-9fc8b32877c9)


In digital timing analysis, various timing parameters and threshold values are used to determine the behavior of digital signals as they transition between logic levels. Here are the definitions for the provided timing parameters:

`slew_low_rise_thr:` This is the threshold value at which the signal is considered to be transitioning from a low level to a rising edge. It's set at 20% of the signal's voltage range.

`slew_high_rise_thr:` This threshold value represents the point at which the signal is transitioning from a high level to a rising edge. It's set at 80% of the signal's voltage range.

`slew_low_fall_thr:` Similar to the first parameter, this threshold represents the point at which the signal transitions from a low level to a falling edge. It's also set at 20% of the voltage range.

`slew_high_fall_thr:` This threshold value indicates the transition from a high level to a falling edge. It's set at 80% of the voltage range.

`in_rise_thr:` This threshold, set at 50% of the voltage range, represents the point at which the input signal is transitioning from a rising edge.

`in_fall_thr:` Similar to the previous parameter, this threshold at 50% of the voltage range indicates the transition from a falling edge in the input signal.

`out_rise_thr:` At 50% of the voltage range, this threshold represents the point at which the output signal is transitioning to a rising edge.

`out_fall_thr:` Similarly, this threshold at 50% of the voltage range indicates the transition point to a falling edge in the output signal.

![183231913-a9b3826b-5139-4bdc-b12b-3495b87cd8b9](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/e80af409-d9e1-48aa-86f7-70fd9bdb88f4)

Additionally, some important timing calculations are mentioned:

`Rise Delay:` Calculated as the time at the out_fall_thr minus the time at the in_rise_thr. It measures the time it takes for a signal to transition from a rising edge at the input to a falling edge at the output.

```
rise delay =  time(out_fall_thr) - time(in_rise_thr)
```

`Fall Transition Time:` Computed as the time at the slew_high_fall_thr minus the time at the slew_low_fall_thr. It represents the time it takes for a signal to transition from a high level to a low level.

```
Fall transition time: time(slew_high_fall_thr) - time(slew_low_fall_thr)
```

`Rise Transition Time:` Calculated as the time at the slew_high_rise_thr minus the time at the slew_low_rise_thr. It indicates the time it takes for a signal to transition from a low level to a high level.

```
Rise transition time: time(slew_high_rise_thr) - time(slew_low_rise_thr)
```

Below are the timing variables for propagation delay. The red is input waveform and blue is output waveform of the buffer. The left side is rise delay and right side is fall delay.

![183232515-fe3cef76-8a2f-475d-9a64-392fc2fda111](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/4337a1b9-4ffb-431a-8beb-9ef647d345f0)

Negative propagation delay is unexpected. That means the output comes before the input so designer needs to choose correct threshold point to produce positive delay. Delay threshold is usually 50% and slew rate threshold is usually 20%-80%.

# DAY 3: Design a Library Cell using Magic Layout and Ngspice Characterization

- [Labs for CMOS inverter ngspice simulations](#labs-for-cmos-inverter-ngspice-simulations)
  - [Designing a Library Cell](#designing-a-library-cell)
  
## Labs for CMOS inverter ngspice simulations

In OpenLANE, you can change configurations on the fly for your current session. For example, to switch the IO_mode to non-equidistant (mode 2), use `% set ::env(FP_IO_MODE) 2;`. After making the change, run the floorplan again with `% run_floorplan`. However, these changes won't affect the `runs/config.tcl` file, and they're only valid for the current session. You can check the current value of a configuration with echo `$::env(FP_IO_MODE)`.

### Designing a Library Cell

When designing a library cell, here are some key points to keep in mind:

`SPICE Deck:` The SPICE deck represents the component connectivity, essentially serving as a netlist for the CMOS inverter. It specifies how the components (transistors, resistors, etc.) are connected.

`SPICE Deck Values:` Values such as W/L (width/length) ratios are crucial. For example, "0.375u/0.25u" indicates that the width is 375nm, and the length is 250nm. In CMOS design, PMOS transistors are typically wider (2x or 3x) than NMOS transistors to achieve balanced performance.

`Gate and Supply Voltages:` These voltages are usually multiples of the transistor length. For instance, in your example, the gate voltage is set at 2.5V. Proper voltage levels are vital for correct CMOS operation.

`Adding Nodes:` Surround each component with nodes and give them meaningful names. These nodes are essential for identifying and connecting components in the SPICE simulation.

![Screenshot 2023-09-17 145644](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/37bf48f5-a04c-4173-a7fb-f37304c56aee)

#### Additional notes:

`Width vs. Length:` In CMOS transistors, the width corresponds to the size of the source and drain regions, while the length is the distance between them. Proper sizing (W/L ratios) is crucial for transistor behavior and performance.

`Carrier Mobility:` PMOS transistors have slower hole carrier mobility compared to NMOS's electron carrier mobility. To match rise and fall times and achieve balanced performance, PMOS transistors should be wider (thicker) to reduce resistance and increase carrier mobility.

Designing a library cell involves careful consideration of these factors to ensure that the cell functions correctly and efficiently within a larger integrated circuit design.

### SPICE Deck Netlist Description

![Screenshot 2023-09-17 152638](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/8bdc900d-50a7-43a1-873d-ffa1b99a1913)

Here are some important notes regarding SPICE simulation for CMOS components:

Syntax for PMOS and NMOS Description: When describing PMOS and NMOS components in SPICE, use the following syntax:

```css
[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]
```

- This syntax specifies the component's name, terminals (drain, gate, source), substrate connection, transistor type, and W/L ratios (width and length).

- Component Description Based on Nodes and Values: Ensure that you describe all components in the SPICE netlist based on their nodes and associated values. This information defines the interconnections and characteristics of the components.

- `.op Simulation:` The .op statement marks the start of the SPICE simulation operation. In this operation, the input voltage (Vin) will be swept from 0 to 2.5V in steps of 0.5V.

`Model File:` tsmc_025um_model.mod is the model file that contains the technological parameters for the 0.25μm NMOS and PMOS transistors. These parameters are essential for accurate SPICE simulations.

- Steps for SPICE Simulation:

- Source the SPICE netlist file using `source [filename].cir`.
- Run the simulation using run.
- Set up plot configurations using `setplot`.
- Perform a DC sweep analysis using `dc1`.
- Plot the output (out) against the input (in) to observe the circuit's behavior.

```bash
source [filename].cir
run
setplot 
dc1 
plot out vs in 
```

These steps are essential for conducting SPICE simulations to analyze the performance of CMOS components and circuits accurately.

## CMOS inverter Switching Threshold Vm

The switching threshold (Vm) of a CMOS inverter refers to the voltage level at which the input (Vin) and the output (Vout) of the inverter become equal. In other words, Vm is the point on the inverter's transfer characteristic where the transition between the high and low logic states occurs.

At this switching threshold (Vm), both the PMOS (p-channel metal-oxide-semiconductor) and NMOS (n-channel metal-oxide-semiconductor) transistors within the CMOS inverter are in the ON state. This means that both transistors are conducting, allowing a path for current to flow from the power supply to ground through the transistors. As a result, there is a leakage current in the inverter at this point.

Understanding the switching threshold Vm is crucial in CMOS design because it defines the point at which the logic level changes, and it can impact the speed and power consumption of digital circuits. Designers often optimize CMOS circuits to ensure that the switching threshold occurs at the desired voltage level and that the transition between logic states is sharp and well-defined.

![Screenshot 2023-09-17 171242](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/94c2f81c-4be4-4e7c-b01d-f429c151a1ca)

## 16 Mask CMOS Fabrication


The CMOS (Complementary Metal-Oxide-Semiconductor) fabrication process you've outlined, known as the 16-Mask CMOS Process, is a fundamental and widely used technique for manufacturing integrated circuits. Here's a summary of the key steps:

Substrate Selection: The process begins with selecting a substrate, typically a P-type silicon wafer. The substrate provides the foundation upon which the IC will be built.

Creating Active Regions for Transistors:

1. Substrate Selection: The process begins with selecting a substrate, typically a P-type silicon wafer. The substrate provides the foundation upon which the IC will be built.

2. Creating Active Regions for Transistors:

- `Isolation:` Separate transistor regions using silicon dioxide (SiO2) for electrical isolation.
- `Mask 1:` A mask protects areas that should not be etched away, preserving active regions.
- `Photoresist Layer:` This layer can be selectively removed with UV light, defining patterns.
- `Si3N4 Layer:` Silicon nitride (Si3N4) prevents uncontrolled SiO2 growth during oxidation.
- `SiO2 Layer (LOCOS):` Local Oxidation of Silicon (LOCOS) grows SiO2 for isolation between transistors.


3. N-Well and P-Well Fabrication: These are created to provide the necessary substrates for PMOS (N-Well) and NMOS (P-Well) transistors.

- `Dopant Materials:` Phosphorus (with 5 valence electrons) is used for the N-Well, while Boron (with 3 valence electrons) is used for the P-Well.
- `Mask 2:` This mask protects the N-Well (PMOS side) during the fabrication of the P-Well (NMOS side).
- `Mask 3:` Mask 3 protects the P-Well (NMOS side) while the N-Well (PMOS side) is being fabricated.


4. Formation of Gate: Gate fabrication impacts the threshold voltage of transistors.


Key factors affecting threshold voltage include:
  
- `Doping Concentration:` Controlled via ion implantation, with Mask 4 for Boron implantation in NMOS P-Well and Mask 5 for Arsenic implantation in PMOS N-Well.
- `Oxide Capacitance:` Controlled by adjusting oxide thickness; the SiO2 layer is removed and rebuilt to the desired thickness.

`Mask 6:` This mask is used for gate formation using a polysilicon layer.


5. Lightly Doped Drain Formation: Before creating the source and drain layers, lightly doped impurities are introduced:

- `Mask 7:` Used for N-type implantation (lightly doped) for NMOS transistors.
- `Mask 8:` Employed for P-type implantation (lightly doped) for PMOS transistors.

Heavily Doped Impurity: The heavily doped impurity (N+ for NMOS and P+ for PMOS) is used for the actual source and drain regions. The lightly doped impurity helps maintain spacing between the source and drain, preventing hot electron and short-channel effects.

6. Source and Drain Formation: For the creation of the source and drain regions:
- `Mask 9:` Used for N+ implantation.
- `Mask 10:` Employed for P+ implantation.
- `Channeling Prevention:` To prevent implantations from penetrating too deep into the substrate, a screen oxide layer is added before implantation.
- `Side-Wall Spacers:` These spacers help maintain the N-/P- regions while implanting the N+/P+ regions.


7. Form Contacts and Interconnects: In this step, different materials are used for interconnections and contacts:
- `TiN:` Used for local interconnections and bringing contacts to the top.
- `TiS2:` Employed for contacts to the actual Drain-Gate-Source.

Mask 11: This mask is used to etch off the TiN interconnect for the first-layer contact.


8. Higher Level Metal Formation: This step involves the formation of higher-level metal interconnects. It includes the following sub-steps:

- `Planarization via CMP (Chemical Mechanical Polishing):` The layer is planarized before adding a metal interconnect.
- `Aluminum Contact:` Aluminum contacts are used to connect the lower contact to the higher metal layer.
- `Repetition:` The process is repeated until the contact reaches the outermost layer.

`Mask 12:` Used for creating the first contact hole.

`Mask 13:` Employed for the first Aluminum contact layer.

`Mask 14:` Used for the second contact hole.

`Mask 15:` Employed for the second Aluminum contact layer.

`Mask 16:` Used for making contact to the topmost layer.



To integrate the CMOS inverter standard cell layout and perform SPICE extraction, follow these steps:

Clone the vsdstdcelldesign repository within your openlane working directory:

```shell
git clone https://github.com/nickson-jose/vsdstdcelldesign
```
This command will create a folder named "vsdstdcelldesign" in your openlane directory.

To invoke Magic and view the `"sky130_inv.mag"` file, you need to include the "sky130A.tech" file along with its path. You can simplify this by copying the `"sky130A.tech"`file from the "magic" folder to the `"vsdstdcelldesign"` folder:
```shell
cp /path/to/openlane/pdks/sky130A/libs.tech/magic/sky130A.tech /path/to/openlane_working_dir/vsdstdcelldesign
```
Replace "/path/to/" with the actual paths to your openlane and vsdstdcelldesign directories.

Now, you can invoke Magic to view the "sky130_inv.mag" file:
```shell
magic -T sky130A.tech sky130_inv.mag &
```
This command will open Magic with the specified technology file, allowing you to view and work with the CMOS inverter layout.


Sky130's first layer is known as the local interconnect layer or "Locali." To verify a layout as that of a CMOS inverter:

1. Observe P-diffusion and N-diffusion regions connected with Polysilicon.

2. Check the drain and source connections. Ensure that the drains of both PMOS and NMOS are connected to the output port (usually labeled as "Y"), and the sources of both are connected to the power supply VDD (often labeled as "VPWR").

3. LEF (Library Exchange Format) is a format used to provide information about cell boundaries, VDD and GND lines, but it does not contain information about the logic of the circuit. It is also used to protect intellectual property (IP).

SPICE extraction from a .mag (Magic) layout to a .spice (SPICE) file can be achieved within the Magic environment using the following commands in tkcon:

```css
extract all
ext2spice cthresh 0 rethresh 0
ext2spice
```
These commands perform the extraction process.


![Screenshot 2023-09-17 193948](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/6517a924-58bd-4601-8591-1a9f19c3274a)


The SPICE deck (sky130_in.spice) is updated to include the PMOS and NMOS libraries (pshort.lib and nshort.lib). Additionally, the minimum grid size of the inverter, obtained from the Magic layout, is included in the deck using the command .option scale=0.01u. The model names in the MOSFET definitions are also modified: the PMOS model name is changed to pshort.model.0, and the NMOS model name is changed to nshort.model.0.

Finally voltage sources and simulation commands are defined as follows:
```bash
VDD VPWR 0 3.3V
VSS VGND 0 0
Va A VGND PUSLE(0V 3.3V 0 0.1ns 0.1 ns 2ns 4ns)
.tran 1n 20n
.control
run 
.endc
.end
```
The final sky130_inv.spice file is modified to:

```bash
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

* .subckt sky130_inv A Y VPWR VGND
M0 Y A VGND VGND nshort_model.0 ad=1435 pd=152 as=1365 ps=148 w=35 l=23
M1 Y A VPWR VPWR pshort_model.0 ad=1443 pd=152 as=1517 ps=156 w=37 l=23
C0 A VPWR 0.08fF
C1 Y VPWR 0.08fF
C2 A Y 0.02fF
C3 Y VGND 0.18fF
C4 VPWR VGND 0.74fF
* .ends

* Power supply 
VDD VPWR 0 3.3V 
VSS VGND 0 0V 

* Input Signal
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

* Simulation Control
.tran 1n 20n
.control
run
.endc
.end
```

For simulation, ngspice is invoked in the terminal:

```bash
ngspice sky130_inv.spice
```

![Screenshot 2023-09-17 195854](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/7da5eab2-e3b3-4b66-9569-9ffec916fb15)

The output "y" is to be plotted with "time" and swept over the input "a":

```bash
plot y vs time a
```
![Screenshot 2023-09-17 200022](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/77de0c57-2815-401e-96c8-390b0a832844)

In inverter standard cell characterization, four important timing parameters are used:

1. Rise Transition: This is the time taken for the output to rise from 20% of its maximum value to 80% of its maximum value.
2. Fall Transition: This is the time taken for the output to fall from 80% of its maximum value to 20% of its maximum value.
3. Cell Rise Delay: It is the delay experienced by the cell when the input rises from 50% to 50%, i.e., from a low to high transition.
4. Cell Fall Delay: It is the delay experienced by the cell when the input falls from 50% to 50%, i.e., from a high to low transition.

Here are the computed values for these parameters:

- `Rise Transition:` 59.08 picoseconds
- `Fall Transition:` 42.87 picoseconds
- `Cell Rise Delay:` 56.36 picoseconds
- `Cell Fall Delay:` 24.79 picoseconds

These parameters provide critical information about the performance of the inverter standard cell and are important for understanding its behavior in digital circuits.


# DAY 4: Pre layout Timing Analysis and Importance of Good Clock Tree

## Timing Analysis & CTS

In timing analysis and clock tree synthesis (CTS), it's crucial to ensure that the ports specified in the `tracks.info` file are located at the intersection of horizontal and vertical tracks. This is particularly important for the CMOS inverter ports A and Y, which are on the li1 layer.

To achieve this, you can access the tracks.info file, which contains information about the pitch and direction of tracks. By carefully analyzing this file, you can determine the placement of the ports A and Y to ensure they align with the grid of horizontal and vertical tracks. This alignment is essential for proper signal routing and timing in your design.

![Screenshot 2023-09-17 201154](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/9d07c327-507e-4874-8566-de1b2e5a6a62)

To ensure that the ports are located at the intersection points of the tracks, you can adjust the grid spacing in Magic (tkcon) to match the li1 layer's X and Y values. This alignment is crucial for proper signal routing and timing analysis. You can achieve convergence of the grid and tracks using the following command:

```bash
grid 0.46um 0.34um 0.23um 0.17um
```

![Screenshot 2023-09-17 201901](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/741bff2b-5c5e-4edd-8842-3f9f3a81d5d6)

### Create port definition

Creating port definitions for your cell is an important step in the design process, as it helps the placer and router tools understand the connectivity of your cell. Here are the steps to define ports for your cell in Magic Layout:

1. Open Magic Layout window.
2. Source the .mag file for your cell design (in this case, a simple CMOS inverter).
3. Go to the "Edit" menu and select "Text." This will open a dialogue box.

These steps will allow you to define ports and set the correct class and use attributes for each port. Defining ports in this way ensures that your cell's connectivity is properly conveyed to the tools in a standard format, making it easier for downstream processes like placement and routing.

![Screenshot 2023-09-17 202757](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/f8e51024-3922-4290-8f72-45d6e16ba401)

To define ports in Magic Layout, create boxes on specific layers corresponding to the ports, and label them with names. Attach sticky labels specifying the associated layer name. Enable the "Port enable" checkbox and uncheck "Default" for each box to designate them as ports. This process ensures proper identification and utilization of ports in the design.

![Screenshot 2023-09-17 202829](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/1e89ffd8-7724-4f49-ad22-ac84513cc15b)

In the provided figures, port A (input port) and port Y (output port) are defined on the locali (local interconnect) layer. The number in the textarea near the "enable" checkbox specifies the order in which the ports will be written in the LEF file, with 0 being the first.

For power and ground layers, the port definitions can be similar or different from the signal layer. In this case, ground and power connections are defined on metal1, as indicated by the sticky label.

![Screenshot 2023-09-17 212447](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/f2434904-a221-4018-8c4d-402f88a966da)

![Screenshot 2023-09-17 212518](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/dae7b870-9908-42cd-bea7-1ce3a8e4f941)

### Standard Cell LEF generation

To generate the LEF (Library Exchange Format) for the CMOS Inverter standard cell, the purpose of ports must be defined as follows:

Select port A in Magic:

- Port class: input
- Port use: signal

Select Y area:

- Port class: output
- Port use: signal

Select VPWR area:

- Port class: inout
- Port use: power

Select VGND area:

- Port class: inout
- Port use: ground

After defining the port attributes, you can carry out LEF extraction in tkcon using the following command:

```bash
lef write
```

This will generate the LEF file for the CMOS Inverter standard cell with the specified port definitions.

![Screenshot 2023-09-17 214904](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/97cdb623-2ebe-41e3-9cc5-0c4ec3f822a4)

### Integrating custom cell in OpenLANE

To integrate a custom standard cell into OpenLANE, you can follow these steps:

1. `Copy LEF File:` Copy your custom standard cell's LEF file, e.g., `sky130_vsdinv.lef`, to the `designs/picorv32a/src` directory.
2. `Copy Library File:` Copy the library file that defines your custom CMOS inverter cell, such as sky130_fd_sc_hd_typical.lib, from the vsdstdcelldesign/libs directory to the same designs/picorv32a/src directory. You can also copy slow and fast library files if needed.
3. `Modify config.tcl:` Open the config.tcl file in your OpenLANE working directory.

Add or modify the paths to your LEF and library files as follows:
```tcl
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```

#### modified config.json file:

```json

{
  "DESIGN_NAME": "picorv32",
  "VERILOG_FILES": "dir::src/picorv32a.v",
  "CLOCK_PORT": "clk",
  "CLOCK_NET": "clk",
  "FP_SIZING": "relative",
  "LIB_SYNTH" : "dir::src/sky130_fd_sc_hd__typical.lib",
  "LIB_FASTEST" : "dir::src/sky130_fd_sc_hd__fast.lib",
  "LIB_SLOWEST" : "dir::src/sky130_fd_sc_hd__slow.lib",
  "LIB_TYPICAL":"dir::src/sky130_fd_sc_hd__typical.lib",
  "TEST_EXTERNAL_GLOB":"dir::../sd_fsm/src/*",
  "SYNTH_DRIVING_CELL":"akhil_inv",
  "pdk::sky130*": {
    "FP_CORE_UTIL": 50,
    "CLOCK_PERIOD": 65,
    "scl::sky130_fd_sc_hd": {
      "FP_CORE_UTIL": 60
    }
  }
}
```

##### To integrate the standard cell into the OpenLANE flow and perform the mentioned steps, follow these commands:

```bash
# Prepare the design
prep -design picorv32a -tag RUN_2023.09.18_11.58.04 -overwrite

# Find and set the LEF files
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

# Add the LEF files to the design
add_lefs -src $lefs

# Run synthesis
run_synthesis
```

![Screenshot 2023-09-17 235417](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/ef52787b-6ad6-437b-bb37-4b45a66a13c6)

Next floorplan is run, followed by placement:

```bash
run_floorplan
```

To check the layout invoke magic from the results/floorplan directory:

```bash
magic -T /home/devipriya/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.max.lef def read picorv32a.def &
```

![Screenshot from 2023-09-18 18-21-30](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/ac439dbb-c73b-4082-ba48-b852e68e0dc6)


```bash
run_placement
```

![Screenshot from 2023-09-18 19-47-24](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/e2ac6d03-aff5-4082-9f49-d048f12754c0)


## Post-synthesis timing analysis

Timing analysis is carried out outside the openLANE flow using OpenSTA tool. For this, a new file pre_sta.conf is created. This file would be reqiured to carry out the STA analysis. Invoke OpenSTA outside the openLANE flow as follows:

```bash
sta pre_sta.conf
```

Since clock tree synthesis has not been performed yet, the analysis is with respect to ideal clocks and only setup time slack is taken into consideration. The slack value is the difference between data required time and data arrival time. The worst slack value must be greater than or equal to zero. If a negative slack is obtained, following steps may be followed:

Change synthesis strategy, synthesis buffering and synthesis sizing values

Review maximum fanout of cells and replace cells with high fanout


## Clock Tree Synthesis Stage:

There are three parameters that we need to consider when building a clock tree:

- Clock Skew = In order to have minimum skew between clock endpoints, clock tree is used. This results in equal wirelength (thus equal latency/delay) for every path of the clock.
- Clock Slew = Due to wire resistance and capacitance of the clock nets, there will be slew in signal at the clock endpoint where signal is not the same with the original input clock signal anymore. This can be solved by clock buffers. Clock buffer differs in regular cell buffers since clock buffers has equal rise and fall time.
- Crosstalk = Clock shielding prevents crosstalk to nearby nets by breaking the coupling capacitance between the victim (clock net) and aggresor (nets near the clock net), the shield might be connected to VDD or ground since those will not switch. Shileding can also be done on critical data nets.


### CTS Command Script:

1. The CTS (Clock Tree Synthesis) command script in OpenLANE helps you handle clock tree synthesis for your design. It involves adding clock buffers to your design to ensure proper clock distribution. Below is an overview of the CTS process and some important configuration variables:

1. Modified Verilog Netlist: Before running CTS, you need to extract a modified Verilog netlist after performing timing ECO (Engineering Change Order), floorplan, and placement. This modified netlist is saved under `/runs/[date]/results/cts/`.

3. TCL Procedure: OpenLANE commands like run_cts are implemented as TCL procedures (procs). These procs can be found in `/OpenLane/scripts/tcl_commands/`. For example, run_cts is implemented in `/OpenLane/scripts/tcl_commands/cts.tcl`. These procs call OpenROAD, which is responsible for running the actual CTS tool.

Configuration Variables: The CTS process involves various configuration variables that control the behavior of TritonCTS (the CTS tool used by OpenLANE). Some important ones are:

- `CTS_CLK_BUFFER_LIST`: This is a list of clock branch buffers that can be used in the clock tree. Examples include `sky130_fd_sc_hd__clkbuf_8`, `sky130_fd_sc_hd__clkbuf_4`, and `sky130_fd_sc_hd__clkbuf_2`.
- `CTS_ROOT_BUFFER`: This variable specifies the clock buffer used at the root of the clock tree. It is typically the largest clock buffer responsible for driving the clock tree across the entire chip. Example: `sky130_fd_sc_hd__clkbuf_16`.
- `CTS_MAX_CAP`: This variable defines the maximum capacitance that the output port of the root clock buffer can drive.

These variables are defined in the /OpenLane/scripts/openroad/cts.tcl script, which contains the specific OpenROAD commands for running TritonCTS with the desired configurations.

## Timing Analysis with Real Clocks:

Setup and hold analysis with real clock will now include clock buffer delays:

- In setup analysis, the point is that the data must arrive first before the clock rising edge to properly latch that data. Setup violation happens when path is slow. This is affected by parameters such as combinational delay, clock buffer delay, time period, setup time, and setup uncertainty (jitter).

- Hold analysis is the delay that the MUX2 model inside the flip flop needs to move the data to outside. This is the time that the launch flop must hold the data before it reaches the capture flop. Hold analysis is done on the same rising clock edge for launch and capture flop unlike in setup analysis where it spans between two rising clock edges. Hold violation happens when path is too fast. This is affected by parameters such as combinational delay, clock buffer delays, and hold time. (time period and setup uncertainty does not matter since launch and capture flops will receive the same rising clock edges fo hold analysis)

The goal is to have a positive slack on both setup and hold analysis.

# DAY 5: Final Steps for RTL2GDS using TritonRoute and OpenSTA

## Maze Routing:
One simple routing algorithm is Maze Routing or Lee's routing:

The shortest path is one that follows a steady increment of one (1-to-9 on the example below). There might be multiple path like this but the best path that the tool will choose is one with less bends. The route should not be diagonal and must not overlap an obstruction such as macros.
This algorithm however has high run time and consume a lot of memory thus more optimized routing algorithm is preferred (but the principles stays the same where route with shortest path and less bends is preferred)


## Power Distribution Network (review):
This is just a review on PDN. The power and ground rails has a pitch of 2.72um thus the reason why the customized inverter cell has a height of 2.72 or else the power and ground rails will not be able to power up the cell. Looking at the LEF file runs/[date]/tmp/merged.nom.lef, you will notice that all cells are of height 2.72um and only width differs.

As shown below, power and ground flows from power/ground pads -> power/ground ring-> power/ground straps -> power/ground rails.



In OpenLANE, the generation of the Power Distribution Network (PDN) is a separate step that occurs after clock tree synthesis (CTS) and post-CTS static timing analysis (STA). It's important to note that PDN generation is not part of the floorplan run in the OpenLANE flow. You would typically generate the PDN using the following command:

```bash
gen_pdn
```

This command generates the necessary power distribution network for your chip design. It ensures that power is properly distributed to all the cells and components on the chip to meet the power requirements.

The PDN generation step comes after CTS because it needs to consider the clock tree and its impact on power distribution. It's essential for ensuring the proper functioning and performance of your chip.

Once you've run the gen_pdn command, you can confirm the success of PDN generation by checking the CURRENT_DEF environment variable, as mentioned in the previous response:

```bash
echo $::env(CURRENT_DEF)
```

If the PDN generation was successful, this command will display the path to the current DEF file, which should now include the generated PDN.

We can confirm the success of PDN by checking the current def environment variable: `echo $::env(CURRENT_DEF)`


The gen_pdn command in ASIC design, which stands for Power Distribution Network generation, is a critical step in the chip design flow. Here are the key points regarding the gen_pdn command:

- `Input DEF File`: The gen_pdn command takes the design_cts.def file as input. This DEF file contains the chip's physical design information after clock tree synthesis (CTS).
- `Grid and Straps`: The command generates the power distribution network, including the grid and straps, for both Vdd (power) and Gnd (ground). These components are strategically placed around the standard cells in the chip layout.
- `Standard Cell Height`: The height of standard cells is designed to be multiples of the space between the Vdd and Gnd rails. In this case, the pitch is 2.72. Adhering to these dimensions is essential to ensure proper power distribution to the standard cells.
- `Power Entry Points`: Power is initially supplied to the chip through power pads. There are separate pads for Vdd and Gnd. From these pads, power enters the power distribution network through vias.
- `Rings and Straps`: The power distribution network includes rings that encircle the standard cells. Vdd straps are connected to the Vdd ring, and Gnd straps are connected to the Gnd ring. Both horizontal and vertical straps are used.
- `Connection to Standard Cells`: The power from the straps is supplied to the standard cells by connecting the straps to the rails of the standard cells. This ensures that each standard cell receives the required power.
- `Handling Macros`: If macros (pre-designed blocks) are present in the chip design, the straps attach to the rings of the macros via macro pads. The power distribution network for macros is pre-configured.
- `Metal Layers and Vias`: In this design, straps are typically located at metal layers 4 and 5, while the standard cell rails are at metal layer 1, Vias are used to connect these layers as needed to ensure proper power distribution.

The gen_pdn command is crucial for ensuring that all components in the chip receive the required power to function correctly, and it plays a significant role in achieving power integrity in the chip design.

## Routing

Routing in the OpenLANE flow is a crucial step in the chip design process, and it involves two stages: global routing and detailed routing. Here are the key points regarding routing in OpenLANE:

1. Global Routing:

- In the global routing stage, the routing region is divided into rectangular grids, which are represented as coarse 3D routes.
- This stage primarily utilizes the Fastroute tool to perform global routing.

2. Detailed Routing:

- Detailed routing involves working with finer grids and routing guides to implement the physical wiring of the chip.
- TritonRoute is the primary tool used for detailed routing.

###vFeatures of TritonRoute:

- TritonRoute has several features, including honoring pre-processed route guides and assuming that each net satisfies inter-guide connectivity.
- It uses a Mixed-Integer Linear Programming (MILP) based panel routing scheme.
- TritonRoute employs an intra-layer parallel and inter-layer sequential routing framework to efficiently route nets.

### Running Routing in OpenLANE:

- The routing step can be initiated in OpenLANE using the command run_routing.
- Various routing options and configurations can be set in the config.tcl file to customize the routing process.
- Different routing strategies can be specified in the configuration, which may involve trade-offs between optimization and runtime.
- For example, in the default setting for the picorv32a design, the routing step takes approximately 30 minutes with the current version of TritonRoute.

### Important Considerations:

- It's essential to set the CURRENT_DEF variable to pdn.def to ensure that the correct input is used for routing.
- The two stages of routing, global route (Fastroute) and detailed route (TritonRoute), work together to generate the final routed design.
- Fastroute is responsible for generating routing guides, while TritonRoute takes the global route information and optimizes the routing path to connect the pins efficiently.

Routing plays a vital role in achieving physical connectivity between components in the chip and ensuring that the design meets performance and timing requirements.

GDSII

GDSII (Graphic Design Standard II) is a file format commonly used in the semiconductor industry to represent the layout of integrated circuits. Here are some key points regarding GDSII files in the OpenLANE flow:

1. GDSII Format:
- GDSII stands for Graphic Design Standard II.
- It is a binary file format used to describe the physical layout of semiconductor devices, including masks and layers.
- GDSII files contain detailed information about polygons, layers, text labels, and other elements that make up the layout of an integrated circuit.
  
2. Tape-Out:

- The term "tape-out" refers to the process of finalizing the design of an integrated circuit and preparing it for fabrication at a semiconductor foundry.
- Historically, GDS files were written onto magnetic tapes and physically sent to foundries for fabrication, hence the name "tape-out."

3. Usage in OpenLANE:

- In the OpenLANE flow, you can use the magic command to work with GDSII files.
- The GDSII file generated during the design process is typically located in the results/signoff/magic directory.

4. Design Rule Checking (DRC):

- DRC is an essential step in the chip design process that checks for violations of design rules, ensuring that the layout meets fabrication requirements.
- It's crucial to perform DRC checks on the GDSII file to identify and rectify any errors or violations.

5. Layout Visualization:

- GDSII files can be used to visualize and inspect the layout of the integrated circuit.
- Magic is a popular tool for viewing and editing GDSII files, and it is often used in the semiconductor design process.

6. No DRC Errors:

- It's mentioned that no DRC errors are found, which indicates that the layout has passed design rule checking and is ready for tape-out.

GDSII files play a critical role in the chip fabrication process, as they provide a standardized format for describing the physical layout of integrated circuits, ensuring accuracy and consistency during manufacturing.


![Screenshot 2023-09-18 001802](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/758eefd8-795a-443a-adf9-078836b49b97)

## Differences from older OpenLANE versions

The newer versions of OpenLANE have introduced some differences and improvements compared to older versions. Here are a couple of key differences:

### Reporting Location:

In the latest version of OpenLANE, the reports generated during the design process are not displayed directly in the terminal. Instead, users need to access and verify these reports from the reports/results folder. This change can help organize and manage the generated reports more efficiently.

### Integrated SPEF Extraction:

In older versions of OpenLANE, users may have needed to perform SPEF (Standard Parasitic Exchange Format) extraction as a separate step in the design flow. However, in the newer versions, SPEF extraction has been integrated into the OpenLANE flow. This integration simplifies the design process by automatically handling SPEF extraction as part of the toolchain.

# Acknowledgements

- The OpenROAD Project
- Kunal Ghosh
