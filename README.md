# 🚀  VSD-HDP
This repository is created as part of the [VLSI System Design - Hardware Design Program (VSD-HDP)](https://www.vlsisystemdesign.com/hdp/). It will provide an inclusive and detailed documentation of the tasks and milestones achieved throughout this internship, which will ultimately lead to a full RTL2GDS tapeout-ready chip design.


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
   


## Day 1

###  RTL Design and Synthesis

The RTL design represents the implementation of certain specs in the form of a verilog file(s). In order to ensure that our RTL matches the desired spec, we simulate the design functionalities using simulators. The simulating tool used throughout this project is iverilog.

- #### Simulation
The simulation is carried out by inputting a stimulus (test_vectors) to the system and observing how the produced output matches the chip specs. The setup on which the stimuli are generated, applied and observed as they output the design is the Testbench (TB).

![Screenshot from 2024-01-25 16-59-25](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/85354b14-1f05-4a68-8659-af694c03c66e)

However, the TB itself has no primary input or output signals. The simulator receives inputs and evaluates the output accordingly. If the input didn't change, the output will remain unchanged as well.

-  ####  iverilog-based Simulation Flow

iverilog reads both the design and the Testbench, once the inputs change, iverilog will dump the change in the output, resulting in a .vcd format file (Value Change Dump). The vcd file is then visualized and displayed as a waveform using another tool called GTKWave.

![Screenshot from 2024-01-25 16-59-40](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/520ab252-c137-4c0e-93fd-d9babd637ab0)

The Testbench instantiates the design, the design instance of the TB is called: uut (or dut), short for [Unit (or Design) Under test].


### [Lab] SKY130RTL D1SK2 - Intro to iverilog and GTKWave

This lab will delve into simulateion of a simple verilog design (2:1 Multiplexer), verifying the design, and viewing it as a waveform on GTKWave. Used tools have been installed ealier.
- ### Lab 1
We initiate be cloning the Github repository that includes the verilog RTL designs, their corresponding Testbenches and synthesis-related standard liberaries.

```bash 
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```
We clone the repo and navigate through the files:

![Screenshot from 2024-01-22 23-11-24](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/54983c4b-13ff-4abf-b491-ed5985ba138d)

- ### Lab 2: Introducing iverilog and GTKWave
These lab aims at applying the previously demonstrated processes of simulating and visualizing the design and its Testbenche using iverilog and GTKWave, respectively.

#### Relevant Commands
To upload a design (design.v) and its TB (tb_design.v) to iverilog: `read_verilog design.v tb_design_mux.v`

For our 2:1 Multiplexer we use:

```bash
read_verilog good_mux.v tb_good_mux.v
```

![Screenshot from 2024-01-22 23-30-431](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/bbe413be-53fc-40b7-bbe4-6995f0b8f91b)

We notice a new file created in the same directory named: `a.out` Executing this newly created file (using the comand `./a.out`) will generate, the previously explained, .vcd file (called: tb_good_mux.vcd).

Eventually, we load the resulting VCD  to our wave viewer GTKWave using the command:

```bash
gtkwave tb_good_mux.vcd
```
![Screenshot from 2024-01-22 23-30-43](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/d667ee41-8fe9-446f-8c16-88eed5c853d9)



Once the command runs, a new winsow will pop up of the GTKWave, it shows the waveforms of all the signals of the chip: inputs `i1, i0 and sel` and outputs `y` and how the input changes will affect the  output.

![Screenshot from 2024-01-22 23-39-05](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/44345433-7a49-430d-87bb-163e1cf1197b)


### Design Synthesis

Synthesis is the process of converting an RTL design into a netlist. The synthesizer used in this project is the open-source tool: Yosys.

Similar to iverilog, Yosys reads two files, the design and the standard cell library (.lib), and produces the netlist, which is the design representation in the form of standard cells in (.lib).



 #### Relevant Yosys commands: 

  - To read the design file: `read_verilog`

  - To read the (.lib) file: `read_liberty`

  - To generate the netlist file: `write_verilog`


![Screenshot from 2024-01-26 01-34-36](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/261ca6c5-675b-47ef-9302-8f63de8df433)


#### Synthesis Verification

The same approach of verifying RTL is followed. We provide the iverilog simulator with the netlist file and the corresponding TB, and consequently get the .vcd file output to be run on GTKWave.


![Screenshot from 2024-01-26 02-07-29](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/228a50d2-ef67-4a71-8982-0007cf282745)


The waveform must match the output observed during the RTL simulation. Since the set of primary inputs and outputs are the same on both the RTL and netlist, the same testbench can be used to verify both.

#### Logic Synthesis 

The RTL code is a behavioral representation of a certain specifications. Synthesis is translating that RTL code to Gate Leve, where the RTL code is mapped to actual gates and interconnections between these gates. The generated file that contains this data (gates and connections) is called a netlist.
The synthesis tool receives the RTL along with a Front End Lib and gives out the netlist.


![Screenshot from 2024-01-26 02-10-56](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/6c4032d6-4c69-4287-883f-38d6b0aa3069)


- Libraries `.lib` 

Contain a collection of logical modules. It contains basic logic gates (AND, OR, XOR, etc.). It also contains different variations of the same gate in terms of various parameters e.g. number of inputs, performance (slow, fast gate), etc.

The combinational delay in the logic path raises the need for different variations of the same gate.

The clock cycle time should be long enough to accommodate all of the propagation delay of the flip-flops (FF), the combinational delay and the setup delay. The sum of these delays must be shorter than the clk cycle. The shorter the clock cycle `(T_clk)` the higher the frequency `(f_clk)`.  To ensure better performance we aim to reduce the delays to obtain a high frequency circuit. However, there is still a need for slower circuits to ensure we don't violate the hold timings `(T_hold)`. This set of various speed gate variations forms the `.lib`.

The load in a digital IC is a capacitor. How fast this capacitor charges and discharges determines the gate delay. In order to speed up the charging process we need to supply the capacitor with higher amperage. This is obtained using wider transistors. However, this short delay was attained at cost of area (wide transistors) and power (greater consumption). Whereas narrow transistors will be more efficient in terms of area and power, but longer delays in contrast.

- #### Cell Selection Trade-offs

The synthesizer needs to optimize the cells selection for logic circuits implementation. On one hand,  excessive use of fast cells will increase the chip area and power consumption along with Hold time violations. On the other hand, using slow cells solely will result in a sluggish circuit that performs poorly. Hence, we use “constraints” to optimize the synthesizer choice of cells.



### [Lab] SKY130RTL D1SK4 - Yosys and SKY130 PDKs

To synthesize our design using Yosys, we, firstly, invoke yosys

```bash
yosys
```


Then, we read our library and (.lib file) and our design (verilog file):

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
```

We'll get the following message `Successfully finished Verilog frontend`. 
Then we synthesize our design, using `synth -top <design_name>`

```bash
synth -top good_mux
```



Once the process is done, we get brief statistics of our design components. Then we generate our netlist using `abc -liberty` followed by our `sky130_fd_sc_hd__tt_025C_1v80.lib ` directory, we can use an absolute or relative path:

```bash
abc -liberty .. /lib/sky130_fd_sc_hd__tt_025C_1v80.lib ![1](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/ecfa772b-1e15-4ce7-8992-ab29a179f275)

```

Our design is now converted into actual gates and wires, using the cells present in `sky130_fd_sc_hd__tt_025C_1v80.lib`. and a breif of the used cells is provided.

![2](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/c674fcd9-feca-4c2c-9b34-09be927a02f6)


![3](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/b0ccf8f3-58f7-46ae-a918-f660456d8ab1)




To get a graphical representation that illustrate our design, command `show` is used.

![4](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/8d4def02-0f99-4f7c-bda8-3611c13b334a)


Eventually, to generate our netlist file `write_verilog good_mux_netlist.v` as stated before. this will produce a detailed verilog file, to have a more concise version we use:

```bash
write_verilog -noattr good_mux_netlist.v
```


![5](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/e331932a-e090-4017-bb3f-ecb411b058b0)



The following verilog file `good_mux_netlist.v` was created:


![6](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/6a26188d-413b-4ac5-91fb-f3efd8fef555)



