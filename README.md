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
   ##### 4.Pre-layout timing analysis and importance of good clock tree
   ##### 5.Final steps for RTL2GDS 


## DAY_1
   RISC-V 
    ![FIG:1](https://github.com/ripudamank2/OpenLANE-Sky130-Physical-Design-Workshop/blob/main/IC.PNG)
