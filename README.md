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


### [Lab] SKY130RTL - Intro to iverilog and GTKWave

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

```verilog

/* Generated by Yosys 0.37+21 (git sha1 8649e3066, gcc 9.4.0-1ubuntu1~20.04.2 -fPIC -Os) */

module good_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule

```


## Day 2


### Timing Libraries

Libraries content is described in terms of three critical parameters and that is Process, Voltage and Temperature (PVT). PVT describes the variations due fabrication, voltage and temperature and how the chip will function under these circumstances. The library file shows these operation conditions in the first line: `sky130_fd_sc_hd__tt_025C_1v80.lib`.

- `tt` typical Process 

- `025C` Temperature

- `1v80` Voltage 


It also contains data about used technology (CMOS in our case) and the measurement units used for all relevant quantities.


For each cell, the library states the data about power leakage  area, input pins (its capacitance, power, etc.), power port, timing and more for all the possible input combinations to that cell.

An example of a cell might be a simple 2-input AND gate in the `.lib`, here is a brief sample of the data it contains:

```lib
 cell ("sky130_fd_sc_hd__and2_0") {
        leakage_power () {
            value : 0.0021372000;
            when : "!A&B";
        }
        leakage_power () {
            value : 0.0018183000;
            when : "!A&!B";
        }
        leakage_power () {
            value : 0.0015938000;
            when : "A&B";
        }
        leakage_power () {
            value : 0.0021392000;
            when : "A&!B";
        }
        area : 6.2560000000;

```
The power leakage  info is calculated for all the 4 possible input combinations to that gate.

The verilog model of this cell can be found in `..my_lib/verilog_model/sky130_fd_sc_hd.v`

#### Verilog model

```verilog
`ifndef SKY130_FD_SC_HD__AND2_0_V
`define SKY130_FD_SC_HD__AND2_0_V

/**
 * and2: 2-input AND.
 * Verilog wrapper for and2 with size of 0 
 * WARNING: This file is autogenerated, do not modify directly!
 */
`timescale 1ns / 1ps
`default_nettype none

`ifdef USE_POWER_PINS
/*********************************/

`celldefine
module sky130_fd_sc_hd__and2_0 (
    X   ,
    A   ,
    B   ,
    VPWR,
    VGND,
    VPB ,
    VNB
);

    output X   ;
    input  A   ;
    input  B   ;
    input  VPWR;
    input  VGND;
    input  VPB ;
    input  VNB ;
    sky130_fd_sc_hd__and2 base (
        .X(X),
        .A(A),
        .B(B),
        .VPWR(VPWR),
        .VGND(VGND),
        .VPB(VPB),
        .VNB(VNB)
    );

endmodule
`endcelldefine
```









We also notice different variations of the 2-input AND gate in our `.lib`

```verilog
    cell ("sky130_fd_sc_hd__and2_0") {
        leakage_power () {
            value : 0.0021372000;
            when : "!A&B";
        }
        area : 6.2560000000;
```

```verilog
    cell ("sky130_fd_sc_hd__and2_2") {
        leakage_power () {
            value : 0.0039778000;
            when : "!A&B";
        }
        area : 7.5072000000;
```
```verilog
    cell ("sky130_fd_sc_hd__and2_4") {
        leakage_power () {
            value : 0.0045182000;
            when : "!A&B";
        }
        area : 8.7584000000;
```

We observe an increase in power leakage and area, implying wider transistors and consequently faster cell performance as explained previously.

### Hierarchical vs. Flat Synthesis

The explore the different synthesis approaches, we will use a simple design that includes two sub-modules instantiated by a main module. Our design `multiple_modules.v` consists of two simple 2-input gates modules `sub_module1` and `sub_module2`, and the overall design is a 3-input single-output module `multiple_modules`:


```verilog
module sub_module2 (input a, input b, output y);
	assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
	assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule
```

We invoke Yosys, read our design and library, synthesize the design then map it to our SKY130 technology:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

And to view our design `show multiple_modules`:


![1](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/4694e0ab-382b-49c2-9596-5c1669d8436b)


hierarchy of our design modules after synthesis as shown. To write out the netlist:

```bash
write_verilog -noattr multiple_modules_hier.v
```

The following verilog file was generated:

```verilog
/* Generated by Yosys 0.37+21 (git sha1 8649e3066, gcc 9.4.0-1ubuntu1~20.04.2 -fPIC -Os) */

module multiple_modules(a, b, c, y);
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  wire net1;
  output y;
  wire y;
  sub_module1 u1 (
    .a(a),
    .b(b),
    .y(net1)
  );
  sub_module2 u2 (
    .a(net1),
    .b(c),
    .y(y)
  );
endmodule

module sub_module1(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  wire a;
  input b;
  wire b;
  output y;
  wire y;
  sky130_fd_sc_hd__and2_0 _3_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  assign _1_ = b;
  assign _0_ = a;
  assign y = _2_;
endmodule

module sub_module2(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  wire a;
  input b;
  wire b;
  output y;
  wire y;
  sky130_fd_sc_hd__or2_0 _3_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  assign _1_ = b;
  assign _0_ = a;
  assign y = _2_;
endmodule
```
This represents a **Hierarchical** netlist. Where the hierarchy of modules is preserved. To a the flat-synthesis netlist we use the command `flatten` then generate the netlist verilog:

```bash
flatten
write_verilog -noattr multiple_modules_flat.v
```

The design was flattened out into a single module, without any submodules:

```verilog
/* Generated by Yosys 0.37+21 (git sha1 8649e3066, gcc 9.4.0-1ubuntu1~20.04.2 -fPIC -Os) */

module multiple_modules(a, b, c, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  wire _5_;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  wire net1;
  wire \u1.a ;
  wire \u1.b ;
  wire \u1.y ;
  wire \u2.a ;
  wire \u2.b ;
  wire \u2.y ;
  output y;
  wire y;
  sky130_fd_sc_hd__and2_0 _6_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  sky130_fd_sc_hd__or2_0 _7_ (
    .A(_4_),
    .B(_3_),
    .X(_5_)
  );
  assign _4_ = \u2.b ;
  assign _3_ = \u2.a ;
  assign \u2.y  = _5_;
  assign \u2.a  = net1;
  assign \u2.b  = c;
  assign y = \u2.y ;
  assign _1_ = \u1.b ;
  assign _0_ = \u1.a ;
  assign \u1.y  = _2_;
  assign \u1.a  = a;
  assign \u1.b  = b;
  assign net1 = \u1.y ;
endmodule
```

![2](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/ad82538a-cf76-46fb-b1bc-0058a7e23239)



It is also possible to synthesize a submodule individually. 

```bash
read_liberty -lib  ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```

![3](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/35043ae0-804b-4c67-b761-ab31ad77eb15)



This might be for 2 reasons:
- There are multiple instances of the same module. Hence, it is optimal to synthesize it once and reuse the netlist.
- It can be used as a **Divide & Conquer** approach in complex designs; to make the synthesis process more efficient.


### Flops Coding Styles and Optimization

Combinational circuits inherently have a delay issue. The change in input cause the output to change **after** the propagation delay, causing the output to glitch in mult-stage circuits. This raises the need to storage elements (i.e. Flops) to stabilize input and output signals by shielding the flop output (Q) from input changes (d). We also use set/reset signals as well to initialize the flop, otherwise if the initial flop state was unknown the output would be useless.


- #### Coding Styles

There is different variations of flops in terms of the set/reset signals and synchronicity/asynchronicity. The following will show simulation and synthesis of a d-FlipFlop (dFF) with:
- asynchronous reset
- asynchronous set
- synchronous reset

We will view the waveform and observe how the set/reset signal affect the output **Q** with respect the clock signal **clk**.

### [Lab] SKY130RTL - Flop Synthesis and Simulation

- **Asynchronous reset dFF**

```verilog
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

To simulate our design, we invoke `iverilog` and use the following commands:

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

- Waveform

![1](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/174d6bcb-e410-40d8-92ea-ab0212598d5f)




Output **Q** was immediately reset to **Low**, before the next  `clk` edge

To synthesize the design, we invoke `yosys` and command as follow:

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
**Note:** We used the same approach except for the command `dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` which determines the flop library to be used as it might be sepreate in some technologies. However, we use the same library for standard cells and flops.



![2](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/747d6db2-c36d-44b1-8df6-1173c199c360)



- **Asynchronous set dFF**

```verilog
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```

To simulate our design, we invoke `iverilog` and use the following commands:

```bash
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

- Waveform


![3](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/69a479c9-5c04-45e1-8e7f-80af3deda9f7)



Output **Q** was immediately set to **High**, before the next `clk` edge

To synthesize the design, we invoke `yosys` and command as follow:

```bash
yosys
read_verilog dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![4](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/4afd4366-1832-4810-86ed-d02d2f885008)



- **Synchronous reset dFF**

```verilog
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```

To simulate our design, we invoke `iverilog` and use the following commands:

```bash
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

- Waveform


![5](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/277a56d3-df7a-4cc2-b603-a441f10c52e6)



Output **Q** was not reset to **Low** until the `clk` edge

To synthesize the design, we invoke `yosys` and command as follow:

```bash
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show

```

![6](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/cd45ed23-e924-49a0-ac78-4f6295382412)


## Day 3
### Combinational and Sequential Optmizations

Combinational circuits inherently have a delay issue. The change in input cause the output to change **after** the propagation delay, causing the output to glitch in mult-stage circuits. This raises the need to storage elements (i.e. Flops) to stabilize input and output signals by shielding the flop output (Q) from input changes (d). We also use set/reset signals as well to initialize the flop, otherwise if the initial flop state was unknown the output would be useless.

- Combinational optimization
We aim at squeezing the logic into the optimum design in terms of area and power consumption, using various techniques, for example:
**Constant propagation:** Using straightforward techniques like K-maps and Quine-McLusky. Results in less transistor count and consequently less area and power demand.
**Boolean logic optimization:** By algebraically simplifying the boolean equation.

- Sequential optimization
There is basic techniques (e.g. constant propagation) and advanced approaches such as state optimization, Sequential logic cloning (Floor plan aware synthesis) and Retiming

### [Lab] SKY130RTL - Combinational Logic Optimizations

The following are four combinantional designs named `opt_check` that will be synthisized then optimized from a multi-muliplexer designs to a single gate design. 

- For optimizing a new command is introduced: `opt_clean`, according to **Yosys Command line reference** it identifies wires and cells that are unused and removes them. We will use the command along with `-purge` which removes internal nets if they have a public name.



#### 1. opt_check

- Verilog Code 
```verilog
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

- Synthesis and Optimization
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog opt_check.v 
synth -top opt_check 
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```
- Design Schematics:



![1](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/5314bdc6-58ac-4f1f-8dc4-b1e70431db38)




#### 2. opt_check2

- Verilog Code 

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

- Synthesis and Optimization

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog opt_check2.v 
synth -top opt_check2 
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```
Design Schematics:


![2](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/9d7cd9e6-b807-4e24-91d7-887997ef3457)




#### 3. opt_check3

- Verilog Code 

```verilog

module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```

- Synthesis and Optimization

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog opt_check3.v 
synth -top opt_check3 
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```
Design Schematics:


![3](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/026ddf72-3450-4e41-904a-a8e878df2409)



#### 4. opt_check4

- Verilog Code 

```verilog
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
 endmodule
```
- Synthesis and Optimization

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog opt_check4.v 
synth -top opt_check4 
opt_clean -purge
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
```
Design Schematics:


![4](https://github.com/mohd-khalid/vsd-hdp/assets/97974068/8c7ab1c2-9ced-4e20-88e2-3360ca928a42)




