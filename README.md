# IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130

This project was completed as part of the "Advanced Physical Design using OpenLANE/Sky130" course by VLSI System Design Corporation. It encompasses the entire RTL to GDSII flow for the PicoRV32a System-on-Chip (SoC) using the OpenLANE toolchain with the Skywater130nm Process Design Kit (PDK). In addition to the standard flow, custom-designed standard cells within the Sky130 PDK were integrated, timing optimizations were performed to meet design constraints, slack violations were rectified, and DRC (Design Rule Check) was rigorously verified.

## Table of Contents
- [Software Installation](#software-installation)
- [Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1-inception-of-open-source-eda-openlane-and-sky130-pdk)

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

![Screenshot 2023-09-08 191119](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/9042de19-c0db-4127-87b1-938e287f51b1)

![Screenshot 2023-09-08 191337](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/a9b97643-bb51-4a69-af72-bcbc068ceebf)

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

![Screenshot 2023-09-09 143356](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/bfface78-fe96-4741-9df2-b74ce90956f1)

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

The RTL-to-GDSII (Register Transfer Level to Graphic Data System II) flow is a crucial process in integrated circuit (IC) design that involves transforming a high-level RTL description of a digital design into a final layout in GDSII format, which is the file format used for manufacturing semiconductor devices. This flow typically consists of several key steps, which I'll document in more detail:

Synthesis:

- Objective: Convert RTL (Register Transfer Level) design into a gate-level netlist using standard cell libraries (SCL).
- Description: During this phase, the RTL code, written in a hardware description language like VHDL or Verilog, is synthesized into a gate-level representation. This involves mapping the RTL constructs into specific standard cells available in the chosen library. The output is a gate-level netlist.

Floor & Power Planning:

- Objective: Plan the silicon area and ensure robust power distribution.
- Description: In this step, the chip's physical floorplan is established. This includes determining the location of various functional blocks, I/O pads, and power distribution networks. Careful floor planning is essential to optimize the use of silicon area and ensure efficient power distribution to all parts of the chip.

Placement:

- Objective: Place cells on floorplan rows aligned with predefined sites.
- Description: The synthesized gate-level cells are placed onto the chip's floorplan. These placements are typically aligned with rows and sites specified by the technology library. The goal is to achieve good utilization of the available area and minimize wirelength for better performance.
Global Placement:

- Objective: Optimize the initial placement of cells for better overall performance.
- Description: After the initial placement, a global placement algorithm is applied to optimize the positions of cells. This optimization considers factors such as timing, power, and area. The goal is to find an optimal arrangement that meets design constraints.

Detailed Placement:

- Objective: Refine cell positions to ensure legal placements and meet design rules.
- Description: Detailed placement fine-tunes the positions of individual cells to ensure they meet legal placement constraints and adhere to design rules. This step helps to further optimize timing, area, and power.

Routing:

- Objective: Create valid patterns for connecting wires between cells.
- Description: Routing involves the creation of metal interconnects (wires) that connect the outputs of one cell to the inputs of another. The routing process must adhere to design rules to avoid issues like short circuits and timing violations. Various routing algorithms and techniques are used to achieve this.

Signoff:

- Objective: Perform physical and timing verifications to ensure design correctness.
- Description: The final step involves thorough verification of the physical design. This includes Design Rule Checking (DRC) to ensure that layout adheres to manufacturing rules, Layout vs. Schematic (LVS) checks to verify consistency between the layout and the netlist, and Static Timing
Analysis (STA) to ensure that timing constraints are met.

Once all these steps are completed successfully, the output of the RTL-to-GDSII flow is a GDSII file that contains the physical layout of the integrated circuit. This GDSII file is then used in the semiconductor manufacturing process to produce the actual ICs.







