# Digital-VLSI-SoC-Design-and-Planning
This is a  comprehensive 2-week hands-on workshop covering the complete RTL to GDSII flow for VLSI SoC design, organized by VSD in collaboration with NASSCOM.
## Section-1- Inception of open-source EDA, OpenLANE and Sky130 PDK 

### Implementation
Objectives:
1. Running 'picorv32a' using OpenLANE and generating necessary outputs.
2. Calculating the flop ratio based on synthesis results.

### Task 1: Running 'picorv32a' using OpenLANE
Here is step by step procedure to obtain synthesis results using OpenLANE.
1. Change to the OpenLANE flow directory:
 use the command cd to enter into Desktop-->work-->tools-->openlane_working_dir-->openlane

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
2.(Optional) Set up Docker alias if needed:
  ```bash
alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
```
3.Start the Docker container:
```bash
docker
```
4.Enter OpenLANE interactive mode:
using which the flow has to go
```tcl
./flow.tcl -interactive
```
5.Load the OpenLANE package:
```tcl
package require openlane 0.9
```
6.Prepare the design 'picorv32a':
```tcl
prep -design picorv32a
```
7.Perform the synthesis:
```tcl
run_synthesis
```
8.Exit the OpenLANE flow:
```tcl
exit
```
Synthesis Output:
Below are screenshots showcasing the execution of synthesis:
* Preparation Completion:

  ![VirtualBox_vsdworkshop_07_10_2024_15_13_48](https://github.com/user-attachments/assets/231d4885-710e-437b-9dfa-336dbdeadb8c)
* Synthesis Running:

![VirtualBox_vsdworkshop_07_10_2024_15_41_00](https://github.com/user-attachments/assets/66fa00da-2f8d-43fa-acc2-984da9c331ee)

### Task 2: Calculating Flop Ratio
We calculate the Flop Ratio and the percentage of D Flip-Flops (DFF) using the synthesis statistics report.
Formulae:

```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
```math
Percentage\ of\ DFF's = Flop\ Ratio * 100
```
Synthesis Report:

![VirtualBox_vsdworkshop_07_10_2024_15_42_06](https://github.com/user-attachments/assets/555efaef-8b20-4bf1-aac0-29cfe473239a)

Values from the Report:
* Number of D Flip-Flops: 1613
* Total Number of Cells: 14876


Calculation:

```math
Flop\ Ratio = \frac{1613}{14876} = 0.108429685
```
```math
Percentage\ of\ DFF's = 0.108429685 * 100 = 10.84296854\ \%
```

## Section 2 - Good Floorplan vs Bad Floorplan and Introduction to Library Cell 

### Implementation
Objectives: 
1.Run 'picorv32a' Design Floorplan Using OpenLANE.

2.Calculate Die Area in Microns.

3.Load Floorplan DEF in Magic Tool.

4.Run Congestion-Aware Placement.

5.Load Placement DEF in Magic Tool.
### Task 1: Run 'picorv32a' Design Floorplan Using OpenLANE.

Steps to perform Floorplan:
  1.Change to the OpenLANE flow directory:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
  2.(Optional) Set up Docker alias if needed:
  ```bash
alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
```
3.Start the Docker container:
```bash
docker
```
4.Enter OpenLANE interactive mode:
```tcl
./flow.tcl -interactive
```
5.Load the OpenLANE package:
```tcl
package require openlane 0.9
```
6.Prepare the design 'picorv32a':
```tcl
prep -design picorv32a
```
7.Perform the synthesis:
```tcl
run_synthesis
```
8.Run the floorplan step:
```tcl
run_floorplan
```
9.Exit the OpenLANE flow:
```tcl
exit
```

Floorplan Output:

![floorplanning](https://github.com/user-attachments/assets/b60781cf-1c13-4d3c-86ca-99c5f5980d3b)
![floorplanning](https://github.com/user-attachments/assets/acc690f3-e4f6-4059-9ebf-051d048fe5db)

#### Task 2: Calculate Die Area in Microns.

 Screenshots showcasing the floorplan DEF file:
 ![die area](https://github.com/user-attachments/assets/a2617c7b-4213-48a5-b961-31aaa20e9cc4)
 According to floorplan def

```math
 1\ Micron = 1000\ Unit\ Distance 
```
```math
Die\ width\ in\ unit\ distance = 660685 - 0 = 660685
```
```math
Die\ height\ in\ unit\ distance = 671405 - 0 = 671405
```
```math
Distance\ in\ microns = \frac{Value\ in\ Unit\ Distance}{1000}
```
```math
Die\ width\ in\ microns = \frac{660685}{1000} = 660.685\ Microns
```
```math
Die\ height\ in\ microns = \frac{671405}{1000} = 671.405\ Microns
```
```math
Area\ of\ die\ in\ microns = 660.685 * 671.405 = 443587.212425\ Square\ Microns
```

### Task 3: Load Floorplan DEF in Magic Tool.

Commands to Load Floorplan DEF in Magic:
1.Change directory to path containing generated floorplan def
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/07-09_20-37/results/floorplan/
```
2.Command to load the floorplan def in magic tool
```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
* Floorplan Loaded in Magic:

The floorplan DEF file has been successfully loaded into Magic for visualization.
![magicT](https://github.com/user-attachments/assets/72d350c0-1aba-4a77-93e4-9efeece8050a)


* Equidistant Placement of Ports:
This image showcases the equidistant placement of ports, which was configured in the config.tcl file to ensure optimal distribution.
![VirtualBox_vsdworkshop_08_10_2024_23_47_21](https://github.com/user-attachments/assets/40741442-99d3-46f6-9cca-1e0314d5f971)

* Port Layer Configuration:

Ports are placed on the layer as defined by  the config.tcl file.
![VirtualBox_vsdworkshop_08_10_2024_23_45_36](https://github.com/user-attachments/assets/c4aa3e4d-0405-4ba9-b446-97e778de3225)

![VirtualBox_vsdworkshop_08_10_2024_23_51_49](https://github.com/user-attachments/assets/675ac609-07ce-41f2-8162-12f30863436a)

* Decap Cells and Tap Cells & Diagonally Equidistant Tap Cells:
  ![VirtualBox_vsdworkshop_08_10_2024_23_55_43](https://github.com/user-attachments/assets/1bb0373d-dfb0-4e48-9576-5aa29e373c1a)

* Unplaced Standard Cells at Origin:

This image shows the standard cells that are currently unplaced, located at the origin, which is typical prior to placement optimization.
![VirtualBox_vsdworkshop_09_10_2024_00_06_40](https://github.com/user-attachments/assets/f7a39731-e157-48bf-a726-5934cd2f2a5c)

### Task 4:Run Congestion-Aware Placement.

Command to run placement
```bash
run_placement
```
* Placement Completion:


  The placement run has successfully completed, with the design placed according to congestion and timing constraints.

  ![run_placement](https://github.com/user-attachments/assets/5ef8d1cf-6504-4d4a-b245-824682e8c15f)

### Task 5: Load Placement DEF in Magic Tool.
To load the generated placement DEF file in the Magic tool, use the following commands in a separate terminal:
Commands to Load Placement DEF in Magic
1.Navigate to the directory containing the placement DEF file:
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/07-09_20-37/results/placement/
```
2.Load the placement DEF in Magic:
```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech \
lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
* Placement DEF Loaded in Magic
  
  The placement DEF file has been successfully loaded into Magic, showing the placement of standard cells and other components.
  ![VirtualBox_vsdworkshop_10_10_2024_01_29_39](https://github.com/user-attachments/assets/aa5366a1-ebce-41ae-8086-e974083bda88)

 * Legally Placed Standard Cells
  
  This screenshot shows the standard cells placed legally according to the design rules, ensuring a functional and optimized layout.
  ![VirtualBox_vsdworkshop_10_10_2024_01_32_14](https://github.com/user-attachments/assets/14a959a5-23d7-42c8-a473-78033854137f)




  







