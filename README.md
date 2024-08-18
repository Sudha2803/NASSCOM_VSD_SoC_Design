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
