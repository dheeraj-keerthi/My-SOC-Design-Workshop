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

  Commands to exit from current run

```tcl
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```

## Section 3 - Design library cell using Magic Layout and ngspice characterization 

### Implementation

Objectives: 

1.Clone the GitHub repository for the Standard Cell Design and Characterization using the OpenLANE flow: [Standard cell design](https://github.com/nickson-jose/vsdstdcelldesign).

2.Open the custom inverter layout in Magic for inspection and exploration.

3.Perform SPICE extraction of the inverter design using Magic.

4.Modify the SPICE model file for further analysis and simulation.

5.Conduct post-layout simulations using ngspice.

6.Rise and Fall Transition Time Calculations.

7.Cell Delay Calculations.

8.Correcting DRC Errors in SkyWater SKY130 Process Using Magic VLSI Layout Tool.

### Task 1: Cloning Custom Inverter Standard Cell Design from GitHub Repository
1.Navigate to the OpenLANE working directory:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
  2.Clone the custom inverter design repository
  ```bash
git clone https://github.com/nickson-jose/vsdstdcelldesign
```
3.Move into the repository's directory
```bash
cd vsdstdcelldesign
```
4.Copy the Magic tech file for easier access
```bash
cp ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
```
5.Verify the contents of the directory
```bash
ls
```
6.Open the custom inverter layout in Magic
```bash
magic -T sky130A.tech sky130_inv.mag &
```
Ensure that you have successfully cloned the repository and opened the custom inverter layout using Magic for further exploration. The screenshot of the commands run is also shown below.

![clone](https://github.com/user-attachments/assets/cd3496ac-4450-4525-931f-ebe1432a3504)

### Task 2: Open the custom inverter layout in Magic for inspection and exploration.

After opening the custom inverter layout into Magic, the design exploration can be conducted by identifying critical components and connections, including NMOS, PMOS, and various signal paths.
* Steps:
  1.Load the layout in Magic:
  ```bash
  magic -T sky130A.tech sky130_inv.mag &
  ```
  2.NMOS and PMOS Identified:
  * Both NMOS and PMOS transistors were identified within the layout. Screenshots of these are provided below.
  ![nmospmos](https://github.com/user-attachments/assets/e4feffe1-147c-4dc6-99c2-32a551b98fc7)
 3.Verification of Output (Y) Connectivity to PMOS and NMOS Drain:
  * The connectivity from the output Y to the PMOS and NMOS drains was checked and verified.
  ![VirtualBox_vsdworkshop_11_10_2024_15_53_30](https://github.com/user-attachments/assets/77c2bed6-e9b9-46ec-94ca-90b2d47edf19)
 * A portion of the layout was deliberately deleted to trigger a DRC (Design Rule Check) error, and the tool successfully flagged the error.
![error](https://github.com/user-attachments/assets/86a20df5-75b8-430a-9e1e-3e26f3bd15fa)
### Task 3: Perform SPICE extraction of the inverter design using Magic.

To extract the SPICE netlist from the custom inverter layout in Magic, follow these steps in the tkcon window:

Commands for SPICE Extraction:
1.Check the current directory
```bash
pwd
```
2.Extract the layout into .ext format
```bash
extract all
```
3.Enable parasitic extraction (capacitance and resistance thresholds set to 0 for full extraction)
```bash
ext2spice cthresh 0 rthresh 0
```
4.Convert .ext file to SPICE format
```bash
ext2spice
```
Screenshots:

1.TKcon window after running extraction commands:

![spice](https://github.com/user-attachments/assets/436b2320-e622-478d-8f9a-95fcbbcfa4fd)

2.Generated SPICE file:
![spicefiles](https://github.com/user-attachments/assets/4f9f9a38-dee0-412a-9f91-321f2f9cdaad)

### Task 4: Modify the SPICE model file for further analysis and simulation.
* Measuring Unit Distance in Layout Grid

  Before editing the SPICE model file, ensure the grid spacing and unit distance in the layout are accurate. This will help in verifying and adjusting the extracted device dimensions.

  * Final Edited SPICE File for ngspice Simulation

  After extracting the SPICE file, review and edit it as needed for ngspice simulation. Ensure that the netlist includes accurate connections and the appropriate device models for the Skywater 130nm process.

* Screenshot: Edited SPICE file
  ![editedspice](https://github.com/user-attachments/assets/1b138196-5d4d-422e-9f34-5c0e5c423b55)
### Task 5: Conduct post-layout simulations using ngspice.

* Running the SPICE Simulation

  To run a post-layout simulation in ngspice, use the following commands to load the SPICE file and plot the output.

  1.Load the SPICE file into ngspice
  ```bash
  ngspice sky130_inv.spice
  ```
  2.Plot output y vs time a (input)
  ```bash
  plot y vs time a
  ```


* Screenshot: Running the ngspice simulation
  ![VirtualBox_vsdworkshop_11_10_2024_22_06_31](https://github.com/user-attachments/assets/8226d12a-b700-463c-916a-3965c09034c9)
   * Screenshot: Generated plot
![VirtualBox_vsdworkshop_11_10_2024_22_09_05](https://github.com/user-attachments/assets/94c28b1a-34f6-46b4-9a7d-19c56b394009)
### Task 6:  Rise and Fall Transition Time Calculations:

1. Rise Transition Time Calculation:

```math
Rise\ transition\ time = Time\ taken\ for\ output\ to\ rise\ to\ 80\% - Time\ taken\ for\ output\ to\ rise\ to\ 20\%
```
```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```
```math
Rise\ transition\ time = 2.24592 - 2.18213 = 0.06376\ ns = 63.76\ ps
```
![VirtualBox_vsdworkshop_11_10_2024_22_25_00](https://github.com/user-attachments/assets/9300eab2-02d8-407a-8e52-98bd1a5597c4)
2. Fall Cell Delay Calculation:
![VirtualBox_vsdworkshop_11_10_2024_22_30_11](https://github.com/user-attachments/assets/7405ab9f-a576-4662-857c-e76587a98a8c)
### Task 8: Correcting DRC Errors in SkyWater SKY130 Process Using Magic VLSI Layout Tool

1.Download and Extract the DRC Test Files

Start by downloading the required test files and extracting them in your home directory.

  1. Change to the Home Directory:

     ```bash
     cd
     ```
  2. Download the Lab Files:

     ```bash
     wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz\
     ```
  3. Extract the Compressed Files:

     ```bash
     tar xfz drc_tests.tgz
     ```
  4. Navigate to the Lab Directory:

     ```bash
     cd drc_tests
     ```
  5. List the Contents of the Directory:
     ```bash
     ls -al
     ```
  ![VirtualBox_vsdworkshop_11_10_2024_22_46_29](https://github.com/user-attachments/assets/e63862dc-9495-4f89-b162-558e7a0748c1)
  2.View the .magicrc Configuration File

Before proceeding, it’s a good idea to inspect the .magicrc file to ensure that Magic is configured properly.

  * Viewing the .magicrc File:

    
     ```bash
     # Open the .magicrc file in a text editor
     gvim .magicrc
     ```
![VirtualBox_vsdworkshop_11_10_2024_22_47_49](https://github.com/user-attachments/assets/c1bef0de-6ffd-407d-9dbd-e02d8fb1e702)
3.Open the Magic Tool with the Updated Tech File

Start Magic in a better graphics mode to view the design and identify DRC violations.

```bash
# Open Magic in XR mode
magic -d XR &
```

4.Fixing Incorrectly Implemented poly.9 Rule

The poly.9 rule defines a minimum spacing rule that was not properly enforced. We will update the DRC rule and verify the correction.

Screenshot of poly rules:


![poly 9 rule](https://github.com/user-attachments/assets/a692e665-8d3d-4d9f-8c06-030f2a905698)

![VirtualBox_vsdworkshop_11_10_2024_22_59_01](https://github.com/user-attachments/assets/3173e956-baf4-4890-9771-e672abf70cdb)

![VirtualBox_vsdworkshop_11_10_2024_23_01_18](https://github.com/user-attachments/assets/87e1a618-6985-4f32-8c23-8efee9682f24)

![VirtualBox_vsdworkshop_11_10_2024_23_04_35](https://github.com/user-attachments/assets/ad2b19c9-ea57-4186-8928-8d73d9826c7c)

Solution: Update the Sky130 DRC tech file.

Inserting New Commands into sky130A.tech File:

open sky130A.tech file

```bash
gvim sky130A.tech
```

Screenshot after Inserting new commands into sky130A.tech file


![edited poly 9 1](https://github.com/user-attachments/assets/0b9d6289-2db0-49aa-a08e-54903e47e76d)


![edited poly 9 2](https://github.com/user-attachments/assets/97dc33f5-92e3-4d2c-b8e9-23e163678f42)

Commands to Run:

```tcl
# Load the updated tech file
tech load sky130A.tech
```
```tcl
# Run DRC check
drc check

```
```tcl
# Select region with errors and view messages
drc why
```

Post-fix Screenshot:

![poly 9 corrected](https://github.com/user-attachments/assets/df147bb8-70c9-47bd-945f-cf37685c2ea9)

5.Fixing Incorrectly Implemented difftap.2 Rule

The difftap.2 rule was improperly implemented, with no DRC violation for spacing < 0.42u.

Screenshot of difftap rules:

![difftap 2](https://github.com/user-attachments/assets/d9bde2af-03a0-46d6-9d4f-5f6669ecc7a8)

Problem: No violation occurred for difftap spacing < 0.42u.

![difftap error](https://github.com/user-attachments/assets/82625354-1267-45f5-bb10-6320d79d90ee)

Solution: Update the Sky130 DRC tech file.

Inserting New Commands into sky130A.tech File:

![edited difftap 2](https://github.com/user-attachments/assets/04840728-0ba4-4b31-9dc6-94145be686e8)

Commands to Run:

```tcl
# Load the updated tech file
tech load sky130A.tech
```
```tcl
# Run DRC check
drc check

```
```tcl
# Select region with errors and view messages
drc why
```

Post-fix Screenshot:

![difftap corrected](https://github.com/user-attachments/assets/ede865a5-c634-4b5f-8108-8af527ffc36b)

6.Fixing Incorrectly Implemented nwell.4 Rule

The nwell.4 rule, which enforces the presence of a tap within the nwell, was also improperly implemented.

Screenshot of nwell rules:

![nwell 4 rule](https://github.com/user-attachments/assets/55d3fec3-0227-44c0-a70b-73ca6ecf8131)

Problem: No DRC violation occurred even though a tap was missing from the nwell.


![nwell error](https://github.com/user-attachments/assets/ce14dc30-1fe4-4633-afa1-b433bdcd8efd)



Solution: Update the Sky130 DRC tech file.

Inserting New Commands into sky130A.tech File:

![edited nwel 1](https://github.com/user-attachments/assets/65398b05-5cf4-4e76-9a49-200181c51eab)

![edited nwel 2](https://github.com/user-attachments/assets/79c55f1a-65cd-4713-92df-df7be286a766)


Commands to Run:

```tcl
# Load the updated tech file
tech load sky130A.tech
```

```tcl
# Change drc style to drc full
drc style drc(full)
```

```tcl
# Run DRC check
drc check

```
```tcl
# Select region with errors and view messages
drc why
```

Post-fix Screenshot:

![corrected nwell](https://github.com/user-attachments/assets/40f87039-fefe-46ed-8cbe-113875dc8314)

# Select region with errors and view messages
drc why
```

Post-fix Screenshot:
![corrected nwell](https://github.com/user-attachments/assets/40f87039-fefe-46ed-8cbe-113875dc8314)

  











  







