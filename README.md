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
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/07-10_09-38/results/floorplan/
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
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/07-10_09-38/results/placement/
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


## Section 4 - Pre-layout timing analysis and importance of good clock tree.

### Implementation

Objectives:

1.Fix minor DRC errors and verify the design for flow insertion.

2.Save the finalized layout with a custom name and open for verification.

3.Generate the LEF file from the layout.

4.Copy the LEF and required library files to the 'picorv32a/src' directory.

5.Edit config.tcl to update the library file and add the new LEF into OpenLane flow.

6.Run OpenLane synthesis with the newly inserted custom inverter cell.

7.Address any violations caused by the custom cell by adjusting design parameters.

8.Run floorplanning and placement, verifying the custom cell's integration in the PnR flow.

9.Perform post-synthesis timing analysis using the OpenSTA tool.

10.Apply timing ECO fixes to resolve any timing violations.

11.Replace the old netlist with the updated netlist and implement floorplan, placement, and CTS.

12.Conduct post-CTS timing analysis using OpenROAD.

13.Optimize post-CTS timing by removing sky130_fd_sc_hd__clkbuf_1 from CTS_CLK_BUFFER_LIST.

### Task 1: Fix minor DRC errors and verify the design for flow insertion.

Conditions to Verify:
  1. Conditiion 1:
     * Input and output ports must lie on the intersection of vertical and horizontal tracks.
  2. Condition 2:
     * The width of the standard cell should be an odd multiple of the horizontal track pitch.
  3. Condition 3:
     * The height of the standard cell should be an even multiple of the vertical track pitch.

Commands to Open the Layout in Magic

```bash
# Change directory to the vsdstdcelldesign folder
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
```

```bash
# Open the custom inverter layout in Magic
magic -T sky130A.tech sky130_inv.mag &
```

Track Information of sky130_fd_sc_hd

* The track information for the standard cell library sky130_fd_sc_hd is shown below. This will be used to verify the alignment and dimensions of the custom inverter cell.
![task1(1)](https://github.com/user-attachments/assets/5afcff87-b74a-4357-a99d-cab640e4d16e)
Setting Grid for Track Alignment

* Use the following commands in the tkcon window to set the grid according to the standard cell track pitch.

```tcl
# Get syntax for the grid command
help grid
```
```tcl
# Set grid values for track alignment (locali layer)
grid 0.46um 0.34um 0.23um 0.17um
```

Screenshot of Command Execution
![command execution](https://github.com/user-attachments/assets/2a640865-30d1-4cde-be15-b74e2d97175b)
Condition 1: Port Alignment on Tracks

* Verified that the input and output ports of the custom inverter cell are aligned at the intersections of vertical and horizontal tracks.

Condition 2: Width Verification
* The width of the standard cell should be an odd multiple of the horizontal track pitch 0.46𝜇𝑚

Horizontal track pitch=0.46 umWidth of standard cell=1.38 um=0.46×3

This condition is satisfied as the width is an odd multiple.
![cond2](https://github.com/user-attachments/assets/6c8a6afd-0892-42d9-9b70-11627ca7fe79)
Condition 3: Height Verification

* The height of the standard cell should be an even multiple of the vertical track pitch 0.34𝜇𝑚

Vertical track pitch=0.34 umHeight of standard cell=2.72 um=0.34×8

This condition is satisfied as the height is an even multiple.
![cond2](https://github.com/user-attachments/assets/6c8a6afd-0892-42d9-9b70-11627ca7fe79)
### Task 2: Save the finalized layout with a custom name and open for verification.

* Once the layout has been verified and finalized, follow the steps below to save it with a custom name and reopen it for verification.

Command to Save the Layout with a Custom Name in Magic (tkcon Window):

```tcl
# Command to save layout as a new file
save sky130_vsdinv.mag
```
![save sky130_](https://github.com/user-attachments/assets/9b6fe1c1-0b3d-44d5-a227-f6ac51f5fa2b)
Command to Open the Newly Saved Layout in Magic:

```bash
# Open the custom inverter layout in Magic
magic -T sky130A.tech sky130_vsdinv.mag &
```

Screenshot of the Newly Saved Layout:

![new saved layout](https://github.com/user-attachments/assets/395c52ee-3f99-4cbe-8737-367baf87a0b0)
### Task 3: Generate the LEF file from the layout.

* After finalizing the custom inverter layout, you can generate the LEF (Library Exchange Format) file, which is essential for integrating the cell into the design flow.

  Command to Write the LEF File in Magic (tkcon Window):

  ```tcl
  # Command to generate LEF
  lef write
  ```

  Screenshot of the Command Execution:
  ![lef write](https://github.com/user-attachments/assets/742c4688-0e9f-4cc5-8613-df424d83e6e4)
 Screenshot of the Newly Created LEF File:
![new lef](https://github.com/user-attachments/assets/4ff22a1a-2e97-4380-8855-7a575d8e2f5c)
### Task 4: Copy the LEF and required library files to the 'picorv32a/src' directory.

* After generating the LEF file, the next step is to copy it along with the required library files to the picorv32a design's src directory for integration.

  Commands to Copy LEF and Library Files:

  ```bash
  # Copy the LEF file to the 'picorv32a/src' directory
  cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
  ```
  ```bash
  # List and check if the LEF file is copied successfully
  ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
  ```
  ```bash
  # Copy the required Liberty (.lib) files
  cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
  ```
  ```bash
  # List and check if the .lib files are copied successfully
  ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
  ```

  Screenshot of Command Execution:
  ![task4](https://github.com/user-attachments/assets/007259c1-c7b0-4be8-8037-026b6c8d26ea)
  ![task4(ii)](https://github.com/user-attachments/assets/4d73bf6c-552c-4874-811c-bd7f82249d21)
  
### Task 5: Edit config.tcl to update the library file and add the new LEF into OpenLane flow.

* To ensure that the custom inverter cell is included in the OpenLane flow, update the config.tcl file by specifying the correct library files and adding the new LEF file.

Commands to Add to config.tcl:

```tcl
# Set the paths for the new library files
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

# Include the custom LEF file
set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

```
Screenshot of the config.tcl:
![task5](https://github.com/user-attachments/assets/cd5a6c99-6b44-401a-a968-4e8d0c4eb145)
### Task 6: Run OpenLane synthesis with the newly inserted custom inverter cell.

* To synthesize the design with the custom inverter cell included, follow the steps below.

Commands to Run OpenLANE Flow Synthesis:

```bash
# Change directory to the OpenLANE flow working directory
cd Desktop/work/tools/openlane_working_dir/openlane
```
```bash
# Invoke the OpenLANE Docker subsystem
docker
```
```tcl
# Enter Interactive Mode in OpenLANE flow
./flow.tcl -interactive

```
```tcl
# Load required packages
package require openlane 0.9
```
```tcl
# Prepare the 'picorv32a' design
prep -design picorv32a
```
```tcl
# Include the newly added LEF file into the OpenLANE flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```
```tcl
# Run synthesis for the design
run_synthesis
```

Screenshots of Commands Execution:
![run_synthesis](https://github.com/user-attachments/assets/ab3c6e5c-3bb6-4526-ab1a-8d1371ea22ef)

### Task 7: Address any violations caused by the custom cell by adjusting design parameters.

* After adding the custom inverter cell, the design may introduce timing violations. Here’s how to modify design parameters to improve timing.

  Initial Design Values Before Modifications:

  Screenshots of initial values:
  ![task 7(1)](https://github.com/user-attachments/assets/a792e948-e3cd-4006-b5d0-63cdf6d11b5c)
  ![task 7(2)](https://github.com/user-attachments/assets/1b3a6a54-c4b7-4fdc-9233-5773c84b6f8b)
Commands to Modify Parameters and Improve Timing:

  ```tcl
  # Prep the design again to update variables
  prep -design picorv32a -tag 13-10_10-15 -overwrite
  ```
  ```tcl
  # Include newly added lef into the flow
  set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
  add_lefs -src $lefs
  ```
  ```tcl
  # Display current value of SYNTH_STRATEGY
  echo $::env(SYNTH_STRATEGY)
  ```
  ```tcl
  # Set new value for SYNTH_STRATEGY
  set ::env(SYNTH_STRATEGY) "DELAY 3"
  ```
  ```tcl
  # Check if SYNTH_BUFFERING is enabled
  echo $::env(SYNTH_BUFFERING)
  ```
  ```tcl
  # Check current value of SYNTH_SIZING
  echo $::env(SYNTH_SIZING)
  ```
  ```tcl
  # Set new value for SYNTH_SIZING
  set ::env(SYNTH_SIZING) 1

  ```
  ```tcl
  # Check the current SYNTH_DRIVING_CELL
  echo $::env(SYNTH_DRIVING_CELL)
  ```
  ```tcl
  # Run synthesis again after updating parameters
  run_synthesis
  ```

  Result of the Synthesis:
  * Custom inverter cell is now included in the design.
  * Screenshot of the merged.lef file in the tmp directory confirming the custom inverter as a macro:
  ![task 7(4)](https://github.com/user-attachments/assets/094789c3-a232-4e62-9596-e4b1a0c3d1d5)
 Comparing Results After Modifications:

* Area: Increased slightly.
* Worst Negative Slack: Improved, now at 0.
![task7(3)](https://github.com/user-attachments/assets/906efc87-4e7f-4d8e-a27b-46800b454e66)

### Task 8: Run floorplanning and placement, verifying the custom cell's integration in the PnR flow.

* Once the custom inverter cell has been accepted during synthesis, we proceed with floorplanning and placement in the PnR flow.

Floorplan Command
Run the run_floorplan command:
```tcl
# Command to run floorplan
run_floorplan
```
However, encountering errors while using this command led us to use the following individual commands to manually initiate the floorplan process:

```tcl
# Initialize floorplan
init_floorplan
```
```tcl
# Place I/O pins
place_io
```
```tcl
# Add tap and decap cells
tap_decap_or
```
Floorplan Command Execution Screenshots:
    * Running floorplan:
![task 8](https://github.com/user-attachments/assets/0f61e5aa-40df-4dd5-831f-ddec6e8a583f)
Placement Command
*  Once the floorplan is complete, we proceed with placement using the following command:

   ```tcl
   # Command to run placement
    run_placement
   ```

   Placement Command Execution Screenshots:

   * Placement process:
![task 8(1-2)](https://github.com/user-attachments/assets/4886a8bd-9ca5-43db-84a9-dccef4dbebec)
 Load Placement DEF in Magic for Visualization
  * To verify that the custom inverter cell has been accepted into the flow and placed correctly, we load the generated placement DEF into Magic.

  ```bash
  # Change directory to placement result folder
  cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-10_10-15/results/placement/
  ```
  ```bash
  # Load placement def in magic
  magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
  ```
Magic Visualization Screenshots:
* Viewing the placement DEF in Magic:
![task 8(2)](https://github.com/user-attachments/assets/0acdfb6b-f596-47d7-b050-36f5f8ad67bc)
View Internal Layers of Cells

* We use the expand command in Magic to view internal connectivity layers of the placed cells, ensuring correct abutment.

  ```tcl
  # Command to view internal connectivity layers
  expand
  ```
  Abutment Verification Screenshots:

  * Custom inverter cell properly abutting power pins with other cells from the library:
![task 8(1-2)](https://github.com/user-attachments/assets/6a667c8c-2b14-4455-adf6-a4ad2787c40e)

### Task 9: Perform post-synthesis timing analysis using the OpenSTA tool.

With synthesis completed and the custom inverter cell accepted, we move on to performing a timing analysis using the OpenSTA tool. The goal is to analyze the timing of the initial synthesis run, which had numerous violations before any timing improvements were made.


Initial Synthesis and STA Setup

  1.Run Synthesis Command:
  * Navigate to the OpenLANE flow directory and run the synthesis with the following commands:

  ```bash
  cd Desktop/work/tools/openlane_working_dir/openlane
  ```
  ```bash
  docker
   ```
  Inside the Docker container:
      
  ```tcl
  ./flow.tcl -interactive
  ```
  ```tcl
  package require openlane 0.9
  ```
  ```tcl
  prep -design picorv32a
  ``````tcl
  set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
  add_lefs -src $lefs
  ```
  ```tcl
  set ::env(SYNTH_SIZING) 1
  ```
  ```tcl
  run_synthesis
  ```
      
![mybase_sdc](https://github.com/user-attachments/assets/566d22ed-883d-4488-ab90-c021c3b14529)
Run STA Analysis:

   Execute the STA analysis using OpenSTA tool:
   ```bash
   cd Desktop/work/tools/openlane_working_dir/openlane
   ```
   ```bash
   sta pre_sta.conf
   ```
   Screenshots of STA Commands and Results:
![task9(3)](https://github.com/user-attachments/assets/75c640c2-fe5f-4e54-9418-f6c34abd3ec0)
Post-Synthesis Timing Improvement

  1.Adjust Parameters:

  * Based on initial STA results indicating high fanout and delays, modify the synthesis parameters to reduce fanout and improve timing:

 2.Run STA Analysis Again:

  * Re-run the STA analysis with the updated design:

   ```bash
   cd Desktop/work/tools/openlane_working_dir/openlane
   ```
   ```bash
   sta pre_sta.conf
   ```
  Screenshots of STA Commands and Results after Adjustment:
![task9(4(](https://github.com/user-attachments/assets/0d89830c-be73-47a6-a5d6-37511cd02158)
![task9(5(](https://github.com/user-attachments/assets/464496a5-0f3a-4b32-afd1-ef682af309cb)
### 10. Make timing ECO fixes to remove all violations.

OR gate of drive strength 2 is driving 4 fanouts
![task 10](https://github.com/user-attachments/assets/cc2782c8-b90f-43df-a591-61f01491f777)
![task 10-2](https://github.com/user-attachments/assets/7f9e258f-3329-4391-8a94-a374ca760435)
![task 10-3](https://github.com/user-attachments/assets/1df51c24-3706-465a-9dd9-94b249387307)
Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```
# Reports all the connections to a net
report_net -connections _11672_

# Checking command syntax
help replace_cell

# Replacing cell
replace_cell _14510_ sky130_fd_sc_hd__or3_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
Result - slack reduced
  ![task 10-4](https://github.com/user-attachments/assets/71893e60-2bd9-4805-ba31-2c8b7612f1cd)
Commands to perform analysis and optimize timing by replacing with OR gate of drive strength 4

```
# Reports all the connections to a net
report_net -connections _11668_

# Replacing cell
replace_cell _14506_ sky130_fd_sc_hd__or4_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
![task 10-5](https://github.com/user-attachments/assets/fb37e512-be61-454c-8e27-c75588b9acab)
Commands to confirm that instance _14506_ has been replaced with sky130_fd_sc_hd__or4_4.

```
# Generating custom timing report
report_checks -from _29043_ -to _30440_ -through _14506_
```
![task 10-6](https://github.com/user-attachments/assets/fbbcf125-bcd8-45b6-a1c9-557a7da0ee08)
![task 10-7](https://github.com/user-attachments/assets/d3df0d6d-3f58-4950-8fc7-51742449c9a4)

### 11. Substitute the old netlist with the new one created after the timing ECO fix, and then execute the floorplan, placement, and clock tree synthesis (CTS).

We will now insert this updated netlist into the PnR flow by using write_verilog to overwrite the synthesis netlist. However, before doing that, we will create a copy of the old netlist.

Commands to make copy of netlist

```
# Directory to synthesis results

Desktop/
├── work/
├── tools/
├── openlane_working_dir/
├── openlane/
├── designs/  
├── picorv32a/
├── runs/
├── 14-10_09-25/
├── results/
└── synthesis

# List contents of the directory
ls

# Copy and rename the netlist
cp picorv32a.synthesis.v picorv32a.synthesis_old.v

# List contents of the directory
ls
```

Image of commands run
![task 11](https://github.com/user-attachments/assets/00cd3bb8-7586-4499-89da-3a83a761850d)
Commands to write verilog

```
# Check syntax
help write_verilog

# Overwriting current synthesis netlist
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-10_99-25/results/synthesis/picorv32a.synthesis.v

# Exit from OpenSTA since timing analysis is done
exit
```

Verified that the netlist is overwritten by checking that instance _14506_ is replaced with sky130_fd_sc_hd__or4_4
Since we have verified that the netlist has been replaced and will be loaded in PnR, we will continue with the clean design to progress to the next stages, as we want to maintain the earlier design with 0 violations.

Commands load the design and run necessary stages

```
# Now once again we have to prep design so as to update variables
prep -design picorv32a -tag 14-10_09-25 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Follwing commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts
```

Images of commands run
![task 11-2](https://github.com/user-attachments/assets/d4932540-a531-40e2-8367-2e58d0d9212b)
![task 11-3](https://github.com/user-attachments/assets/ccbdccc8-c0fc-44b6-aace-7c56580ae952)
![task 11-4](https://github.com/user-attachments/assets/5615056b-6d33-4c1d-9b08-dfa88f2db9d2)
![task 11-5](https://github.com/user-attachments/assets/7d7a7025-b066-41ad-9c8d-538a9ec93dba)
![task 11-6](https://github.com/user-attachments/assets/4ee362f8-48a8-4f21-9ba0-ed236deb985a)
![task 11-7](https://github.com/user-attachments/assets/05760813-5f17-4212-b4ec-8809a9428b3f)
### 12. Post-CTS OpenROAD timing analysis.

Commands to execute in the OpenLANE flow for performing OpenROAD timing analysis using the integrated OpenSTA tool.

```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/14-10_09-25/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/14-10_09-25/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/14-10_09-25/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Check syntax of 'report_checks' command
help report_checks

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Images of commands run and timing report generated
![task 12 last pic](https://github.com/user-attachments/assets/122ecae5-20be-4e9c-b9a2-bd1d92b95640)
![task 12 last pic-2](https://github.com/user-attachments/assets/3350c411-b5f5-4b1a-8f18-82088a379e5b)
### 13. Explore post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'.

Commands to execute in the OpenLANE flow for conducting OpenROAD timing analysis after modifying the CTS_CLK_BUFFER_LIST.

```
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Checking current value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Setting def as placement def
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/14-10_09-25/results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/14-10_09-25/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/14-10_09-25/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts1.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/14-10_09-25/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)
```

Images of commands run and timing report generated
![task 12 last pic-2](https://github.com/user-attachments/assets/ed142460-9715-4112-875c-58a670f7f784)

## Section 5 - Final steps for RTL2GDS using tritonRoute and openSTA
Tasks:
1. Generate the Power Distribution Network (PDN) and examine the PDN layout.
2. Perfrom detailed routing using TritonRoute.
3. Perform post-route parasitic extraction using the SPEF extractor.
4. Conduct post-route OpenSTA timing analysis using the extracted parasitics from the routing. 

### 1. Generate the Power Distribution Network (PDN) and examine the PDN layout.
```
# Directory to invoke the OpqnLANE flow:

Desktop/
├── work/
├── tools/
├── openlane_working_dir/
└── openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```

```
# After entering the Docker sub-system for OpenLANE, we can launch the flow in Interactive mode with the following command.
./flow.tcl -interactive

# With the OpenLANE flow now running, we need to provide the necessary packages for it to function correctly.
package require openlane 0.9

# The OpenLANE flow is now set to run any design. First, we need to prepare the 'picorv32a' design by creating the necessary files and directories.'
prep -design picorv32a

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# With the design prepared, we can now run the synthesis using the following command.
run_synthesis

# Following commands are alltogather sourced in "run_floorplan" command
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# Incase getting error
unset ::env(LIB_CTS)

# With placement done we are now ready to run CTS
run_cts

# Now that CTS is done we can do power distribution network
gen_pdn 
```

Images of power distribution network run
![day 5 task1(1)](https://github.com/user-attachments/assets/8f49296e-93c9-447b-8eb3-ee1339235b36)
![day 5 task1(2)](https://github.com/user-attachments/assets/9c54920c-ecc9-44c5-bf86-8ed4c0e71aed)
Commands to load PDN def in magic in another terminal

```
# Directory to path containing generated PDN def

Desktop/
├── work/
├── tools/
├── openlane_working_dir/
├── openlane/
├── designs/  
├── picorv32a/
├── runs/
├── 14-10_09-25/
├──tmp/
└── floorplan

# Command to load the PDN def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```
Images of PDN def
![day5 task1(3)](https://github.com/user-attachments/assets/7128c219-9f8b-4d39-9949-59940a0dd6f2)
![day 5 task 1 last](https://github.com/user-attachments/assets/047c0576-5923-4dcd-ad3c-f06d5dac56a8)

### 2. Perfrom detailed routing using TritonRoute.

```
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```

Images of routing run
![day5task2run_routing](https://github.com/user-attachments/assets/a111aa01-8735-4d76-bff6-ff1ac1abd3de)

Instructions to load the routed DEF file in Magic from a different terminal.

```
# Directory to path containing routed def

Desktop/
├── work/
├── tools/
├── openlane_working_dir/
├── openlane/
├── designs/  
├── picorv32a/
├── runs/
├── 14-10_09-25/
├── results/
└── routing

# Command to load the routed def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```

Images of routed def
![day 5 task2 magic pic](https://github.com/user-attachments/assets/f843ff16-67c2-49e5-87a5-8f0be9cbba57)
![day5 task 2 last pics](https://github.com/user-attachments/assets/f2dd35e0-d1c6-4613-9d26-b3702eb72929)
### 3. Perform post-route parasitic extraction using the SPEF extractor.

Commands for SPEF extraction using external tool

```
# Directory
Desktop/
├── work/
├── tools/
└── SPEF_EXTRACTOR

# Command extract spef
python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-09_19-22/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-09_19-22/results/routing/picorv32a.def
```
### 4. Conduct post-route OpenSTA timing analysis using the extracted parasitics from the routing. 

Commands to execute in the OpenLANE flow for performing OpenROAD timing analysis using the integrated OpenSTA within OpenROAD.

```
# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/14-10_09-25/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/14-10_09-25/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/14-10_09-25/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Link design and library
link_design picorv32a

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /openLANE_flow/designs/picorv32a/runs/14-10_09-25/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```

Images of commands run and timing report generated
![last_pic_workshop_1](https://github.com/user-attachments/assets/6c5c726b-a919-4494-b21d-e327565c500a)
![last_pic_workshop_2](https://github.com/user-attachments/assets/6229da6c-297e-4482-a84d-908cfd5f8129)

