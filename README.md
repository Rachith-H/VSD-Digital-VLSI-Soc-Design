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
### 2.2 Floor planning in Openlane
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
---
### 2.3 Library binding and Placement
---  

The binding process in physical design, where the logical netlist is mapped to physical cells from the standard cell library.This step is crucial before placement and routing in the backend flow.  

![Image Description](Media/Day%20-%202/Image%20(12).png)   

Placement is the process of determining the optimal physical locations of standard cells on the chip while minimizing timing delays, power consumption, and area. It ensures that cells are placed without overlap and maintains logical connectivity from the netlist. Placement serves as the foundation for the routing phase in the physical design flow.  

![Image Description](Media/Day%20-%202/Image%20(13).png)  
![Image Description](Media/Day%20-%202/Image%20(14).png)   

The following image illustrates the VLSI physical design flow, covering logic synthesis, floorplanning, placement, and routing. It highlights how a netlist is transformed into a physical layout, optimizing timing, power, and noise. Key stages include organizing blocks (floorplanning), placing cells (placement),clock tree synthesis(cts) and connecting them (routing).  
![Image Description](Media/Day%20-%202/Image%20(15).png)        

---
### 2.4 Placement in Openlane  
---  

Execute the following code to perform placement procedure in openlane,
`run_placement`      

![Image Description](Media/Day%20-%202/Image(17).png)     

Execute the following code to view it in magic,  
`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &`    

![Image Description](Media/Day%20-%202/Image(16).png)     

---
### 2.5 Cell design and Timimg parameters  
---  
The cell design flow begins with defining the cell's functionality, size, and performance specifications. Next, the circuit design is created at the transistor level, followed by generating the physical layout while optimizing for area and ensuring DRC compliance. Parasitic extraction is performed to model resistance and capacitance accurately, and SPICE simulations verify the cell's performance under varying process, voltage, and temperature (PVT) conditions. This ensures the cell meets all timing, power, and area requirements before inclusion in the standard cell library.  

![Image Description](Media/Day%20-%202/Image%20(18).png)   
![Image Description](Media/Day%20-%202/Image%20(19).png)       

Timing characterization involves determining the timing behavior of a standard cell, which includes analyzing how quickly signals propagate through the cell and how it responds to input changes. Key parameters include setup time, hold time, propagation delay, and transition time. These values are extracted from simulations under different voltage, temperature, and process conditions to ensure the cell meets performance targets.  
![Image Description](Media/Day%20-%202/Image%20(21).png)     
![Image Description](Media/Day%20-%202/Image%20(22).png)       

---
## Day - 3 
---
### 3.1 CMOS inverter in SPICE
---  
A SPICE deck is a text file that describes a CMOS circuit for simulation. It includes details like circuit components, their connections, and simulation commands. The file defines transistors, power supply, inputs, and outputs, along with instructions for analyzing the circuit's behavior. It helps in testing how the circuit responds to different conditions, such as voltage changes, timing delays, and power consumption, before actual fabrication.    

![Image Description](Media/Day%20-%203/Photo%20(2).png)        

![Image Description](Media/Day%20-%203/Photo%20(3).png)        

The 16-mask CMOS process is a standard fabrication method that uses 16 photolithography masks to define different layers and structures in an integrated circuit. It includes steps like well formation (n-well and p-well for transistors), field oxidation (for isolation), gate oxide and polysilicon formation (for transistor gates), source/drain doping (for transistor operation), contact and metal layer formation (for interconnections), and passivation (for protection). This process ensures precise transistor fabrication, isolation, and connectivity, making it suitable for high-performance CMOS circuits.  

![Image Description](Media/Day%20-%203/Photo%20(4).png)    
![Image Description](Media/Day%20-%203/Photo%20(5).png)        



---
### 3.2 CMOS Inverter using magic layout and ngspice   
---  
The required files of cmos inverter can be found in the github repository cloned with the following command,  
`git clone https://github.com/nickson-jose/vsdstdcelldesign.git`  
The contents of this are as follows,    

![Image Description](Media/Day%20-%203/Image%20(1).png)        

The sky130A.tech file is copied to this new folder with the following command,      
`cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign`    

The layout of the inverter is opened in magic with the following command,  
`magic -T sky130A.tech sky130_inv.mag &`    

![Image Description](Media/Day%20-%203/Image%20(2).png)    

The nmos and pmos are identified with `what` command in tkcon window,    

![Image Description](Media/Day%20-%203/Image%20(3).png)
![Image Description](Media/Day%20-%203/Image%20(4).png)


This layout is extracted as a spice file with the following command in tkcon window,  
```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```

![Image Description](Media/Day%20-%203/Image%20(5).png)

The extracted spice file is as follows,    

![Image Description](Media/Day%20-%203/Image%20(6).png)  

The following changes are made in this spice file, in order the execute and observe the waveforms using ngspice,    

![Image Description](Media/Day%20-%203/Image%20(7).png)

The ngspice plots are obtained using th efollowing commands,  
`ngspice sky130_inv.spice`  
`plot y vs time a`  
  
![Image Description](Media/Day%20-%203/Image%20(8).png)     

The following image shows the ngspice waveform for the cmos inverter by which various parameters like rise time and fall time can be determined,  

![Image Description](Media/Day%20-%203/Image%20(9).png)
![Image Description](Media/Day%20-%203/Image%20(10).png)

To perform DRC for the layout, the required test files can be downloaded with the following command,  
```
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
tar xfz drc_tests.tgz
cd drc_tests
ls -ltr
```

![Image Description](Media/Day%20-%203/Image%20(11).png)   

The following image shows the .magicrc file now available in drc_tests folder, it can be viewed withe following command,  
`vi .magicrc`    

![Image Description](Media/Day%20-%203/Image%20(12).png)    

Now, open Magic tool using the command `magic -d XR &`  
and open the metal3 file,        
The pheriphery rule of metal 3 are available at  
`https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#m3`

![Image Description](Media/Day%20-%203/Image%20(13).png)        

Design rule check can be performed with the following commands in tkcon window,    
`drc why` , for checking the violations  
`load poly ` , to see the errors    

![Image Description](Media/Day%20-%203/Image%20(14).png)          

After the necessary changes the final layout with no drc errors is as shown below,    

![Image Description](Media/Day%20-%203/Image%20(15).png)   
![Image Description](Media/Day%20-%203/Image%20(16).png)     
---
## Day - 4
---  
### 4.1
---
The tracks info can be checked with `less tracks.info` command,  

![Image Description](Media/Day%20-%204/Image%20(1).png)   

Now open the inverter in Magic tool, and set up the grid with the following command in tkcon window,  
`help grid`  

`grid 0.46um 0.34um 0.23um 0.17um`  

![Image Description](Media/Day%20-%204/Image%20(2).png)     

Save this as a new lef file with the `save` command,  

![Image Description](Media/Day%20-%204/Image%20(3).png)     

Now open this file and enter `lef write` in tkcon window,    

![Image Description](Media/Day%20-%204/Image%20(4).png)       

The lef file is as shown below,    

![Image Description](Media/Day%20-%204/Image%20(5).png)     

Now copy the lef file and library file to 'src' folder under 'picorv32a'  
The image below shows the pasted files in the required location,    

![Image Description](Media/Day%20-%204/Image%20(6).png)     
![Image Description](Media/Day%20-%204/Image%20(7).png)      

Now modify the existing config.tcl file as shown below,  
```
 set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
 set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
 set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
 set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
 set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```
![Image Description](Media/Day%20-%204/Image%20(8).png)      

After this, run begin the openlane flow from design preparation step, followed by synthesis, floorplan and placement,
The following images shows the openlane flow,   

![Image Description](Media/Day%20-%204/Image%20(9).png)      
![Image Description](Media/Day%20-%204/Image%20(10).png)      
![Image Description](Media/Day%20-%204/Image%20(11).png)    

Further load the generated file in magic tools with th efollowing command,  
`cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/02-01_11-58/results/placement/`  

`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &`    

![Image Description](Media/Day%20-%204/Image%20(13).png)    
![Image Description](Media/Day%20-%204/Image%20(14).png)      
---  
## Day - 5
---
### Routing 
---
Maze routing is a pathfinding algorithm used in VLSI physical design to find a viable route between circuit components while avoiding obstacles. Lee’s algorithm, a popular maze routing method, is a breadth-first search (BFS)-based approach that ensures an optimal path if one exists. It works by expanding a wavefront from the source node, marking grid points with increasing step counts until it reaches the destination, then backtraces the shortest path. While effective, Lee’s algorithm is computationally expensive and can be slow for large grids, making it suitable mainly for small-scale routing problems.

![Image Description](Media/Day%20-%205/Image%20(1).png)    

TritonRoute is an open-source detailed router used in VLSI physical design as part of OpenROAD. It performs detailed routing, meaning it finalizes the wire connections at a granular level while adhering to design rules (DRC) and manufacturing constraints. TritonRoute is based on a grid-based routing approach and uses integer linear programming (ILP) and rip-up and reroute techniques to resolve design rule violations. It is widely used in academic and industry research for physical design automation (PDA) and is optimized for modern process nodes.    

![Image Description](Media/Day%20-%205/Image%20(2).png)      
![Image Description](Media/Day%20-%205/Image%20(3).png)    
---
---
## Acknowledgements  
- [Kunal P Ghosh](https://github.com/kunalg123) : Director and co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd.
- [Nickson Jose](https://github.com/nickson-jose) : Technical Lead @HCLTech  , Ex-Intel
- [R. Timothy Edwards](https://github.com/RTimothyEdwards) : Senior Vice President of Analog and Design at efabless corporation
---











































































