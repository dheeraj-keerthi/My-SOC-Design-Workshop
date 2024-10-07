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
 ![VirtualBox_vsdworkshop_07_10_2024_15_13_48](https://github.com/user-attachments/assets/44a201c9-c66c-4035-afb4-fbd0212672d9)
  


