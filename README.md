# IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130

This project was completed as part of the "Advanced Physical Design using OpenLANE/Sky130" course by VLSI System Design Corporation. It encompasses the entire RTL to GDSII flow for the PicoRV32a System-on-Chip (SoC) using the OpenLANE toolchain with the Skywater130nm Process Design Kit (PDK). In addition to the standard flow, custom-designed standard cells within the Sky130 PDK were integrated, timing optimizations were performed to meet design constraints, slack violations were rectified, and DRC (Design Rule Check) was rigorously verified.

## Table of Contents
- [Software Installation](#software-installation)
- [Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1-inception-of-open-source-eda-openlane-and-sky130-pdk)

## Software Installation

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


# Day 1: Inception of open-source EDA, OpenLANE and Sky130 PDK

## Overview

OpenLANE is a powerful open-source tool and flow designed for open-source tape-outs. This versatile flow integrates a variety of essential tools, including Yosys, ABC, OpenSTA, Fault, OpenROAD app, Netgen, and Magic. Its primary purpose is to harden chips and macros, ultimately generating the final GDSII layout from the initial design RTL (Register-Transfer Level). The overarching objective of OpenLANE is to achieve a seamless and fully automated process, eliminating the need for extensive human intervention. It's noteworthy that OpenLANE is optimized for compatibility with the Google-Skywater130 Open Source Process Design Kit (PDK).

## Inception of Open-Source EDA
### How to Talk to Computers?
To understand the significance of open-source EDA, it's crucial to grasp the fundamental process of communicating with computers. The RISC-V Instruction Set Architecture (ISA) serves as the language to interact with computers whose hardware is based on RISC-V cores. When a user intends to run a specific application software on a computer, the corresponding program written in languages like C, C++, or Java must first be translated into instructions by a compiler. These compiler-generated instructions are inherently hardware-dependent. They serve as inputs to the assembler, which in turn produces binary code. This binary code is what the hardware logic within the chip layout comprehends. Depending on the received bits, the digital logic, composed of gates, performs the necessary functions as required by the user of the application software.

### SoC Design & OpenLANE
Components of Open-Source Digital ASIC Design
Designing a digital Application Specific Integrated Circuit (ASIC) relies on three critical enablers or elements:

Resistor Transistor Logic Intellectual Property (RTL IPs): These are the foundational building blocks for ASIC designs. Open-source RTL designs are readily accessible through platforms like GitHub, LibreCores, and OpenCores.

Electronic Design Automation (EDA) Tools: Open-source EDA tools play a pivotal role in the design process, offering robust support for various stages of ASIC development. Notable open-source EDA tools include QFlow, OpenROAD, and OpenLANE.

Process Design Kit (PDK) Data: The availability of open-source PDK data is a game-changer. The Google Skywater130 PDK is a prime example of open-source PDKs that empower designers with essential process information.

The overarching objective of the ASIC flow is to transform RTL designs into the GDSII format, which serves as the final layout for chip manufacturing. This flow is essentially a software-driven process, often referred to as automated Place & Route (PnR). It combines RTL IPs, EDA tools, and PDK data to create a seamless path from design concept to physical chip realization.

OpenLANE, with its suite of open-source tools, plays a pivotal role in this ecosystem, enabling designers to harness the power of open-source for ASIC development.

By embracing open-source principles, the world of Electronic Design Automation has seen a democratization of resources and a surge in collaborative innovation, making sophisticated chip design accessible to a broader community of engineers and researchers.
