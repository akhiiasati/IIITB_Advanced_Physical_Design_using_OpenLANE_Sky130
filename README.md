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
  - [How to Talk to Computers?](#how-to-talk-to-computers?)
  - [Components of Open-Source Digital ASIC Design](#components-of-open-source-digital-asic-design)
  - 

## Overview

OpenLANE is a powerful open-source tool and flow designed for open-source tape-outs. This versatile flow integrates a variety of essential tools, including Yosys, ABC, OpenSTA, Fault, OpenROAD app, Netgen, and Magic. Its primary purpose is to harden chips and macros, ultimately generating the final GDSII layout from the initial design RTL (Register-Transfer Level). The overarching objective of OpenLANE is to achieve a seamless and fully automated process, eliminating the need for extensive human intervention. It's noteworthy that OpenLANE is optimized for compatibility with the Google-Skywater130 Open Source Process Design Kit (PDK).

## Inception of Open-Source EDA

### How to Talk to Computers?
To understand the significance of open-source EDA, it's crucial to grasp the fundamental process of communicating with computers. The RISC-V Instruction Set Architecture (ISA) serves as the language to interact with computers whose hardware is based on RISC-V cores. When a user intends to run a specific application software on a computer, the corresponding program written in languages like C, C++, or Java must first be translated into instructions by a compiler. These compiler-generated instructions are inherently hardware-dependent. They serve as inputs to the assembler, which in turn produces binary code. This binary code is what the hardware logic within the chip layout comprehends. Depending on the received bits, the digital logic, composed of gates, performs the necessary functions as required by the user of the application software.

![Screenshot 2023-09-08 191049](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/2c56860d-c518-47c1-bff0-ac51bfffcdc5)
![Screenshot 2023-09-08 191119](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/9042de19-c0db-4127-87b1-938e287f51b1)
![Screenshot 2023-09-08 191831](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/31ea6fc7-1e0c-4a73-9316-812527577434)
![Screenshot 2023-09-08 191230](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/da9df8ad-6e02-4548-9b23-9e25bff65ea3)
![Screenshot 2023-09-08 191337](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/a9b97643-bb51-4a69-af72-bcbc068ceebf)

### Components of Open-Source Digital ASIC Design:

#### Foundry IP Blocks and Macro Blocks:

Foundry IP Blocks: These are blocks such as ADC (Analog-to-Digital Converter), DAC (Digital-to-Analog Converter), PLL (Phase-Locked Loop), and SRAM (Static Random-Access Memory). They are designed by foundries and often contain analog components.
Macro Blocks: These are pure digital logic blocks like RISC-V SoCs (System-on-Chips) and SPI (Serial Peripheral Interface) controllers. They do not typically contain analog parts.

#### Open-Source Components:

RTL Designs: These are digital designs at the Register Transfer Level (RTL) available from open-source platforms like GitHub (github.com), LibreCores (librecores.org), and OpenCores (opencores.org).

EDA Tools (Electronic Design Automation): Open-source EDA tools play a crucial role in the design flow. Examples include OpenROAD, OpenLANE, and QFlow, which automate various stages of chip design.

PDK (Process Design Kit): The PDK serves as the interface between the chip designer and the fabrication process. It includes essential data files and documents, such as cell libraries, IO libraries, and design rules (DRC, LVS, etc.).

Google + Skywater 130nm Production PDK:
The PDK for open-source digital ASIC design is often provided by a collaboration between Google and Skywater. The PDK includes the necessary information for designing and manufacturing ICs using the Skywater 130nm process technology.

PDK (Process Design Kit) Details:

- Cell Libraries: These contain predefined digital and analog cells that designers can use in their custom ICs.
IO Libraries: These define the input and output characteristics of the chip's pins.
- Design Rules (DRC, LVS, etc.): These documents specify constraints and criteria that must be followed to ensure proper chip functionality and manufacturability.
- Open-source digital ASIC design combines these components to enable cost-effective and accessible custom chip creation while leveraging the collaborative efforts of the open-source community and utilizing established design and fabrication standards.

![Screenshot 2023-09-08 191359](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/8614c949-4432-4125-8eb8-a00c466ef5d0)

## SoC Design & OpenLANE

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

### Process Design Kit(PDK)

A Process Design Kit (PDK) is a collection of essential data and information used in semiconductor chip design. It provides designers with details about the manufacturing process, including the properties of various components, design rules, and guidelines needed to create custom integrated circuits. PDKs serve as a bridge between chip designers and semiconductor foundries, ensuring that designs are compatible with the fabrication process.

The collaboration between Google and SkyWater Technology involves the development and release of an open-source Process Design Kit (PDK) specifically tailored for the SkyWater 130nm semiconductor manufacturing process. Here's a breakdown of this collaboration:

#### Google and SkyWater Technology:

- Google: A multinational technology company known for its internet-related products and services.

- SkyWater Technology: A semiconductor foundry specializing in providing access to advanced semiconductor manufacturing technologies.

#### SkyWater 130nm Process:

- The SkyWater 130nm process refers to a specific semiconductor manufacturing process offered by SkyWater Technology.
-  It denotes the technology used to fabricate integrated circuits with a feature size of 130 nanometers.


  
#### Open-Source PDK:

- The collaboration between Google and SkyWater resulted in the development of an open-source Process Design Kit (PDK) for the SkyWater 130nm process.
- The PDK includes essential data and documents required for chip design, such as cell libraries (digital and analog), IO libraries, design rules (DRC, LVS, etc.), and other manufacturing-related information.
- This open-source PDK aims to make the process of designing and manufacturing custom integrated circuits more accessible and cost-effective.

![Screenshot 2023-09-09 183613](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/5bf530f3-c1e4-4cac-b5e8-3e23e1b896bc)



