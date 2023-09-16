# IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130

This project was completed as part of the "Advanced Physical Design using OpenLANE/Sky130" course by VLSI System Design Corporation. It encompasses the entire RTL to GDSII flow for the PicoRV32a System-on-Chip (SoC) using the OpenLANE toolchain with the Skywater130nm Process Design Kit (PDK). In addition to the standard flow, custom-designed standard cells within the Sky130 PDK were integrated, timing optimizations were performed to meet design constraints, slack violations were rectified, and DRC (Design Rule Check) was rigorously verified.

## Table of Contents
- [Software Installation](#software-installation)
- [Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1-inception-of-open-source-eda-openlane-and-sky130-pdk)
- [Day 2: Good floorplan vs bad floorplan and introduction to library cells](#Day-2-good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)

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
magic -T /home/akhilasati/vsdstdcelldesign/libs/sky130A.tech lef read /home/akhilasati/OpenLane/designs/picorv32a/runs/RUN_2023.09.16_14.30.58/tmp/merged.min.lef def read picorv32a.def &
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
In the OpenLANE ASIC flow, the placement step is a crucial stage in designing an integrated circuit

#### Placement Optimization

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

magic -T /home/akhilasati/vsdstdcelldesign/libs/sky130A.tech lef read /home/akhilasati/OpenLane/designs/picorv32a/runs/RUN_2023.09.16_18.33.33/tmp/merged.max.lef def read picorv32.def &
