# Digital VLSI SoC Design and Planning
---
## Day-1
---
### 1.1   Introduction to package, chip, pads, core, die and IP's
---
- **Introduction**

![Image Description](Media/DAY%20-%201/1.1/Image%20(4).png)
- **Package:**
The physical casing that holds the silicon chip (die) and provides connections to the outside world. It protects the chip and allows it to be installed on a circuit board. Example: The black square or rectangle with metal pins you see on a PCB.

![Image Description](Media/DAY%20-%201/1.1/Image%20(2).png)
- **Chip:**
The complete electronic component that includes the silicon die, packaging, and connections. It is what we use in circuits, such as microcontrollers or processors.

![Image Description](Media/DAY%20-%201/1.1/Image%20(1).png)
- **Pad:**
A small metallic area on the die used to connect the internal circuits to the outside world through wires or bumps.

![Image Description](Media/DAY%20-%201/1.1/Image%20(3).png)
- **Core:**
A core is the fundamental processing unit within a chip that consists of the main hardware and executes instructions and performs computations.

![Image Description](Media/DAY%20-%201/1.1/Image%20(3).png)
- **Die:**
The small, thin piece of silicon inside the chip that contains the actual electronic circuits and components. It is where the transistors and other components are fabricated.

![Image Description](Media/DAY%20-%201/1.1/Image%20(6).png)
- **IP (Intellectual Property):**
Reusable design blocks or pre-designed functional units that can be integrated into a chip. Examples include designs for processors, memory controllers, or USB interfaces. Companies can license these IPs instead of designing them from scratch.
---
### 1.2 Introduction to RISC-V
---
To execute some C code on a hardware layout, the compiler first translates the high-level C code into a RISC-V assembly language program. This assembly code is a low-level representation of the program, which is then further converted into machine language—a binary format consisting of 1s and 0s—that can be directly executed by the hardware.

Additionally, an essential interface between the RISC-V architecture and the hardware layout is the use of Hardware Description Languages (HDLs), such as Verilog. HDLs are used to design and simulate the hardware structure, ensuring the binary instructions generated are compatible and can be correctly implemented on the hardware.

This process bridges the gap between software code and physical hardware implementation, enabling seamless execution of programs on custom hardware designs.  

![Image Description](Media/DAY%20-%201/1.2/Image.png)
![Image Description](Media/DAY%20-%201/1.3/Image%20(1).png)
![Image Description](Media/DAY%20-%201/1.3/Image%20(2).png)
---
---
### 1.3 Introduction to Openlane  
--- 

![Image Description](Media/DAY%20-%201/2/Image%20(1).png)
![Image Description](Media/DAY%20-%201/2/Image%20(2).png)
- **OpenLane** is an open-source, end-to-end physical design automation (EDA) toolchain for digital ASIC design. It takes an RTL design as input and produces a manufacturable GDSII layout as output. Built on top of open-source tools like Yosys, OpenROAD, and Magic, OpenLane supports the entire flow, including synthesis, floor planning, placement, routing, and sign-off checks. It is widely used for learning and implementing VLSI design concepts, especially in academic and research environments.
---
### 1.4 Simplified RTL to GDSII Flow in Openlane
---
![Image Description](Media/DAY%20-%201/2/Image3.png)
![Image Description](Media/DAY%20-%201/2/Image%20(4).png)
- RTL (Register Transfer Level) : This is the starting point where the design is described in an HDL (e.g., Verilog or VHDL).It defines the logic and behavior of the circuit at a high level.
  
- Synthesis:
Translates the RTL code into a gate-level netlist using standard cells from a given technology library.
The output is a representation of the circuit in terms of basic gates (AND, OR, etc.) that can be implemented on silicon.

- FP + PP (Floor Planning and Power Planning)
Floor Planning: Defines the placement of major blocks in the chip, such as macros and standard cell regions, to optimize area and performance.
Power Planning: Designs the power distribution network (PDN) to ensure all components receive adequate power and ground connections.

- Placement :
Standard cells are placed in specific locations on the die based on the floor plan.
Ensures proper placement for routing and performance optimization while minimizing delays.

- CTS (Clock Tree Synthesis) :
Builds the clock distribution network to ensure the clock signal reaches all parts of the chip with minimal skew (delay differences).
Critical for synchronizing operations across the chip.

- Routing :
Connects the placed cells and macros with wires to establish signal paths as defined in the netlist.
Focuses on minimizing wire length, avoiding congestion, and meeting timing constraints.

- Sign-Off :
Final verification stage before tape-out.
Includes checks for timing (STA), power, signal integrity, and physical design rules.
Ensures the design is ready for fabrication.

- PDK (Process Design Kit) :
A set of files provided by the foundry that contains technology-specific information (e.g., design rules, standard cells) required for implementing the design.

- GDSII :
The final output of the flow, representing the chip's physical layout in a format used for fabrication.
Contains all layers and features of the design for manufacturing.
---
### 1.5 Openlane directory structure
---
The following images display the various folders and their contents present in the OpenLane directory by default.   

![Image Description](Media/DAY%20-%201/3/Image%20(1).png)
![Image Description](Media/DAY%20-%201/3/Image%20(2).png)
---
---
### 1.6 Design Preparation step
---

The following images display the source files related to picorv32a    

![Image Description](Media/DAY%20-%201/4/Image%20(1).png)
![Image Description](Media/DAY%20-%201/4/Image%20(2).png)
![Image Description](Media/DAY%20-%201/4/Image%20(3).png)  

Follow the code in openlane directory to start openlane in interactive mode and begin design setup  

`docker`  

`./flow.tcl -interactive`  

`package require openlane 0.9`  

`prep -design picorv32a`  

![Image Description](Media/DAY%20-%201/4/Image4.png)  

Next the synthesis is performed by the following code,  
`run_synthesis`      

![Image Description](Media/DAY%20-%201/4/Image%20(5).png)    
![Image Description](Media/DAY%20-%201/4/Image%20(6).png)  
The Flipflop ratio is determined as follows,    

Flipflop ratio = (number of flipflop) / (total number of flipflop)  

Flipflop ratio = 1613/14876 = **0.10843**  or  **10.843%**    

![Image Description](Media/DAY%20-%201/4/Image(7).png)    
---
---
## Day - 2
---
### 2.1 Chip Floor planning considerations
---  
**Floorplanning** in VLSI design refers to the process of arranging the functional blocks or modules on a chip. The goal is to optimize the chip’s layout by minimizing area, power consumption, and wirelength while ensuring good performance and manufacturability.      

![Image Description](Media/Day%20-%202/Image%20(1).png)        

Utilization Factor (or Utilization) is a metric that measures the efficiency of the chip area usage. It is calculated as the ratio of the area occupied by the functional modules (core area) to the total available chip area.     

![Image Description](Media/Day%20-%202/Image%20(2).png)          

A custom IP in VLSI design refers to a user-defined intellectual property (IP) block that performs a specific function tailored to the designer's needs. It allows for the reuse of pre-designed, verified logic in multiple projects    

![Image Description](Media/Day%20-%202/Image%20(3).png)            

Decoupling capacitors are used to filter out noise and smooth voltage fluctuations in power supply lines, ensuring stable operation of integrated circuits (ICs). They provide a local charge reservoir to supply current during transient events, reducing power supply ripple.   

![Image Description](Media/Day%20-%202/Image%20(4).png)    

Noise margin refers to the ability of a digital circuit to tolerate noise without causing errors in logic levels. It is the difference between the actual voltage level of a signal and the minimum voltage required for a correct logic interpretation.    

![Image Description](Media/Day%20-%202/Image%20(5).png)    
![Image Description](Media/Day%20-%202/Image%20(6).png)    

Power planning in VLSI design is the process of managing and optimizing the power consumption of a chip during the design phase. It involves strategies such as power gating, clock gating, and the use of low-power cells to reduce dynamic and static power dissipation. Effective power planning ensures that the chip meets performance requirements while minimizing energy usage and heat generation.   

![Image Description](Media/Day%20-%202/Image%20(7).png)      

Pin placement in VLSI design refers to the process of positioning the input/output (I/O) pins of a chip on its physical layout. Proper pin placement is crucial for minimizing routing congestion, optimizing signal integrity, and reducing the overall area of the chip. It also ensures efficient communication with external devices while meeting electrical and design constraints.    

---
### 2.2 Floor step planning in Openlane
---
The next step after synthesis is floor plannning which can be done with the following code  

`run_floorplan`    

The following image displays the observations made after this command,    


![Image Description](Media/Day%20-%202/Image%20(8).png)      

The following image shows the newly created files after running floor planning,    

![Image Description](Media/Day%20-%202/Image%20(10).png)        

The following image shows details of DIE Area,    
 
![Image Description](Media/Day%20-%202/Image%20(9).png)        

The following line of code opens it in MAGIC,      

`magic -T /home/vsduser//Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &`    


![Image Description](Media/Day%20-%202/Image%20(11).png)    
---
### 2.3 Library binding and Placement
---





















































