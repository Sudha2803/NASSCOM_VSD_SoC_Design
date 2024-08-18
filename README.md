# NASSCOM_VSD_SoC_Design
## **DAY 1 - Inception of Open-source EDA, OpenLANE, and Sky130 PDK**
#### Introduction to RTL to GDSII flow

The RTL to GDSII flow is a critical process in IC design. It transforms a high-level behavioral description (RTL) into a detailed physical layout (GDSII) ready for manufacturing. Key steps include synthesizing the RTL into a gate-level netlist, followed by floorplanning, placement, and routing. Rigorous verification processes, such as static timing analysis and design rule checking, ensure the chip meets performance, power, and reliability requirements. Iterative design refinement is essential to optimize the layout before generating the final GDSII file.

A diagrammatic view of the RTL to GDSII process is shown below

![image](https://github.com/user-attachments/assets/306080ab-211d-4e16-adcc-cbf82fcb10ae)

### Introduction to OpenLANE
OpenLane is an open-source digital ASIC flow that automates the design process from RTL to GDSII. It provides a comprehensive toolchain for synthesis, place and route, and physical verification. Openlane is built around Skywater 130nm process node and is capable of performing full ASIC implementation steps.

The figure below shows the flow of operations in OpenLANE

![image](https://github.com/user-attachments/assets/4ddc7679-c1ff-41c1-97c1-d86467cce7a6)

# ASIC Design Flow 
In the process of designing an ASIC _(Application Specific Integrated Circuit)_ ,the various broad stages involved are Design input, Synthesis, Physical Design, Verification. Post verification, the output is the final layout data in the GDSII format, which is ready for fabrication

**Design Input:**
* design/src/*.v: This folder contains the Verilog RTL code describing the circuit's functionality.
* SW PDK: The Skywater 130nm process design kit provides technology-specific information for layout and verification. 

**Synthesis:**
* RTL Synthesis (Yosys + abc) : Converts Verilog RTL into a gate-level netlist.
* Synth Exploration : Explores different synthesis strategies for optimization.
* STA (OpenSTA) : Performs static timing analysis to estimate circuit performance.
  
**Physical Design:**
* Floorplanning : Arranges major blocks within the chip's die area.
* Placement : Positions individual cells within the floorplan.
* CTS : Generates a clock distribution network to ensure consistent clock arrival.
* Optimization : Refines placement and routing for better performance and power.
* Global Routing : Determines the overall path for wires between blocks.
* Detailed Routing : Assigns specific metal layers for wire connections.

**Verification:**
* LEC (Yosys) : Checks layout equivalence with the netlist.
* STA (OpenSTA) : Verifies timing after placement and routing.
* DFT (Fault) : Inserts test structures for manufacturing testing.
* Physical Verification (magic & netgen) : Ensures layout adheres to design rules and is electrically correct.


### LAB 
The OpenLANE docker is inititated using the following commands as shown. 

![Screenshot 2024-08-15 233213](https://github.com/user-attachments/assets/9a332bc9-d3b0-45c6-9b00-2bb8d57e6ce2)

The design in use, picorv32a, is made ready for synthesis by the command
```
prep -design picorv32a
```
in order to merge the lef file
After the design is prepared, we run the synthesis as shown in the docker. 
```
run_synthesis
```

![Screenshot 2024-08-16 052134](https://github.com/user-attachments/assets/f69092c3-71ef-41bb-8b19-5f9401d9d865)

Synthesis was successful, and this creates a new run, with the specified date and time, within the **runs** folder of picorv32a.
The chip area for this design is 147712.92

![chip area](https://github.com/user-attachments/assets/8612611e-eb07-4039-a359-4178d535677f)

The **flop ratio** can be defined as the number of flip flips divided by the total number of cells. To calculate this, 
```
Number of flipflops = 1613
Number of cells = 14876
=> Flop ratio = 0.1084
```

![flop r![Screenshot 2024-08-16 050800](https://github.com/user-attachments/assets/ef30e456-2ee2-4e98-a963-6c20fb0cb217)
atio](https://github.com/user-attachments/assets/a7e6ca7d-c5c9-494b-a6b4-3b5789780ddc)

The results of running this synthesis are stored in a separate folder in the runs directory as shown

![Screenshot 2024-08-16 050800](https://github.com/user-attachments/assets/56e91de6-3f5d-46a0-81a1-a0e4e0a9daaa)

## **DAY 2 - Good Floorplan vs. Bad Floorplan & Introduction to Library Cells**

### LAB
The different variables of the design flow can be ascertained from the *README.md* file present in OpenLANE configuration. The path is specified below.
```
/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/configuration
```
![image](https://github.com/user-attachments/assets/5c0adcf9-d72a-4753-ab58-ded2ca9283b9)

![Screenshot 2024-08-16 231102](https://github.com/user-attachments/assets/6cb79bea-0df2-4aeb-b3c8-a3cb6f6a9e16)

With respect to floorplanning, there are definitions for standard parameters like *core utilization* and *aspect ratio*, as well as the types of switches involved in this phase.

![Screenshot 2024-08-16 231702](https://github.com/user-attachments/assets/c946d5a9-bb32-4635-b6ad-297cfe3c9ff9)

The *floorplan.tcl* file, also present in the configuration library, contains the default values for all these parameters, which define the standard specifications which will get executed.
![Screenshot 2024-08-16 172834](https://github.com/user-attachments/assets/02aa8df0-1d26-43a5-8087-ad076a07219a)

For the picorv32a design, design specific variable values and floorplan settings are present in the *config.tcl* and *sky130A_sky130_fd_sc_hd_config.tcl* files. The path for these files is :
```
~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a
```
* Config.tcl
![Screenshot 2024-08-16 232635](https://github.com/user-attachments/assets/022675cd-bb4b-42e1-ac9e-c4e49c09826c)

* sky130A_sky130_fd_sc_hd_config.tcl
![image](https://github.com/user-attachments/assets/6020a3e2-f334-4058-abca-7520901a00c9)

To run the floorplan, once again the OpenLANE docker is used, and the command is run :
```
run_floorplan
```
![Screenshot 2024-08-17 002128](https://github.com/user-attachments/assets/e041748e-238c-47a4-a863-f58cc91a043b)

The floorplan has been successfully run. The results for the floorplan stage can be observed in the *picorv32a.floorplan.def* file as shown.
![Screenshot 2024-08-16 165652](https://github.com/user-attachments/assets/b5114715-54f0-41b8-8150-1decc47e86cf)


While configuring the designs, the priority order of the files in terms of value specifications is :
1. *sky130A_sky130_fd_sc_hd_config.tcl*
2.  _config.tcl_
3. *floorplan.tcl*

The **MAGIC tool** is an open source layout editor, which is commonly used in VLSI design. It provides a graphical user interface for editing IC layouts, and allowing designers to minutely inspect floorplan, placement and routing of their designs. It is easily compatible with other tools needed for the ASIC design flow, and as such, is widely used. 

The command to open the MAGIC tool is :
```
magic -T home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
![Screenshot 2024-08-17 123219](https://github.com/user-attachments/assets/6d78617d-39c7-4cdd-840c-3e3ed6cbc073)

More details regarding the floorplan cells can be found by typing appropriate commands into the *tckon* window.

![Screenshot 2024-08-17 123249](https://github.com/user-attachments/assets/add38225-c9c9-4cba-b58c-d9b9a9a452c6)

Placement takes place in two stages, **global** and **detailed**. Global placement is more coarse, and no actual legalization of cell locations happen. 
The placement process is carried out by returning to the docker in OpenLANE, and running the command
```
run_placement
```
After successfuly running placement, the results are available in the *picorv32a.placement.def* file.
This can once again be viewed using the MAGIC tool as used before, by running the command :
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
```
The result of the placement step is shown below.
![Screenshot 2024-08-18 061812](https://github.com/user-attachments/assets/c7baa116-a603-46c8-be27-5fac61d9339c)
Again for this, details regarding the specific cells and interconnections can be accessed using commands typed into the *tckon* window.

![Screenshot 2024-08-18 061912](https://github.com/user-attachments/assets/1aca1258-3aff-4a6f-8817-72476b5ad867)
