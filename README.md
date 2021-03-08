# About
This repository  gives you a overview on the usage of the open source EDA tools in the implementation of SoC Physical Design flow for the PicoRV32 design. *[PicoRV32](https://github.com/cliffordwolf/)* is a example CPU design that implements RISC-V RV32IMC Instruction Set, suitable for use in FPGA designs and ASICs.

Open Source Synthesis Toolchain : qflow (opencircuitdesign.com)

Synthesis – yosys

Placement – graywolf

Routing – qrouter

Layout – magic

DRC – magic

STA – OpenTimer

Circuit Simulation – ngspice

For installation of open source EDA tools refer *[here](https://www.vlsisystemdesign.com/vsd-a-complete-guide-to-install-open-source-eda-tools/)*

# Getting started with open source tools

You may start the physical design flow by first ensuring all the tools are installed properly and are working correctly by invoking them. 


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/01_invoke_yosys.png)

![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/02_invoke_OpenTimer.PNG)


Below you see the path the sta tool is linked to.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/03_sta_path.png)


Now we use VSDFLOW, which is built using Open Source Hardware Tools that takes in the RTL and gives you GDSII  layout, performance and area metrics of your design.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/04_git_clone_vsdflow.png)


Change the directory to vsdflow and execute the spi_slave  testcase.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/05_spi_slave_design_details.png)
 

You will see that the qflow starts to implement all the stages of physical design and output files getting created.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/06_spi_slave_qflow.png)


Move to the outdir_spi_slave directory. You will see 16 files and folders created. 


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/07_files_and_folders_count.png)


Use Qflow to view the layout of the design.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/08_layout.png)


Use the tkcon window to know the area of the design by typing box. (You can further explore the layout and other commands)


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/09_tkcon_output_area.png)


# PnR flow using qflow


Now that all the tools are working fine, you can create your new project directory containing source, synthesis and layout directories in it. Copy the picorv32.v file from vsdflow directory to the now created source directory.  


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/10_make_directory.png)


Invoke the qflow. You can now see the qflow manager window with all the stages to convert RTL to GDSII. Start with the synthesis preparation stage by selecting correct technology, Verilog source file and entering the top level Verilog module name. Click on Set stop button to stop after every stage  in the flow and then click the run button to runthe stage.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/11_qflow_manager.png)


After successfully completing each stage you can get the log files of each stage by clicking on the Okay button. Below you can see the synthesis log file showing details of the design such as number of wires and cells. You can calculate the percentage of flipflop in the design. For this design, it is 12.22%  (number of flipflops/total number of cells, 1613/13197 = 0.1222).


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/12_synthesis_log.png)


You can press on the Settings button to input desired values for the different parameters. Below you can see parameters like Initial density (Utilization factor) and Aspect ratio that help you to have a specified shape (rectangle/square) of your chip, how much of it you would use initially to place your cells, you can have power stripes with specified width and pitch for power supply throughout the chip. Click on Arrange Pins button to specify the locations of the pins. Below is an example for creating a new group and specifying the pin locations on the chip edges.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/13_create_pin_group.png)

![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/14_pin_group_location.png)


Press the Run button and you will see the graywolf window showing the placement stage implementation.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/15_placement.png)



You can press the Edit Layout button in the Qflow manager window to view the layout or you can invoke qflow in your project directory (use qflow diplay picorv32 & command) to view the layout window as shown below. Select the whole chip by first moving the cursor to the bottom left and left mouse click and then moving to top right and right mouse click, then press Shift+i.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/16_select_chip.png)


Now you will see the selected chip are in the tkcon window. 


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/17_chip_area.png)


# Library cell Characterization using Magic Layout tool and ngspice


For SPICE simulations clone *[git link](https://github.com/kunalg123/ngspice_labs/)*. To see inv SPICE deck, go the ngspice_labs directory, open the inv.spice file to know the width of PMOS and NMOS transistors.  SPICE deck has list of circuit nodes and the elements between them, generates a series of nodal equations, and solves for the voltages,  statements identifying which mode(s) the circuit should solve for: DC, AC, or transient analysis.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/18_pmos_nmos_width.png)


The drain, gate, source and substrate are mentioned after M1 and M2. Then you have the width and length mentioned. You can calculate the Wp/Wn ratio, which is 0.5/0.375 = 1.3 here. Then the output load of 10fF is connected between the output and node 0 (vss/gnd). Vdd of 2.5v is connected between vdd and node 0. Vin of 2.5v is connected between in and node 0. The simulation is done Vin swing between 0 to 2.5 with steps of 0.05. All the technology parameters of nmos and pmos are taken from the mentioned model file.


Invoke ngspice to plot the CMOS VTC for the inv.spice file. 


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/19_invoke_ngspice.png)


Click on the intersection of blue line and CMOS VTC, you will observe the x0 and y0 values. 


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/20_plot_inv_vtc.png)


You can change the width of PMOS in the inv.spice and the vtc plot again.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/21_edit_inv.spice.png)


Observe that even with different switching threshold due to increase in the Wp/Wn ratio, the characteristic curve still remains in same shape which shows the robustness of the CMOS logic.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/22_edited_plot_cmos_vtc.png)


Open the inv_tran.spice file and edit PMOS width and observe the plot in ngspice.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/23_edit_inv_tran.spice.png)


At 50% of maximum voltage, click once on the rising output and once on the falling input. Take the difference of the x0 values of the two to obtain the rise delay. Here it is 1.09346ns - 1.01869ns = 74.7ps.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/24_edited_plot_inv_tran.spice.png)


Simulate the fn_prelayout.spice file and obtain x0 values for the fall and rise waveforms intersection with horizontal blue line at 1.25.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/25_invoke_ngspice_fn_prelayout.spice.png)


Calculate the difference between these x0 values to obtain the pulsewidth. Here it is 2.22807ns – 1.57895ns = 649.12ps.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/26_fn_prelayout_pulse_width.png)


# Layout and Extraction

Invoke the magic tool by mentioning the tech file. Source the draw_fn.tcl file which has already done layout example and observe the layout. This file has the information to create n-well area, p-diffusion area, polysilicon stripes etc.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/27_invoke_magic.png)

![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/28_layout_tkcon_window.jpg) 


You will observe 8 nsubstratecontact and 6 polysilicon stripes. Make proper connections using the metal layers in the layout window to obtain final layout.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/29_layout.png)


Now open the fn_postlayout.mag in magic.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/30_magic_fn_postlayout.png)


Observe the area of the layout in tkcon window.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/31_design_area.jpg)


Perform postlayout parasitic extraction to get the SPICE netlist. 


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/32_postlayout_spice_netlist.png) 


Modify the fn_postlayout.spice file with same stimulus that was used for prelayout simulation.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/33_edited_postlayout_spice_netlist.jpg) 


Simulate fn_postlayout.spice and observe the plot that has the changed pulse width. Here it is 2.2807ns – 1.59649ns = 684.21 ps. The waveform is still in the same shape which shows the functionality is correct, some shifts are due to addition of parasitics.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/34_fn_postlayout.spice.png)

![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/35_plot_fn_postlayout.jpg)


# Pre-Layout Timing Analysis 


Open the inv_tran.spice file and observe the input rise and fall slew values and the output load.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/36_input_rise_and_fall_slew.png)


Simulate inv_tran.spice in ngspice and observe the rise delay.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/37_rise_delay.png)


Modify the output load in inv_tran.spice.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/38_edit_output_load.png)


Observe the new rise delay by simulating inv_tran.spice in ngspice again. You can see that the rise delay increases with increase in the output load.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/39_plot_edited_rise_delay.png)


Open the osu018_stdcells.lib file and look for slew upper threshold for fall waveform.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/40_slew_upper_threshold_fall.png)


Observe the output rise threshold value.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/41_output_threshold_pct_rise.png)


Observe the variables mentioned in the look up table for delay calculations.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/42_lu_table_variables.png)


Observe the cell information mentioned and different drive strengths for the same type of cells.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/43_inv_cell.png)


Observe the delay templates in the look up tables that show the number of entries for each variable used in index_1 and index_2.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/44_invx1_delay_template.png)


Create a clock named clk with a period of 2.5, edit the sdc file accordingly. in the prelayout_sta.conf file read the lib file, Verilog file and link design then read sdc.
Invoke the sta tool and obtain the timing reports and observe the slack, data arrival and data required times from the reports.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/45_report_checks.jpg)


In the sta terminal, set all the clocks as propagated clocks and check timing reports and observe changes in the slack values. In the timing reports observe the launch clock network delay and capture clock network delay values. Observe the hold time and hold slack by reporting the hold violated paths. 



# Final Steps for RTL2GDS


In your project directory, perform qflow route, sta and backannotation steps on the design to complete the implementation flow. You can see the routing implementation in the qrouter window.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/46_qrouter.png)

![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/47_backannotate.png)


Open the sta log file and note the pre-layout frequency.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/48_prelayout_frequency.png)


Open the post_sta.log file and note the post-layout frequency. You will observe the drop in frequency which is due to the parasitics.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/49_postlayout_frequency.png)


# Acknowledgement
KUNAL GHOSH Director and Co-Founder of VLSI System Design (VSD) Corp. Pvt. Ltd
