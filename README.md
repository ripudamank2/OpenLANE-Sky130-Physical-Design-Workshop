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
           a. Timing modelling using delay tables
           b. Timing analysis with ideal clocks using openSTA
           c. Clock tree synthesis TritonCTS and signal integrity
           d. Timing analysis with real clocks using openSTA
   ##### 5.Final steps for RTL2GDS
           a. Routing and design rule check (DRC)
           b. Power Distribution Network and routing
           c. TritonRoute Features
   ##### 6. Refrences
   ##### Acknowledgements


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
   This include Magic tool with 130nm PDK and you .def file .
   
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
   
   
 ## DAY_3 
 In this day we get to learn about the about the layout tool (magic) and in which we have implementented the layout of cmos-inverter as  shown in below figures. 
            
             's' will select only one via or layer 
            's' 's' two times will select the full path in magic tool 
     
   ![FIG:10](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/nmos.JPG)
   ![FIG:11](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/pmos.JPG)
   ![FIG:12](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/out_connection.JPG)
   ![FIG:13](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/source_vdd.JPG)
   ![FIG:14](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/out_connection.JPG)
   
   

   **In the below picture you can see the how we can analyse the layout through spice tool** 

   ![FIG:15](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/layout_2_spice.JPG)
   ![FIG:16](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/simfile.png)
   ![FIG:17](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/trans.JPG) 
   
   
   
   
   ## DAY_4 
  
  
  This day was bit interested in which we had inserted the inverted which we had designed earlier adn then we synthesis the full design using that newly design inverter
  and later we done the timing analysis to find the slack and uses ways to optimize the slack because we were getting negative slack . The neagtive slacck is bad for design.
  So we done some analysis through timing report and optimize the buffer paths to reduce the delay.
  
   **Skew** is the time delta between the actual and expected arrival time of a clock signal.
   ![FIG:18](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/skew.JPG)
   
   **Hold Time**: the amount of time the data at the input must be stable after the active edge of clock.
   ![FIG:19](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/hold_time.JPG)
    
   **Setup Time**: the amount of time the data at the input must be stable before the active edge of clock
   ![FIG:20](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/setup_time.JPG)
   
   **Plug the custom LEF to openlane flow**
   If a new custom cell needs to be plugged into openlane flow, include the lefs (the one extracted in Step-5) as below:

   In the design's config.tcl file add the below line to point to the lef location which is required during spice extraction.

             set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
   the below command to include the additional lef into the flow:

             set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
  
             add_lefs -src $lefs
   Note: A inverter magic file (sky130_vsdinv.mag) has been included as a reference resource.
   
   ![FIG:21](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/cell_inserted.JPG)
   ![FIG:22](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/cell_expand.JPG)
   
   
  **One way to reduce the slack is to optimize the buffers in the path which has higher fanout.
   ![FIG:23](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/slack_occur.JPG)
   ![FIG:24](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/modify_cell_delay.png)
   ![FIG:25](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/new_slack.JPG)
   
   
   
   **Way to remove and insert the clk_buff** 
   
   
   ![FIG:26](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/clk_buff_remove.jpg)
   ![FIG:27](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/how_add_clk_buff.JPG)
   
   
   
   #### DAY_5
   In this last day we undertsand how to generate power analysis report using gen_pdn and full routing technique for our custom asic.
   In your OpenLane interactive view type this command
     
              echo $::env(CURRENT_DEF)   ---> see current def 
   then run this command
   
                 gen_pdn
   
   THIS COMMAND WILL CREATE POWER AND GND RAILS FOR CELLS.
   After completing the above step last step is to rounting 
   
                 get_rounting 
      
   it will take some time. But remember you need to get the result violation and DRC free to go for fabrication .
   Last Step is to generated the SPEF file with separate SPEF tool which is not included in OpenLane.
   This tools take two files **merged.lef and design.def**  to extract the parasitics. **(SPEF_EXTRACTOR)**
   
          python3 main.py <.LEF> <.DEF>
    
    
          
                    
    
   Below Figures show the final layout with the standard inverter cell which was inserted in the picorv32a . 
    
   ![FIG:28](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/final_layout.JPG)
   ![FIG:29](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/std_cell_lay.JPG)
   
   
  
  
#### Refrences
      https://github.com/nickson-jose/vsdstdcelldesign
      https://skywater-pdk--136.org.readthedocs.build/en/136/
      https://github.com/google/skywater-pdk
      http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
      
      
      
#### Acknowledgements:
* Kunal Ghosh, Co-founder (VSD Corp. Pvt. Ltd)
* Nickson P Jose, Teaching Assistant (VSD Corp. Pvt. Ltd)      

   
   
   
  
     
     
   
            
   
