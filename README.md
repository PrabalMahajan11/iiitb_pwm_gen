# iiitb_pwm_gen -> Pulse Width Modulated Wave Generator with Variable Duty Cycle
This project simulates the designed Pulse Width Modulated Wave Generator with Variable Duty Cycle. We can generate PWM wave and varry its DUTYCYCLE in steps of 10%.

- [1. Introduction to PWM Generator](#1-Introduction-to-PWM-Generator)

- [2. Applications](#2-Applications)
  
- [3. Blocked Diagram of PWM GENERATOR](#3-Blocked-Diagram-of-PWM-GENERATOR)

- [4. Functional Simulation](#4-Functional-Simulation)
  - [4.1 About iverilog](#41-About-iverilog)
  - [4.2 About GTKWave](#42-About-GTKWave)
  - [4.3 Installing iverilog and GTKWave](#43-Installing-iverilog-and-GTKWave)
  - [4.4 Functional Simulation Process](#44-Functional-Simulation-Process)
  - [4.5 Functional Characteristics](#45-Functional-Characteristics)
  
- [5. SYNTHESIS](#5-SYNTHESIS)
  - [5.1 About Synthesys](#51-About-Synthesys)
  - [5.2 Synthesizer](#52-Synthesizer)
  
- [6. GATE LEVEL SIMULATION](#6-GATE-LEVEL-SIMULATION)
  - [6.1 About GLS](#61-About-GLS)
  - [6.2 Running GLS](#62-Running-GLS)
  - [6.3 Observations from GLS Waveforms](#63-Observations-from-GLS-Waveforms)
  
## 1. Introduction to PWM Generator
Pulse Width Modulation is a famous technique used to create modulated electronic pulses of the desired width. The duty cycle is the ratio of how long that PWM signal stays at the high position to the total time period.![pwm](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/8c7a5a06-e3de-422c-9d5a-798512dbfe89)

## 2. Applications
Pulse Width Modulated Wave Generator can be used to
- control the brightness of the LED
- drive buzzers at different loudnes
- control the angle of the servo motor
- encode messages in telecommunication
- used in speed controlers of motors

## 3. Block Diagram of PWM Generator
This PWM generator generates 10Mhz signal. We can control duty cycles in steps of 10%. The default duty cycle is 50%. Along with clock signal we provide another two external signals to increase and decrease the duty cycle.

![BasicBlockedDeagram](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/af440fa6-4220-4390-a286-0ae447277f97)

In this specific circuit, we mainly require a n-bit counter and comparator. Duty given to the comparator is compared with the current value of the counter. If current value of counter is lower than duty then comparator results in output high. Similarly, If current value of counter is higher than duty is then comparator results in output low. As counter starts at zero, initially comparator gives high output and when counter crosses duty it becomes low. Hence by controlling duty, we can control duty cycle.
![BlockDiagram](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/f6cb3183-715a-4a01-b97d-2b901fa5bf08)
As the comparator is a combinational circuit and the counter is sequential, while counting from 011 to 100 due to improper delays there might be an intermediate state like 111 which might be higher or lower than duty. This might cause a glitch. To avoid these glitches output of the comparator is passed through a D flipflop.

## 4. Functional Simulation
### 4.1 About iVerilog
Icarus Verilog is an implementation of the Verilog hardware description language.

### 4.2 About GTKWave
GTKWave is a fully featured GTK+ v1. 2 based wave viewer for Unix and Win32 which reads Ver Structural Verilog Compiler generated AET files as well as standard Verilog VCD/EVCD files and allows their viewing

### 4.3 Installing iVerilog and GTKWave
#### For Ubuntu
Open your terminal and type the following to install iverilog and GTKWave

```
$   sudo apt-get update
$   sudo apt-get install iverilog gtkwave
```

### 4.4 Functional Simulation Process
To clone the Repository and download the Netlist files for Simulation, enter the following commands in your terminal.
```
$   sudo apt install -y git
$   git clone https://github.com/PrabalMahajan11/iiitb_pwm_gen
$   cd iiitb_pwm_gen
$   iverilog iiitb_pwm_gen.v iiitb_pwm_gen_tb.v
$   ./a.out
$   gtkwave pwm.vcd
```

### 4.5 Functional Characteristics

<img width="1237" alt="pwm" src="https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/d584f807-5cd9-467f-ac28-1dd65f8a9872">

#### Simulation Results while increasing Dutycycle
<img width="1037" alt="pwmI" src="https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/4c584e6c-b68d-44c9-ae6b-23ee632a683d">


#### Simulation Results while decreasing Dutycycle
<img width="1248" alt="pwmD" src="https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/6fc0c061-6327-471a-a531-569e5f4b5bf7">

## 5. Synthesis
### 5.1 About Synthesis
**Synthesis:** Synthesis transforms the simple RTL design into a gate-level netlist with all the constraints as specified by the designer. In simple language, Synthesis is a process that converts the abstract form of design to a properly implemented chip in terms of logic gates.

Synthesis takes place in multiple steps:

- Converting RTL into simple logic gates.
- Mapping those gates to actual technology-dependent logic gates available in the technology libraries.
- Optimizing the mapped netlist keeping the constraints set by the designer intact.

### 5.1 Synthesizer 
**Synthesizer:** It is a tool we use to convert out RTL design code to netlist. Yosys is the tool I've used in this project.

#### About Yosys
Yosys is a framework for Verilog RTL synthesis. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains.

- more at https://yosyshq.net/yosys/

To install yosys follow the instructions in this github repository  
- https://github.com/YosysHQ/yosys
Now you need to create a yosys_run.sh file , which is the yosys script file used to run the synthesis. The contents of the yosys_run file are given below:

 **note:** Identify the .lib file path in cloned folder and change the path in highlighted text to indentified path
![yosys](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/dee4315f-848c-439f-806c-fdbad97c2764)

Now, in the terminal of your verilog files folder, run the following commands:
- Run the following commands to syhthesize.
```
$   yosys
$   yosys>    script yosys_run.sh
```
To see different types of cells after synthesis.
```
$   yosys>    stat
```
- To generate schematics
```
$   yosys>    show
```
Now the synthesized netlist is written in ``` iiitb_pwm_gen_synth.v ``` file.

## 6. Gate Level Simulation
### 6.1 About GLS
GLS is generating the simulation output by running test bench with netlist file generated from synthesis as design under test. Netlist is logically same as RTL code, therefore, same test bench can be used for it.We perform this to verify logical correctness of the design after synthesizing it. Also ensuring the timing of the design is met.

### 6.2 Running GLS
Folllowing are the commands to run the GLS simulation:
In the verilog model folder, inside the iiitb_pwm_gen folder, run the following commands.
```
$ iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 primitives.v sky130_fd_sc_hd.v iiitb_pwm_gen_synth.v iiitb_pwm_gen_tb.v
$ ./a.out
$ gtkwave pwm.vcd
```

#### GLS Waveforms
![pwmgls](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/2842b7f8-1abf-4199-9446-66a4d3810591)

- Simulation Results while increasing Dutycycle
![pwmgls_increase](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/0a315d76-0eb9-419a-a45a-a3faa601c9fd)


- Simulation Results while decreasing Dutycycle
![pwmgls_decrease](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/71159502-3fb0-43e2-85eb-d25197ee9da8)

### 6.3 Observations from GLS Waveforms
Output characteristics of **Functional simulation** is matched with output of **Gate Level Simulation**.

## 7. Physical Design
### 7.1 Overview of Physical Design Flow
Place and Route (PnR) is the core of any ASIC implementation and Openlane flow integrates into it several key open source tools which perform each of the respective stages of PnR. Below are the stages and the respective tools that are called by openlane for the functionalities as described:
![physicaldesignflow](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/ab280b09-b144-4ca2-a834-bbd6bdd85d01)

### 7.2 OpenSource EDA tools
OpenLANE utilises a variety of opensource tools in the execution of the ASIC flow:

| Task  | Tool/s |
| ------------- | ------------- |
| RTL Synthesis & Technology Mapping	  | yosys, abc  |
| Floorplan & PDN  | init_fp, ioPlacer, pdn and tapcell  |
| Placement  | RePLace, Resizer, OpenPhySyn & OpenDP  |
| Static Timing Analysis  | OpenSTA  |
| Clock Tree Synthesis	  | TritonCTS  |
| Routing  | FastRoute and TritonRoute  |
| SPEF Extraction	  | SPEF-Extractor  |
| DRC Checks, GDSII Streaming out	  | Magic, Klayout  |
| LVS check		  | Netgen  |
| Circuit validity checker	  | CVC  |

### 7.3 OpenLane
OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, CU-GR, Klayout and a number of custom scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII.

#### OpenLane Design Stages
1. Synthesis
  - ```yosys``` - Performs RTL synthesis.
  - ```abc``` - Performs technology mapping.
  - ```OpenSTA``` - Performs static timing analysis on the resulting netlist to generate timing reports.
2. Floorplan and SDN
  - ```init_fp``` - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing).
  - ```ioplacer``` - Places the input and output ports.
  - ```pdn``` - Generates the power distribution network.
  - ```tapcell``` - Inserts welltap and decap cells in the floorplan.
3. Placement
  - ```RePlace``` - Performs global placement.
  - ```Resizer``` - Performs optional optimizations on the design.
  - ```OpenDP``` - Performs detailed placement to legalize the globally placed components.
4. CTS
  - ```TritonCTS``` - Synthesizes the clock distribution network (the clock tree).
5. Routing
  - ```FastRoute``` - Performs global routing to generate a guide file for the detailed router.
  - ```CU-GR``` - Another option for performing global routing.
  - ```TritonRoute``` - Performs detailed routing.
  - ```SPEF-Extractor``` - Performs SPEF extraction.
6. GDSII Generation
  - ```Magic``` - Streams out the final GDSII layout file from the routed def.
  - ```Klayout``` - Streams out the final GDSII layout file from the routed def as a back-up.
7. Checks
  - ```Magic``` - Performs DRC checks and antenna checks.
  - ```Klayout``` - Performs DRC checks.
  - ```Netgen``` - Performs LVC checks.
  - ```CVC``` - Performs circuit validity checks.
    
more at https://github.com/The-OpenROAD-Project/OpenLane

#### Openlane Installation instructions
 ```
$   apt install -y build-essential python3 python3-venv python3-pip
```
- Docker Installation Process: https://docs.docker.com/engine/install/ubuntu/
- Goto home directory->
```
$   git clone https://github.com/The-OpenROAD-Project/OpenLane.git
$   cd OpenLane/
$   sudo make
```
- To test the openLane
```
$ sudo make test
```
It takes approximate time of 5min to complete. After 43 steps, if it ended with saying Basic test passed then open lane installed succesfully.

### 7.4 Magic
Magic is a venerable VLSI layout tool, written in the 1980's at Berkeley by John Ousterhout, now famous primarily for writing the scripting interpreter language Tcl. Due largely in part to its liberal Berkeley open-source license, magic has remained popular with universities and small companies. The open-source license has allowed VLSI engineers with a bent toward programming to implement clever ideas and help magic stay abreast of fabrication technology. However, it is the well thought-out core algorithms which lend to magic the greatest part of its popularity. Magic is widely cited as being the easiest tool to use for circuit layout, even for people who ultimately rely on commercial tools for their product design flow.

More about magic at http://opencircuitdesign.com/magic/index.html

Run following commands one by one to fulfill the system requirement.
```
$   sudo apt-get install m4
$   sudo apt-get install tcsh
$   sudo apt-get install csh
$   sudo apt-get install libx11-dev
$   sudo apt-get install tcl-dev tk-dev
$   sudo apt-get install libcairo2-dev
$   sudo apt-get install mesa-common-dev libglu1-mesa-dev
$   sudo apt-get install libncurses-dev
```
  











