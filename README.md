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

# Study and review various components of RISC-V based picoSoC
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


# Chip planning strategies and introduction to foundry library cells

You can press the Edit Layout button in the Qflow manager window to view the layout or you can invoke qflow in your project directory (use qflow diplay picorv32 & command) to view the layout window as shown below. Select the whole chip by first moving the cursor to the bottom left and left mouse click and then moving to top right and right mouse click, then press Shift+i.


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/16_select_chip.png)


Now you will see the selected chip are in the tkcon window. 


![alt text](https://github.com/1-anita-h/SoCPhysicalDesign-VSDWorkshop/blob/main/Images/17_chip_area.png)


# Design and Characterize one library cell using Magic Layout tool and ngspice

# Pre-Layout Timing Analysis and importance of good clock tree

# Final Steps for RTL2GDS

# Acknowledgement
