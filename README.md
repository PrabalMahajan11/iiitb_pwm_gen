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

### 5.2 Synthesizer 
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
To install magic Goto home directory
```
$   git clone https://github.com/RTimothyEdwards/magic
$   cd magic/
$   ./configure
$   sudo make
$   sudo make install
```

Type magic terminal to check whether it installed succesfully or not. type exit to exit magic.

### 7.5 Generating Layout with existing library cells

NON-INTERACTIVE MODE: Here we are generating the layout in the non-interactive mode or the automatic mode. In this we cant interact with the flow in the middle of each stage of the flow.The flow completes all the stages starting from synthesis until you obtain the final layout and the reports of various stages which specify the violations and problems if present during the flow.
- Open terminal in home directory
```
$   cd OpenLane/
$   cd designs/
$   mkdir iiitb_pwm_gen
$   cd iiitb_pwm_gen/
$   wget https://raw.githubusercontent.com/PrabalMahajan11/iiitb_pwm_gen/main/config.json
$   mkdir src
$   cd src/
$   wget https://raw.githubusercontent.com/PrabalMahajan11/iiitb_pwm_gen/main/iiitb_pwm_gen.v
$   cd ../../../
$   sudo make mount
$   ./flow.tcl -design iiitb_pwm_gen
```

![flow_comp](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/2ea4a434-1c34-452c-bc25-0f7bd98f9f09)

To see the layout we use a tool called magic which we installed earlier.

Open terminal in home directory

```
$   cd OpenLane/designs/iiitb_pwm_gen/run
$   ls
```
Select most run directory from the list.

Example

![runfile](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/c517e0c0-10c2-490e-8b67-5f57c486de92)

```
$ cd RUN_2023.10.29_20.50.45
```

Run following instruction:
```
$   cd results/final/def
```

Update the highlited text with appropriate path

![commandforlayoutwithoutsky](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/85bc00e6-6a7c-4c9d-9c13-d344444c3312)


```
magic -T /home/prabal/Desktop/iiitb_pwm_gen/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../../tmp/merged.max.lef def read iiitb_pwm_gen.def &
```

Layout will be opened in new window.

#### Layout without sky130_vsdinv
- The final layout obtained after the completion of the flow in non-interactive mode is shown below:

![layoutwithoutsky](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/0e364adb-332e-4398-ac14-cabac44b87fd)


### 7.6 Customizing the layout
#### 7.6.1 sky130_vsdinv cell creation
Here we are going to customise our layout by including our custom made sky130_vsdinv cell into our layout.
- CREATING THE SKY130_VSDINV CELL LEF FILE.
- You need to first get the git repository of the vsdstdccelldesign.To get the repository type the following command:
```
$ git clone https://github.com/nickson-jose/vsdstdcelldesign
```
![clonevsdiat](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/43716685-9784-4f4b-80dc-27c00e4373b6)

![vsdls-ltr](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/695a8a65-7b42-4659-98fb-ce3edfebdf4a)


- Now you need to copy your tech file sky130A.tech to this folder.

- Next run the magic command to open the sky130_vsdinv.mag file.Use the following command:
```
$ magic -T sky130A.tech sky130_inv.mag 
```
#### 7.6.2 Layout of inverter cell


![layoutofsky_130_inv](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/08f60f3a-6aff-4b4b-be56-485f09518747)

- The next step is setting port class and port use attributes. The "class" and "use" properties of the port have no internal meaning to magic but are used by the LEF and DEF format read and write routines, and match the LEF/DEF CLASS and USE properties for macro cell pins. These attributes are set in tkcon window (after selecting each port on layout window. A keyboard shortcut would be,repeatedly pressing s till that port gets highlighed).

The tkcon command window of the port classification is shown in the image below:


![metal1](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/29f9cc5a-2f10-408c-b317-6bb7b5eff4a1)

![vpwr_masklayers](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/d003975c-d9ae-4270-8a24-afcdbe25f868)



![input_nmos_pmos](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/cb018299-e13e-4378-807f-4ccd7eb78a4c)


![Y_output](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/aca19246-b0a9-4d99-b64e-dae29e5da8c5)

#### 7.6.3 Generating lef file
- In the next step, use lef write command to write the LEF file with the same nomenclature as that of the layout .mag file. This will create a sky130_vsdinv.lef file in the same folder.

In tkcon terminal type the following command to generate .lef file
```
% lef write sky130_vsdinv
```
![write_lef](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/8e33f9ac-9ebe-4b2c-87f0-285bb3566c6f)
![foldershowing_sky130_vsdin](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/217d28e6-cf79-4bc3-8e2c-d3769e21b849)

Copy the generated lef file to designs/iiit_pwm_gen/src Also copy lib files from vsdcelldesign/libs to designs/iiit_pwm_gen/src

![copy_files](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/d7f87804-6dd1-40cb-8ff5-7852513e09f1)

Next modify the config.json file in our design to folloing code:


![config_json_in_folder](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/ec6d8fe7-bf82-4535-83bb-1c4c58c81eba)



![edited_config](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/728a6d4e-960a-44d2-9dd5-5a36f3f72ed7)


### 7.7 Generating Layout which includes custom made sky130_vsdinv
#### 7.7.1 Invoking OpenLane

Go to openlane directory and open terminal there
```
$ sudo make mount
```
![Screenshot from 2024-01-30 16-16-05](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/d17ada24-1ac4-4b21-b192-dac5e0e36d58)



- INTERACTIVE MODE: We need to run the openlane now in the interactive mode to include our custom made lef file before synthesis.Such that the openlane recognises our lef files during the flow for mapping.

- Running openlane in interactive mode: The commands to the run the flow in interactive mode is given below:

```
$ ./flow.tcl -interactive
```

Loading the package file
```
% package require openlane 0.9
```
![Screenshot from 2024-01-30 16-17-36](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/fbc512df-6588-41e4-bdd7-26e5092e62bc)

#### 7.7.2 Preparing the Design
- Preparing the design and including the lef files: The commands to prepare the design and overwite in a existing run folder the reports and results along with the command to include the lef files is given below:

```
% prep -design iiitb_pwm_gen
```
![Screenshot from 2024-01-30 16-26-28](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/6f820221-1673-4a8e-a9c4-0fb62f49dab1)

Include the below command to include the additional lef (i.e sky130_vsdinv) into the flow:
```
% set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
% add_lefs -src $lefs
```

![Screenshot from 2024-01-30 16-26-45](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/2b164bc4-6ad4-4f76-a2ba-3ed045137858)


#### 7.7.3 Synthesis
Logic synthesis uses the RTL netlist to perform HDL technology mapping. The synthesis process is normally performed in two major steps:
- GTECH Mapping – Consists of mapping the HDL netlist to generic gates what are used to perform logical optimization based on AIGERs and other topologies created from the generic mapped netlist.
- Technology Mapping – Consists of mapping the post-optimized GTECH netlist to standard cells described in the PDK

To synthesize the code run the following command
```
% run_synthesis
```
![Screenshot from 2024-01-31 14-23-09](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/16f66f1e-f03f-4ada-b63a-fbc8094ec892)




Statistics after synthesis
To check post synthesis statistics, visit the following directory:
```
/Desktop/iiitb_pwm_gen/OpenLane/designs/iiitb_pwm_gen/runs/RUN_2024.01.31_08.52.17/reports/synthesis$ cat 1-synthesis.AREA_0.stat.rpt
```
![Screenshot from 2024-01-31 14-30-33](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/b5664b22-ecd1-47ff-b7a4-001be37a9756)


Calculation of Flop Ratio:
```
             Number of D Flip flops 
Flop ratio = -------------------------
             Total Number of cells
```
Here, total number of flip flops = 38 + 2.
Total number of cells = 179.

Flip-Flop to standard cell ratio = 40/179 = 0.22346

#### 7.7.4 Floorplan
Goal is to plan the silicon area and create a robust power distribution network (PDN) to power each of the individual components of the synthesized netlist. In addition, macro placement and blockages must be defined before placement occurs to ensure a legalized GDS file. In power planning we create the ring which is connected to the pads which brings power around the edges of the chip. We also include power straps to bring power to the middle of the chip using higher metal layers which reduces IR drop and electro-migration problem.

- Importance of files in increasing priority order:
    ```floorplan.tcl``` - System default envrionment variables ```conifg.tcl``` ```sky130A_sky130_fd_sc_hd_config.tcl```
- Floorplan environment cariables or switches:
  -  ```FP_CORE_UTIL``` - floorplan core utilisation
  -  ```FP_ASPECT_RATIO``` - floorplan aspect ratio
  -  ```FP_CORE_MARGIN``` - Core to die margin area ```FP_IO_MODE``` - defines pin configurations (1 = equidistant/0         = not equidistant)
  -  ```FP_CORE_VMETAL``` - vertical metal layer
  -  ```FP_CORE_HMETAL``` - horizontal metal layer
- Note: Usually, vertical metal layer and horizontal metal layer values will be 1 more than that specified in the file
- Following command helps to run floorplan
```
% run_floorplan
```
![Screenshot from 2024-01-31 14-47-09](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/5756a5ef-32b1-482c-8b1b-764b821d3a80)


- Post the floorplan run, a .def file will have been created within the results/floorplan directory. We may review floorplan files by checking the floorplan.tcl. The system defaults will have been overriden by switches set in conifg.tcl and further overriden by switches set in sky130A_sky130_fd_sc_hd_config.tcl.
- To view the floorplan: Magic is invoked after moving to the results/floorplan directory,then use the following command:

```
magic -T /home/prabal/Desktop/iiitb_pwm_gen/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read iiitb_pwm_gen.def &
```

Floorplan

![Screenshot from 2024-01-31 15-07-36](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/ac4df466-153f-4dbd-acc5-621b492ded3e)

```Die area (post floor plan)```

![Screenshot from 2024-01-31 15-06-05](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/698accb9-8323-4d62-aafa-e10d43e127f2)


```Core area (post floor plan)```

![Screenshot from 2024-01-31 15-06-21](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/05450e49-e161-4a88-8900-f95b4d001a38)


#### 7.7.5 Placement

Place the standard cells on the floorplane rows, aligned with sites defined in the technology lef file. Placement is done in two steps: Global and Detailed. In Global placement tries to find optimal position for all cells but they may be overlapping and not aligned to rows, detailed placement takes the global placement and legalizes all of the placements trying to adhere to what the global placement wants.
- The next step in the OpenLANE ASIC flow is placement. The synthesized netlist is to be placed on the floorplan. Placement is perfomed in 2 stages:
- Global Placement: It finds optimal position for all cells which may not be legal and cells may overlap. Optimization is done through reduction of half parameter wire length.
- Detailed Placement: It alters the position of cells post global placement so as to legalise them.

run the following command to run the placement
```
% run_placement
```

#### Clock Tree Synthesis (CTS)

- Clock tree synteshsis is used to create the clock distribution network that is used to deliver the clock to all sequential elements. The main goal is to create a network with minimal skew across the chip. H-trees are a common network topology that is used to achieve this goal.
- The purpose of building a clock tree is enable the clock input to reach every element and to ensure a zero clock skew. H-tree is a common methodology followed in CTS. Before attempting a CTS run in TritonCTS tool, if the slack was attempted to be reduced in previous run, the netlist may have gotten modified by cell replacement techniques. Therefore, the verilog file needs to be modified using the write_verilog command. Then, the synthesis, floorplan and placement is run again
- Run the following command to perform CTS

```
% run_cts
```
