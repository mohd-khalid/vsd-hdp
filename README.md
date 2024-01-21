# VSD-HDP
🚀 This repository is created as part of the VLSI System Design - Hardware Design Program (VSD-HDP). It will provide an inclusive and detailed documentation of the tasks and milestones achieved throughout this internship, which will ultimately lead to a full RTL2GDS tapeout-ready chip design.



## Day 0
  This is the day prior to the actual program start to ensure the following:

  ####  System Specs Check ⚡️
  - 400 GB SSD | 8 GB RAM | Ubuntu 20.04 LTS (Not on a virtul Machine)
  #### Create the Github repository ✅
  - [Github 🔗](https://github.com/mohd-khalid/vsd-hdp/)
  ####  EDA tools installation 🛠
  The entirety of the project will be done using free open-source software
  - Yosys, iverilog and GTKWave will be installed initially.
  - Other tools will be introduced in later stages of the project (OpenSTA, NGSpice, Magic, OpenLANE)


### Tools Installation

Firstly, update and upgrade the system

```bash
  sudo apt-get update
  sudo apt-get upgrade
```

#### Yosys
```bash
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make
$ sudo apt-get install build-essential clang bison flex \
libreadline-dev gawk tcl-dev libffi-dev git \
graphviz xdot pkg-config python3 libboost-system-dev \
libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
$ make
$ sudo make install
```
![Screenshot from 2024-01-21 17-07-49](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/b79b74cf-70fd-416f-9ca8-f521a1280323)

#### iVerilog
```bash
sudo apt-get install iverilog
```
![Screenshot from 2024-01-21 17-14-14](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/9d069fcf-29d6-465b-89ed-5cf71f3f635f)

#### GTKWave
```bash
sudo apt install gtkwave
```
![Screenshot from 2024-01-21 17-17-39](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/cd2f8d3d-d8c9-4852-9c8b-f7959aeb334d)
   
