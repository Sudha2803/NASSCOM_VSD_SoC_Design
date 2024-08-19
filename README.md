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

## **DAY 3 - Design Library Cell using MAGIC layout and ngspice characterization**

### LAB
Floorplan variables like *core utilization* and *IO Mode* can be altered even during the flow, so modifications to the design are relatively easy. 

For this analysis, the design of an inverter cell is used, and spice analysis is performed on it. In order to do this, the Github respository containing the design and specification files is cloned by the command :

```
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
The inverter design file from the cloned repository is then opened using the **MAGIC** tool, as shown

![Screenshot 2024-08-18 122355](https://github.com/user-attachments/assets/4feb815b-092d-463e-ad84-103ca069cf92)

The upper half of the shown diagram is the PMOS, with the nwell surrounding it, and the lower half is the NMOS. This can be verified from the *tckon* window as well, by running the following commands :
```
Hover the mouse over the component in question
Click 's' to select the component
Open the tckon window and type 'what'
```
![Screenshot 2024-08-18 061912](https://github.com/user-attachments/assets/d46f33de-2bc1-4f18-a491-778b7b0e467b)

Now in the *tckon* window, the following commands are used to exctract the spice file for this design
```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```
Now in the library, the spice file has been generated in the same folder as shown in the highlighted lines.

![Screensho![Screenshot 2024-08-18 123409](https://github.com/user-attachments/assets/45b2b846-5d89-4a26-9bed-9025cd79bd51)
t 2024-08-18 131005](https://github.com/user-attachments/assets/9237ca4f-13ca-41e7-bcd6-274ed8085b84)

Now, opening the spice file from this library :

![Screenshot 2024-08-18 123409](https://github.com/user-attachments/assets/a3647b21-fb6c-4b77-92ec-601ccad1fbd8)

The following changes are made to the file, and the file is saved.

![Screenshot 2024-08-18 133350](https://github.com/user-attachments/assets/0274005b-2651-40c0-9831-4da7a02c4fc6)

**Navigation commands in Vim**
* Normal mode - ESC
* Insert mode - i/a/o
* Search mode - /text_to_be_searched
* Save and exit mode - :wq!

To simulate the file in ngspice, the following command is used
```
ngspice sky130_inv.spice
```

![Screenshot 2024-08-18 133727](https://github.com/user-attachments/assets/8573b3f0-32ff-47b6-baf5-ddbdf2e392e9)

The results of this simulation are plotted using the command
```
plot y vs time a
```

![Screenshot 2024-08-18 133938](https://github.com/user-attachments/assets/fed0e50a-81d8-4653-ae82-7270a290b8b0)

The inverter characteristics such as *rise time*, *fall time*, *transition time* can be calculated from the graph
* Rise time - The time taken for the signal to transition from 20% of Vdd to 80% of Vdd (0 -> 1)
* Fall time - The time taken for the signal to transition from 80% of Vdd to 20% of Vdd (1 -> 0)
* Transition time - The time difference between input signal attaining 50% of Vdd and output signal attaining 50% of Vdd

For Vdd of 3.3V as chosen, the 20% and 80% values are calculated and respective readings are taken

![Screenshot 2024-08-19 004732](https://github.com/user-attachments/assets/607c5239-841d-492f-a3f6-83d12ea6b04d)
![Screenshot 2024-08-19 004857](https://github.com/user-attachments/assets/147aead5-84e8-4ebc-8dc2-93b069ba4919)

![Screenshot 2024-08-19 004605](https://github.com/user-attachments/assets/0da754fe-ce1c-4b45-918f-a9d7641b2f15)

Calculating from the observed values, 
* **Rise time** - 0.0634ns
* **Fall time** - 0.0452ns
* **Transition time** - 0.068ns

In the next set of lab analysis, a set of drc test files are to be imported and unzipped, using the commands
```
sudo wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
sudo tar xfz drc_tests.tgz
```

![Screenshot 2024-08-19 014738](https://github.com/user-attachments/assets/9de5fe89-20a7-473b-b087-2d38b17e0e96)

To examine these files using the MAGIC tool
```
command magic -d XR
```
Now open the *met3.mag* file in MAGIC.

![Screenshot 2024-08-19 015130](https://github.com/user-attachments/assets/cf1191ac-ae44-4324-94ed-05af5b3bed9e)

In order to see the various DRC errors, select a component and type 'why' in the *tckon* console

Open the *poly.mag* file. 
![Screenshot 2024-08-19 021946](https://github.com/user-attachments/assets/687f9867-6f13-4736-96ee-97097b17a9d7)

An attempt to fix one of the errors, poly.9 error, first the participating layers are ascertained as shown. The error lies in the spacing between the two poly layers (*npolyres* and *poly*), which hasn't been defined correctly as an error.
The spacing is measured to be 0.2 micrometres.

![Screenshot 2024-08-19 021916](https://github.com/user-attachments/assets/9cebbe03-ba0e-4511-bdfd-fec555af213d)

To rectify this, open the *sky130A.tech* file, which is located in the *drc_tests* directory. Make the modifications as shown below and save the file.

![image](https://github.com/user-attachments/assets/1ff3c177-0d19-49d9-b5cc-0b88540384c7)

Reload the *sky130A.tech* file and rerun the DRC check by the commands
```
tech load sky130A.tech
drc check
```

![Screenshot 2024-08-18 152005](https://github.com/user-attachments/assets/fb2622a1-bf85-41f5-af5d-86c9568c96b5)

Now the poly.9 shows an error upon DRC check.
In this next analysis to show a DRC error as a geometric construct. Open *nwell.mag* and execute the following commands in the *tckon* console.

```
cif ostyle drc
cif see dnwell_shrink
```
* nwell.mag
![Screenshot 2024-08-18 162417](https://github.com/user-attachments/assets/8cb9a2c2-be07-4468-af99-03555ad353fe)

With respect to nwell.6, the error in the poly layer acting as a boundary, and specifically, the dimensions of this boundary violating the tech specifications. 

![Screenshot 2024-08-18 154244](https://github.com/user-attachments/assets/612f06ae-dc1a-401f-802f-c662ec33d9fb)

Once again open *sky130A.tech*, search for 'drc', and make the following changes

![Screenshot 2024-08-18 162155](https://github.com/user-attachments/assets/aabbcbca-1eab-4563-b8be-4b77ae168f84)

After altering the tech file, once again open the *tckon* console and enter the following commands to see the results
```
drc check
drc style drc(full)
drc check
```

## **DAY 4 - Pre-layout Analysis and importance of good Clock Tree**

### LAB

In order to perform PnR (Placement and Routing), all the information present in a layout file is not necessary. A *.lef* file is extracted from the inverter layout file. This is in order to make it into a standard cell that can be implemented in other designs as well.

Some primary requirements for PnR are :
1. The I/O ports must be present at the junctions where the vertical and horizontal tracks intersect.
2. The standard cell's width should be an odd multiple of the track's horizontal pitch.
3. The standard cell's height must be an odd multiple of the track's vertical pitch.

**Steps to convert grid info to track info**
Open the custom inverter layout, from the *vsdstdcelldesign* library in MAGIC using the command
```
magic -T sky130A.tech sky130_inv.mag &
```
![image](https://github.com/user-attachments/assets/b36708d1-2a9f-4210-8565-f29363a28b18)

In the *tckon* console, type the command 
```
grid 0.46um 0.34um 0.23um 0.17um
```
![image](https://github.com/user-attachments/assets/b3057a36-9e4b-497d-8fed-4f21908acedc)

From expanding this layout view, it is seen that the li1 layer is completely on the grid layer. The inout and output ports lie on the intersections of vertical and horizontal tracks. Moreover, the width of the standard cell is seen to be 3 grid/track boxes. Thus all the requisite conditions are satisfied. 

**Converting MAGIC layout to a standard cell LEF**
First port definitions are created by selecting the port, going to file -> text, and entering details.

![image](https://github.com/user-attachments/assets/5a38e27a-ef98-4885-964c-e732d0fbf6a8)

Next the port class and port use attributes are set, in order to define the purpose of the port, using the commands 
```
# confirm the port
what
# define the port class and use
port class input
port use signal
```
This is repeated for all the ports in the layout. Then the *.mag* file is saved using the command in *tckon* :
```
save sky130_vsdinv.mag
```
![image](https://github.com/user-attachments/assets/916480a1-e14d-4fdd-a810-d9262d006f2a)

Next, the saved *.mag* file is opened using the command in the terminal :
```
magic -T sky130A.tech sky130_vsdinv.mag &
```
In the *tckon* console, enter the following command to extract the lef file
```
lef write
```
Now the extracted lef file is visible in the same directory
![image](https://github.com/user-attachments/assets/7f9b474b-1135-4268-9fc1-c723168c3417)

![image](https://github.com/user-attachments/assets/7c503fa3-243d-4406-a606-a6942e24a9e5)

Now this lef file is copied into the source directory of picorv32a using the command
```
cp sky130_vsdinv.lef /home/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
```
From this stage onwards, the standard cell for the inverter is to be included in the picorv32a design. Once again, the first stage is **synthesis**, similar to the execution of Day 1. In order for synthesis to run successfully, the standard cell library files are required. These are copied using the command :
```
cp sky130_fd_sc_hd__* /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
```
![image](https://github.com/user-attachments/assets/1d9c07d0-28cc-44ef-8cd7-6e0f3da3f182)

In addition, the *config.tcl* file needs to be modified as shown in the figure below.

![image](https://github.com/user-attachments/assets/5121e4bd-c961-4ab8-a732-dbcb0f29d5ee)

Now once again the docker is invoked and the same steps for synthesis are repeated 
```
./flow.tcl
package require openlane 0.9
prep -design picorv32a -tag -overwrite
run_synthesis
```
![image](https://github.com/user-attachments/assets/e86b3048-673a-4738-a9c2-080717790745)

After synthesis has been run, add the following commands to incorporate the additional lef file into the flow
```
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]  
add_lefs -src $lefs
```
Once this is done, the OpenLANE flow is continued by running floorplan stage using the command 
```
init_floorplan
place_io
tap_decap_io
```

From the results of the synthesis, it is observed that there are 1554 instances of the user defined standard cell **sky130_vsdinv**. 
Moreover, the chip area is 147712.91

![image](https://github.com/user-attachments/assets/c40e5e3d-1737-4b4a-9d98-c35c743f27d6)

It is also observed that the worst slack is -23.89 and the total negative slack is -711.59. This can be combatted by running timing-driven synthesis.
In the *README.md* file, a parameter called **SYNTH)_STRATEGY** is defined, whose value specifies the optimization of the syntehsis step

* 0/1 - Delay driven
* 2/3 - Area driven

The variable is checked with the command 
```
echo $::env(SYNTH STRATEGY)
```
In this case te synthesis is already delay drive, so no changes need to be made. 
Next step is placement, using the command
```
run_placement
```
![image](https://github.com/user-attachments/assets/8bcd7853-11d1-4dbe-b61f-31f401f515f8)
![Screenshot 2024-08-20 012632](https://github.com/user-attachments/assets/06d3fafb-1c7f-4607-b691-bc4ecdc071ae)

Now placement has been succesfully run.
Moreover, it is seen that the total negative slack is 0, which indicates no **slack violation**

Now to view the user defined cell inside the design, once again the MAGIC tool is utilized. Enter the following commands 
```
cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-08_06-08/results/placement
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![Screenshot 2024-08-20 021758](https://github.com/user-attachments/assets/b31edd2d-edc9-4f8c-89dc-b8b05f6044a8)

![image](https://github.com/user-attachments/assets/f6a64d92-a812-4dce-a8c3-05fa40720ccd)

As shown, the user defined standard cell is present within the picorv32a design. 

**Static Timing Analysis**
Post floorplanning and placement, Static Timing Analysis is conducted to examine the proper fucntionality of the cell without any timing violations.

An STA config file is created in the directory 
```
/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane
```

![image](https://github.com/user-attachments/assets/ae1c32f6-ff0a-4e03-8e25-b3e2e5dfb428)
![Screenshot 2024-08-20 032549](https://github.com/user-attachments/assets/725f1972-09fa-4b59-ade1-e8ac741b2401)

An sdc file containing all the specifications is created in the **src** directory of picorv32a

![image](https://github.com/user-attachments/assets/33a2bda6-9027-41a3-838e-a482d7eb9e8f)

Now running STA analysis in openlane by the command
```
sta pre_sta.conf
```
![image](https://github.com/user-attachments/assets/9509a26a-12e2-4c22-8aeb-5ac69d4c5c8a)


The timing details can also be viewed as a result of this operation. Thus the timing requirements are met as shown in the figure.

![image](https://github.com/user-attachments/assets/af2b62da-c46a-4c7f-8689-dd09a7d349cd)

Next, the clock tree synthesis and routing is carried out. 

**Steps to carry out CTS using TritonCTS**
TritonCTS is the EDA tool that carries out clock tree synthesis. Various CTS variables are present in the *README.md* file in configuration directory

To rewrite the verilog file post synthesis, the command
```
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-08_06-08/results/synthesis/picorv32a.synthesis.v
```
Now, returning to the OpenLANE docker, after having rewritten the verilog file, floorplan is run using the commands
```
init_floorplan
place_io
tap_decap_or
```

![Screenshot 2024-08-20 093821](https://github.com/user-attachments/assets/328d5575-321b-4504-92ff-5c27ad4ee283)

Next run placement using the command

```
run_placement
```
![image](https://github.com/user-attachments/assets/d395516c-89ba-4214-96c9-5cc13ee1a5e1)

```
run_cts
```

![image](https://github.com/user-attachments/assets/fe48f074-9b10-4734-8e17-9d355cb3eccf)

In this stage, the clock buffers get added, which changes the netlist. After this step, a new *.cts* file has been created to the synthesis directory, containing details of the previous netlist as well as clock buffers added in this stage.

**Timing Analysis using OpenSTA**
For timing analysis, first the database containing ass lef and def files is created by the following commands.
```
read_lef /openLANE_flow/designs/picorv32a/runs/17-08_06-08/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/17-08_06-08/results/cts/picorv32a.cts.def
```

![image](https://github.com/user-attachments/assets/eb5f0839-f2ba-41d3-835c-6cadd0bda15c)

![image](https://github.com/user-attachments/assets/319f4ae1-4e81-4a5a-9ca6-3af9ee6671b7)

```
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/26-07_10-33/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```

![image](https://github.com/user-attachments/assets/82c2069a-3c28-4f1a-b35c-7d57ad93eb9a)

![image](https://github.com/user-attachments/assets/0d4ef0a0-7ca2-487b-a172-ef9bf5a4f79c)
Here it is also seen that the slack in the hold time is satisfactorily met.

**Steps to execute OpenSTA with the right timing libraries and CTS assignment**

Exit from openroad. Check the buffers utilized in the CTS netlist using the command
```
echo $::env(CTS_CLK_BUFFER_LIST)
```
![image](https://github.com/user-attachments/assets/1117b7dd-6efe-4d23-9033-ba1b16b25581)

## **DAY 5 - Final steps for RTL2GDS using tritonRoute and openSTA**

**Building a power distribution network for the design**

After the CTS has been generated, a power distribution netowkr (PDN) is created (in OpenLANE, exit openroad) for the design before routing is begun. Using the following command
```
gen_pdn
```
![image](https://github.com/user-attachments/assets/71ca1bba-f9db-49c2-9905-dd374d2db526)

Once the PDN has been successfully generated, the design can now be routed.The grid and straps for power and ground have been created.  

![image](https://github.com/user-attachments/assets/479c88a5-93e6-48b5-8948-0a9fe3d15696)

The details pertaining to routing variables can be found in the *README.md* in the configuration directory. Run the following command for routing
```
run_routing
```

![image](https://github.com/user-attachments/assets/ec97b4bd-0847-4872-8768-2f26896b21e1)
The design has been routed successfully. To view the final results, which include the *parasitic extraction file* *.spef*, go to the following directory
```
/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/20-08_05-43/routing
```

![image](https://github.com/user-attachments/assets/77d9d00a-d793-4c89-9897-64b58ca66b55)

To view the final layout, use the command 
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def
```
![image](https://github.com/user-attachments/assets/499d28b2-a518-4a8d-a9d2-d307607c5a4c)

This design successfuly incorporated the user defined inverter standard cell. A zoomed in image of the design with the standard inverter cell highlighted is shown below.

![image](https://github.com/user-attachments/assets/743d5113-1c95-41bd-8530-a82a2dc96a40)

## REFERENCES
* [VSD Standard Cell Design](https://github.com/nickson-jose/vsdstdcelldesign.git)
* [Google Skywater PDK](https://skywater-pdk.readthedocs.io/en/main/index.html)

## Acknowledgements
* **Kunal Ghosh**, Co founder (VSD Corp. Pvt. Ltd)
* **Nickson P Jose**, Teaching Assistant (VSD Corp. Pvt. Ltd.)
