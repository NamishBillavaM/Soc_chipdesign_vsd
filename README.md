# ADVANCED PHYSICAL DESIGN USING OPENLANE AND SKY130
## THEORY
<details>
  <summary>
Expand or Collapse
  </summary>

## HOW TO TALK TO COMPUTERS
<details>
  <summary>
Expand or Collapse
  </summary>
  
### PACKAGE

![ARDUINOLEONARDO](https://github.com/user-attachments/assets/1cc18b24-b063-49dc-801e-37b23f1a02b5)

- The **_PACKAGE_** of the chip which is a protective layer or packet bound over the actual chip and the actual manufatured chip is usually present at the center of a package.
- The connections from package is fed to the chip by WIRE BOUND method which is none other than basic wired connection.

### CHIP
![DAY11](https://github.com/user-attachments/assets/40997a27-3077-4344-be31-955efa96e0d4)


- Inside the chip, all the signals from the external world to the chip and vice versa is passed through **_PADS_**.
- The area bound by the pads is **_CORE_** where all the digital logic of the chip is placed.
- Both the core and pads make up the **_DIE_** which is the basic manufacturing unit in the semiconductor chips.

![week14](https://github.com/user-attachments/assets/22420752-f470-422d-965d-e72ec4c214a9)

### FOUNDRY
- **_Foundry_** is the place where the semiconductor chips are manufactured and FOUNDRY IP's are Intellectual Properties based on a specific foundry and these IP's require a specific level of intelligence to be produced.

### MACROS
- Digital logic blocks in the CHIP are called **_MACROS_**.

### INSTRUCTION SET ARCHITECTURE( RISC V ARCHITECTURE) :-
- It is a C program which has to be run on a specific hardware layout which is the interior of a chip in your laptop,PC or any other device there is certain flow to be followed.
- An INSTRUCTION SET ARCHITECTURE (ISA) is part of the abstract model of a computer that defines how the CPU is controlled by the software.
- HARDWARE DESCRIPTION LANGUAGE(HDL) is present as an interface between the RISC V ARCHITECTURE and the layout.
  
  ![DAY15](https://github.com/user-attachments/assets/79481bfa-b943-451a-b09f-294c844ce7e2)
#### FLOW :
- C Program to Assembly Language The C program is written in a high-level language for ease of programming. A compiler (like GCC with RISC-V backend) translates the C code into assembly language instructions adhering to the RISC-V ISA. These instructions are human-readable but hardware-specific, tailored to the RISC-V architecture.
- Assembly to Machine Language The assembly program is assembled into machine code using an assembler. Machine code consists of binary instructions (0s and 1s) that the processor can execute directly. Each assembly instruction is mapped to its binary opcode and associated data.
- Implementation of RISC-V Specification in RTL The RISC-V specification is implemented in RTL (Register Transfer Level) using a Hardware Description Language (HDL) like Verilog or VHDL. This involves describing the architecture's control logic, datapath, and how instructions are executed in terms of hardware signals.
- RTL to Layout (PnR to GDSII Flow) The RTL description is synthesized into a gate-level netlist (logic gates and their interconnections). Place and Route (PnR) tools convert the synthesized design into a physical layout. The layout is validated and finalized into a GDSII (Graphic Data System II) file, the standard format for representing integrated circuits' physical design.
- Final Output: The GDSII file is used for fabricating the chip, which will then execute the original C program's logic when powered. This flow ensures that the high-level design is accurately translated into functional hardware.
### From Software Applications to Hardware :-
![DAY16](https://github.com/user-attachments/assets/37afaced-f76a-40b3-acdf-25220ca88d40)
**Application Software:** Programs designed to perform specific tasks (e.g., word processors, games, or web browsers). Written in high-level languages like C, C++, Java, Python, etc.
**System Software:** Acts as a bridge between the application software and the hardware. Key components include: 
1.] *Operating System (OS):* Manages hardware resources and provides services to application programs. 
2.] *Compiler:* Translates high-level language programs into machine-dependent assembly or machine code.  
3.] *Assembler:* Converts assembly code into binary machine code (specific to the underlying hardware).
#### Process Flow
**STEP 1**
- The OS processes the application program and translates it into smaller functions or system calls written in high-level languages (e.g., C, C++, VB, Java).
- These functions may interact with device drivers and hardware APIs to enable communication with the physical system.
**Step 2: Compilation**
- The compiler takes high-level language outputs and converts them into assembly instructions specific to the target hardware architecture (e.g., RISC-V, ARM, x86).
- Each hardware architecture has its unique instruction set, which defines the syntax and semantics of its assembly language.
**Step 3: Assembly**
- The assembler translates the assembly instructions into machine code, which is in binary format (0s and 1s).
- Machine code is the lowest level of abstraction and directly represents instructions for the hardware.
**Step 4: Execution on Hardware**
- The machine code is loaded into the hardware (via memory or other interfaces).
- The hardware interprets the binary instructions, performing operations like arithmetic calculations, data movement, or - controlling peripherals as dictated by the binary program.
![image](https://github.com/user-attachments/assets/4a3abb62-97ec-47fd-9930-105f12cebd31)
- The output of the compiler are instructions and the output of the assembler is the binary pattern. We need some RTL (a Hardware Description Language) which understands and implements the particular instructions. This RTL is synthesised into a netlist in form of gates which is fabricated into the chip through a physical design implementation.
#### Summary Workflow
- Compiler → Produces instructions.
- Assembler → Converts instructions to binary patterns.
- RTL (HDL) → Implements the instruction set architecture.
- Synthesis → Converts RTL to a netlist of logic gates.
- PnR → Maps the netlist to physical silicon.
- Fabrication → Produces the physical chip ready to execute machine code.
  </details>
  
  ## SOC DESIGN AND OPENLANE
  <details>
  <summary>
Expand or Collapse
  </summary>
  
### Open-Source ASIC Design Implementation
**Key Enablers for Open-Source ASIC Design:**
*1.] RTL Designs:* High-level descriptions of digital logic.
*2.] EDA Tools:* Software for design, simulation, synthesis, and physical layout.
*3.]PDK Data:* Process Design Kits containing essential fabrication-related information.
### Historical Background:
- Early IC design and fabrication were tightly integrated and practiced by companies like TI and Intel.
- In 1979, Lynn Conway and Carver Mead revolutionized the field with structured design methodologies and λ-based design rules, leading to the first VLSI book, "Introduction to VLSI Systems."
- This separated design from fabrication, birthing:
  *1.) Fabless Companies: Focus on design.*
  *2.) Pure Play Fabs: Specialize in fabrication.*
### Process Design Kits (PDKs):
- PDKs act as the interface between designers and fabs, containing:
Device models, technology details, design rules, standard cell libraries, etc.
- Traditionally distributed under NDAs, making them inaccessible to the public.
- On June 30, 2020, Google and SkyWater released the first open-source PDK for the 130nm process.
  ![312922831-87384374-e66b-4ec6-b9c4-3fb92ad4d275](https://github.com/user-attachments/assets/bd51228c-f6cd-4a47-8228-573b095cdd66)

### ASIC Design Flow:
- ASIC design involves many steps and tools combined into a cohesive ASIC flow.
- Tools coordinate tasks like simulation, synthesis, placement, routing, and layout generation.
  ![312933981-1762d6d6-c5f8-4bd9-8a3d-968eb4360889](https://github.com/user-attachments/assets/05a3340c-f147-4c19-af0a-36e12f7b0ff0)

### OpenLANE Flow:
- An open-source ASIC design framework.
- Objective: Transition the design from RTL to GDSII, the final format for chip fabrication.
- Open-source initiatives like the SkyWater PDK and OpenLANE are making ASIC design more accessible, enabling innovation in academia and industry.
  ![312934312-533f58ee-4524-4a18-abb5-36b4d6a56b1f](https://github.com/user-attachments/assets/5f798657-94d9-41a4-963a-d5772579b353)

### Synthesis:
- Synthesis is the process of convertion or translation of design RTL into circuits made out of Standard Cell Libraries (SCL) the resultant circuit is described in HDL and is usually reffered to as the Gate-Level Netlist.
- Gate-Level Netlist is functionally equivalent to the RTL.
  ![image](https://github.com/user-attachments/assets/a2f8c68f-1d29-40d8-b368-80c0201da5b7)
- The fundemental building blocks which are the standard cells have regular layouts.
- Each cell has different views/models which are utilised by different EDA tools like liberty view with electrical models of the cells, HDL behavioral models, SPICE or CDL views of the cells, Layout view which include GDSII view which is the detailed view and LEF view which is the abstract view.
  ![image](https://github.com/user-attachments/assets/847c6756-320a-41ea-bac5-80443f9f2686)

### Chip Floor and Power Planning:
![image](https://github.com/user-attachments/assets/84bf40ad-3ee3-423b-98cb-d7b21b9d23dd)
- Floor and Power Planning is a critical stage in the VLSI (Very Large Scale Integration) design flow.
- It is part of the physical design process, where the synthesized design (gate-level netlist) is prepared for placement, routing, and manufacturing.
- This stage ensures that the chip's layout is organized, functional, and meets performance, area, and power requirements.
- The main goal is to define the physical structure of the chip by determining the location of different functional blocks (e.g., CPU, SRAM, I/O pads) on the silicon die and creating a robust power distribution network.
### MACROS Floor and Power Planning:
![image](https://github.com/user-attachments/assets/3cb47a32-df61-4848-a858-22214307cf64)
### Power Planning:
![image](https://github.com/user-attachments/assets/d55e29cb-8b00-4ea9-b446-3848f5b68861)
### Placement:
- Macro placement is a vital step in digital circuit design that defines the physical location of large collections of components, known as macros, on a 2-dimensional chip.
- The physical layout obtained during placement determines key performance metrics of the chip, such as power consumption, area, and performance.
![image](https://github.com/user-attachments/assets/5ad0262a-1a6d-41a3-bcce-504ac36769e6)
- Global placement provide approximate locations for all cells based on connectivity but in this stage the cells may be overlapped on each other and in detailed placement the positions obtained from global placements are minimally altered to make it legal (non-overlapping and in site-rows)
![image](https://github.com/user-attachments/assets/45a514f0-c7b9-43a0-872a-359a1bc4fc02)
### Clock Tree Synthesis:
- Clock Tree Synthesis is a technique for distributing the clock equally among all sequential parts of a VLSI design.
- The purpose of Clock Tree Synthesis is to reduce skew and delay.
- Clock Tree Synthesis is provided with the placement data as well as the clock tree limitations as input.
- Clock Tree Synthesis (CTS) is the technique of balancing the clock delay to all clock inputs by inserting buffers/inverters along the clock routes of an ASIC design.
- As a result, CTS is used to balance the skew and reduce insertion latency.
- Before Clock Tree Synthesis, all clock pins were driven by a single clock source.
- Clock tree synthesis includes both clock tree construction and clock tree balance.
![image](https://github.com/user-attachments/assets/b0184966-cd5a-4bf2-b8ce-a5868bb38f16)
- Clock skew is the time difference in arrival of clock at different components.
### Routing:
- Routing in the VLSI design course is making physical connections between signal pins using metal layers.
- Following Clock Tree Synthesis (CTS) and optimization, the routing step determines the exact pathways for interconnecting standard cells, macros, and I/O pins.
- The layout creates electrical connections using metals and vias that are determined by the logical connections in the netlist (i.e.; logical connectivity converted as physical connectivity).
![image](https://github.com/user-attachments/assets/c2341136-a12c-4d8c-ab65-2d4522f5529d)
- The skywater PDK has 6 routing layers in which the lowest layer is called the local interconnect layer which is a Titanium Nitride layer.
- The following 5 layers are all Aluminium layers.
![image](https://github.com/user-attachments/assets/765108ee-41f6-4e80-9a02-3b86bbc34885)
#### Detailed and Global Routing:
- In VLSI design and chip layout, routing is key.
- It shapes the circuit’s final form.
- The process splits into two main parts: global routing and detailed routing. Knowing these differences is vital for chip design.
- Global routing starts the process. It divides the chip into logical parts called buckets. This stage estimates the needed paths for each connection. It aims to fit all connections within the available resources.
- Detailed routing comes next. It’s about making the actual wires for the chip. This step must follow strict rules for wire width and spacing. It ensures the circuit works right.
- The two-stage method helps with complex designs. It tackles the big picture first, then the details. This way, designers manage the vast number of connections and rules.
![image](https://github.com/user-attachments/assets/1493a098-442d-4d21-9121-337ff4bc2053)
### Sign off
- In semiconductor design, “sign-off” during the tape-out (tapeout) of a chip refers to the formal approval process to ensure that the chip design is error-free, meets all specifications, and is ready for manufacturing at the foundry.
![image](https://github.com/user-attachments/assets/7508ccc7-05a0-4ab0-9a54-704f0ea0ce43)
- Once done with the routing the final layout can be generated which undergoes various Sign-Off checks.
- Design Rules Checking (DRC) which verifies that the final layout honours all design fabrication rules.
- Layout Vs Schematic (LVS) which verifies that the final layout functionality matches the gate-level netlist that we started with.
- Static Timing Analysis (STA) to verify that the design runs at the designated clock frequency.
   </details>

## GOOD FLOORPLAN VS BAD FLOORPLAN AND INTRODUCTION TO LIBRARY CELLS
 <details>
  <summary>
Expand or Collapse
  </summary>

### Utilization Factor and Aspect Ratio :-
![image](https://github.com/user-attachments/assets/559c07c6-2507-4d15-a4ea-1c6048681e82)
![image](https://github.com/user-attachments/assets/1729eb1f-30e4-4015-b15e-c5382e395649)
- **_A NETLIST DESCRIBES THE CONNECTIVITY AND FLOW OF AN ELECTRONIC DESIGN._**
- Dimensions of a chip is mostly dependant on dimensions of the logic gates.
Converting the highlighted symbols into physical dimensions:
![image](https://github.com/user-attachments/assets/7c127126-2e37-4592-8bd5-7a6058e9978e)
- A **CORE** is the section of the chip where the fundamental logic of the design is placed.
- A **DIE**, which consists of core, is a small semiconductor material specimen on which the fundamental circuit is fabricated.
![image](https://github.com/user-attachments/assets/d37d0c08-e769-44cf-b7f0-8d8dcbd1d6c1)
```math
Utilization\ Factor = \frac{Area\ Occupied\ By\ Netlist}{Total\ Area\ of\ The\ Core}
```
```math
Utilization\ Factor = \frac{4\ sq.\ units}{2\ unit\ * \ 2\ unit}
```
```math
Utilization\ Factor = 1
```
- In real life scenarios, some space is always left for future changes.
- Ideal utilizatiion perentage is 50-60% and the ideal utilization factor is 0.5-0.6
  ```math
Aspect\ Ratio = \frac{Height}{Width}
```
 ```math
Aspect\ Ratio = \frac{2\ units}{2\ units}
```
 ```math
Aspect\ Ratio = 1
```
- If the aspect ratio is **1**, then it signifies that the chip is a square.
### Concept of Pre PLaced Cells :-
![image](https://github.com/user-attachments/assets/2269130f-9f27-4b6a-821b-a10beeb4ad3f)
- The arrangement of these **IPs** in a chip ia called floor planning.
- These **IPs**/**BLOCKS** have user defined locations, and hence are placed in a chip before automated placement and routing. So they are called as **preplaced cells**.
- Automated placement and routing tools places the remaining logiacal cells in the design onto the chip.
  ![image](https://github.com/user-attachments/assets/3bcf4e44-ce01-48ef-89ad-500614cf41f7)
### Surrounding THe Preplaced Cells with Decoupling Capacitors :-
![image](https://github.com/user-attachments/assets/62a1ae66-07a0-4bcc-9312-edfcc0f1e6be)
- A decoupling capacitor is a capacitor, which is used decouple the critical cells from main power supply, in order to protect the cells from the disturbance occuring in the power distribution lines and source.
- The purpose of using decoupling capacitors is to deliver current to the gates during switching.







</details>
 </details>
 



## PRACTICAL
  <details>
  <summary>
Expand or Collapse
  </summary>
    
## GETTING FAMILIAR TO OPENSOURCE EDA TOOLS
<details>
  <summary>
Expand or Collapse
  </summary>

### RUNNING OPENLANE IN INTERACTIVE MODE:-

```bash
# Change directory to openlane directory
vsduser@vsdsquadron:~$
vsduser@vsdsquadron:~$ cd Desktop/work/tools
vsduser@vsdsquadron:~Desktop/work/tools$ cd openlane_working_dir/openlane
vsduser@vsdsquadron:~Desktop/work/tools/openlane_working_dir/openlane$
# run command docker
vsduser@vsdsquadron:~Desktop/work/tools/openlane_working_dir/openlane$ docker
bash-4.2$
#give command to run in interactive mode
bash-4.2$ ./flow.tcl -interactive
# program starts running in interactive mode
```
![image](https://github.com/user-attachments/assets/ffcd36fc-412c-42c7-b90c-3db03a4f3432)
![image](https://github.com/user-attachments/assets/d3ed419f-5416-4dbd-9fba-56b3f700f6c3)
- a new directory will open in the runs folder
![image](https://github.com/user-attachments/assets/854a0684-e24b-4a85-ae1c-6ecfd17c9748)
- Commands to run synthesis :-
```bash
% package require openlane 0.9
0.9
# Now the OpenLANE flow is ready to run any design.
# Initially we have to prep the design creating some necessary files and directories for running the 'picorv32a'
% prep -design picorv32a
# The design is prepped and ready, we can run synthesis using following command
% run_synthesis
# Synthesis starts
```
  
#### SECTION 1 TASK - CALCULATE THE FLIP FLOP RATIO :-

```math
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
##### CALCULATION OF FLOP RATIO USING DATA FROM SYNTHESIS STATISTICS REPORT :-
![image](https://github.com/user-attachments/assets/ef8f1059-6a0d-4739-8742-28ed44768dae)
![image](https://github.com/user-attachments/assets/04279ed1-15d0-40a8-af90-d4fc00af8a89)

```math
Flop\ Ratio = \frac{1613}{14876} = 0.10842968539
```
```math
PERCENTAGE\ OF\ D\ FLIP\ FLOPS' = 0.10842968539 * 100 = 10.842968539
```
</details>

## GOOD FLOORPLAN VS BAD FLOORPLAN AND INTRODUCTION TO LIBRARY CELLS
 <details>
  <summary>
Expand or Collapse
  </summary>

### Utilization Factor and Aspect Ratio :-

</details>
</details>
  
  
  
  
