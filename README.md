# OpenLANE-Sky130-Physical-Design-Workshop
Physical design is the process of turning a design into fabricable geometries. It comprises a number of steps, including floorplanning, placement, clock tree synthesis, and routing. 
  1. Physical design begins with a netlist, which is synthesized from RTL. The netlist describes the components of a circuit and there connection.
  2. Floorplanning is the big step which involves identifying which IPs or circuits should be placed near others, taking into account area restrictions, speed, and                 the various constraints required by IPs.
  3. Placement determines the locations of each component on the chip or die , considering timing and interconnect length. 
  4. Clock tree synthesis involves inserting buffers or inverters such that the clock is distributed evenly in a design, minimizing skew and delay. 

The final output of the physical design process is typically GDSII.

### Table of Content
   ##### 1.Inception of open-source EDA, OpenLANE and Sky130 PDK
           a.  Chip Floor planning considerations
           b.  Library Binding and Placement
           c.  Cell design and characterization flows
           d.  General timing characterization parameter
   ##### 2.Understand importance of good floorplan vs bad floorplan and introduction to library cells
           a.  Chip Floor planning considerations
           b.  SKY130_D2_SK2 - Library Binding and Placement
           c.  Cell design and characterization flows
           d.  General timing characterization parameters
   ##### 3.Design and characterize one library cell using Magic Layout tool and ngspice
            a. Labs for CMOS inverter ngspice simulations
            b. Inception of Layout – CMOS fabrication process
            c. Sky130 Tech File Labs
   ##### 4.Pre-layout timing analysis and importance of good clock tree
   ##### 5.Final steps for RTL2GDS 


## DAY_1 - Its was intorduction day which explain about EDA , Openlane and skylane130nm library.
   Architecture of Integrated Circuit with different IP blocks:
   
   ![FIG:1](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/IC.PNG)

   Layout of Integrated Circuit :
   
   ![FIG:2](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/layout_IC.PNG)
   
  
 **OpenLANE** is a an Architecture which consist of different tool which help an IC DESIGNER to covert RTL code to GDSII/LEF. In our lab we have use the OpenLane in an interactive way as shown below .
  **1.Synthesis**
  
        a. yosys - Performs RTL synthesis
        b. abc - Performs technology mapping
        c. OpenSTA - Pefroms static timing analysis on the resulting netlist to generate timing reports
 
 **2.Floorplan and PDN**
 
        a. init_fp - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
        b. ioplacer - Places the macro input and output ports
        c. pdn - Generates the power distribution network
        d. tapcell - Inserts welltap and decap cells in the floorplan

  **3.Placement**

         a. RePLace - Performs global placement
         b. Resizer - Performs optional optimizations on the design
         c. OpenPhySyn - Performs timing optimizations on the design
         d. OpenDP - Perfroms detailed placement to legalize the globally placed components
  
  **4.CTS**
  
         a.TritonCTS - Synthesizes the clock distribution network (the clock tree)
  **5.Routing**

         a.FastRoute - Performs global routing to generate a guide file for the detailed router
         b.TritonRoute - Performs detailed routing
         c.SPEF-Extractor - Performs SPEF extraction
  **6.GDSII Generation**
        
        a.Magic - Streams out the final GDSII layout file from the routed def

**Below picture show the flow of OpenLane :**

   ![FIG:3](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/openlane.flow.1.png)
   
   
   
   
   
   
   
   
  In this LAB1 we had to synthesis a design picorv32a by using OpenLANE -interactive mode.
  If your using the lab session from https://www.vlsisystemdesign.com/ then you dont need to configure Docker but if your using in your PC you need to configure docker.
  So i didnt use my PC for this practical session , so i just need to open flow.tcl file in intercative mode . Its script which will invoke the Openlane. 
  
            ./flow.tcl -interactive
  
  Include the package by using the below command

             package require openlane 0.9
             
  After that you need to prepare your design files "picorv32a" with below command

             prep -design ./designs/picorv32a
             
  When the design is prepared run syntheis by using below command

              run_synthesis
          
  Finally we need check the total area occupied by picorv32a in form of flipflops, adders, AND, OR gates etc. :
        ![FIG:3](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/image.png)
    
    
    
## DAY_2 - It give us inforamtion of floorplan and standard cell   

  **Floorplanning** is the art of any physical design. A well and perfect floorplan leads to higher performance and optimum area in an ASIC.
  Floorplanning can be challenging.It deals with the placement of I/O pads and macros as well as power and ground structure.
  Before we are going for the floor planning to make sure that inputs are used for floorplan is prepared properly.

    Inputs for floorplan:
       Netlist (.v)
       Technology file (techlef)
       Timing Library files (.lib)
       Physical library (.lef)
       design constraints 
       
  
  There are basically three ways to design VLSI circuits; either gate array, standard cell or full custom layouts can be used
  We will be working with standard cell which is a group of transistor and interconnect structures that provides a boolean logic function . the deve;opment tim to design a        standard cell is 10 - 14 weeks 
   
In this workshop as you alreday know from above steps we are uisng Openlane for development purpose . Below are some information on openlane .

   The below image show about the interactive mode of openLANE.
   ![FIG:4](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/openlane.JPG)
   
   If you need to update some parameter in **config.tcl** file in your project directory after you had done prep -desifgn.
   There are basically two option 
   1. , it reflect in the tool only you do prep **-design again** with the **-overwrite** flag.  
   2.  **set env(parameter_name) parameter_value**  from within openlane flow.
   
   ![FIG:5](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/openl.JPG)
   
   
   Then to check the floor of your IP block after running this command in openlane (run_floorplane) is shown in beelow picture.
   This include tool Magic with 130nm PDK and you .def file .
   ![FIG:6](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/floorplan.JPG)
   
   Some basic command to use in magic 
           
         s – select
         v — fit layout
       
       zoom  -left mosue click and then right mosue click then z 
       Key macro z implements the command zoom 0.5 (zoom out by a factor of 2).
       Key macro Z implements the command zoom 2 (zoom in by a factor of 2).
   **Reference**    http://opencircuitdesign.com/magic/userguide.html. 
   
       i/o pins equal distance  ---to see the layer of pin …just move the mouse at that pin and click  s 
        in tkon will show layer information by typing "what"  

   
   ![FIG:7](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/tkon_placem.JPG)
   
   
   After running the placement command in openLane(run_placement). Then we need to see the placement of standard cells which can be shown in the below pictures 
   
   ![FIG:8](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/place_command.JPG)
   
   ![FIG:9](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/after_placement.JPG)
   
   
