# ADVANCED PHYSICAL DESIGN USING OPENLANE AND SKY130

<details>
  <summary>
Expand or Collapse
  </summary>
  
## THEORY 

<details>
  <summary>
Expand or Collapse
  </summary>

### HOW TO TALK TO COMPUTERS

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
  
### SOC DESIGN AND OPENLANE

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

### GOOD FLOORPLAN VS BAD FLOORPLAN AND INTRODUCTION TO LIBRARY CELLS

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

$$Utilization\ Factor = \frac{Area\ Occupied\ By\ Netlist}{Total\ Area\ of\ The\ Core}$$

$$Utilization\ Factor = \frac{4\ sq.\ units}{2\ unit\ * \ 2\ unit}$$

$$Utilization\ Factor = 1$$

- In real life scenarios, some space is always left for future changes.
- Ideal utilizatiion perentage is 50-60% and the ideal utilization factor is 0.5-0.6
 
$$Aspect\ Ratio = \frac{Height}{Width}$$

$$Aspect\ Ratio = \frac{4\ units}{2\ units\ * \ 2\ units}$$

$$Aspect\ Ratio = 1$$

- If the aspect ratio is **1**, then it signifies that the chip is a square.
### Concept of Pre PLaced Cells :-
![image](https://github.com/user-attachments/assets/2269130f-9f27-4b6a-821b-a10beeb4ad3f)
- The arrangement of these **IPs** in a chip ia called floor planning.
- These **IPs**/**BLOCKS** have user defined locations, and hence are placed in a chip before automated placement and routing. So they are called as **preplaced cells**.
- Automated placement and routing tools places the remaining logiacal cells in the design onto the chip.
  
  ![image](https://github.com/user-attachments/assets/3bcf4e44-ce01-48ef-89ad-500614cf41f7)
### Surrounding The Preplaced Cells with Decoupling Capacitors :-
![image](https://github.com/user-attachments/assets/62a1ae66-07a0-4bcc-9312-edfcc0f1e6be)
- A decoupling capacitor is a capacitor, which is used decouple the critical cells from main power supply, in order to protect the cells from the disturbance occuring in the power distribution lines and source.
- The purpose of using decoupling capacitors is to deliver current to the gates during switching.
### Power Planning :-
![image](https://github.com/user-attachments/assets/db829ed4-118e-4c8c-9811-2b8f41146896)
- If power is drawn from only one point, then it it might result in a **VOLTAGE DROOP** in VDD or a **VOLTAGE BUMP** in the VSS.
- The solution of this problem if to have many power supply points.
  
![image](https://github.com/user-attachments/assets/6091d8fa-71db-47cf-839d-dbfee6b0999d)
![image](https://github.com/user-attachments/assets/9755dcec-3bd6-40b5-8297-45f8853cb34d)
### Pin Placement and Logical Cell Placement Blockage :-
- The connectivity information between the gates is coded using VHDL/Verilog Language and is called as the **NETLIST**.
  
![image](https://github.com/user-attachments/assets/deb9787e-5f65-409d-9564-8e56dba87bd6)
- Avoid repetition of input or output pins
- The area between the DIE and the CORE has to be blocked so that the space is reserved for pin configuration.
  
![image](https://github.com/user-attachments/assets/86853323-4d6b-41a7-92e2-38371ddd64ea)
### Netlist Binding and Initial Place Design :-
#### Library :
- It consists of cells, shapes and size of the cells, various flavours of the same cells and timing information.
  
![image](https://github.com/user-attachments/assets/84a0ef25-63a1-437d-b90c-fc1538058e99)
![image](https://github.com/user-attachments/assets/1fa9d260-84b6-41f8-ba0a-00437ef41ff5)
#### Optimizing Placement :-
- This is the stage where we estimate wire length and capacitance, and based on that, insert repeaters.      
- **REPEATERS** are buffers that recondition the original signal, make a new signal, and sends the data forward.
##### Placement of Buffers :
![image](https://github.com/user-attachments/assets/625b2f47-7321-43b6-bcf0-578bed94903b)
### Library Charecterisation and Modelling:-
#### Logic Synthesis :-
- It is an arrangement of gates that represents the original functionality described using an RTL.
#### Floor Planning :-
- Floor planning is the most important process in physical design.
- floor planning is the process of placing blocks/MACROS in the chip or core area.
- In this step we hae netlist which describes the design and the various blocks of the design and the interconnection between the different blocks.
#### Placement :-
- Macro placement is a vital step in digital circuit design that defines the physical location of large collections of components, known as macros, on a 2-dimensional chip.
- The physical layout obtained during placement determines key performance metrics of the chip, such as power consumption, area, and performance.
#### Clock Tree Synthesis :-
- Clock Tree Synthesis is a technique for distributing the clock equally among all sequential parts of a VLSI design.
- The purpose of Clock Tree Synthesis is to reduce skew and delay.
- Clock Tree Synthesis is provided with the placement data as well as the clock tree limitations as input.
- Clock Tree Synthesis (CTS) is the technique of balancing the clock delay to all clock inputs by inserting buffers/inverters along the clock routes of an ASIC design.
- As a result, CTS is used to balance the skew and reduce insertion latency.
- Before Clock Tree Synthesis, all clock pins were driven by a single clock source.
- Clock tree synthesis includes both clock tree construction and clock tree balance.
#### Routing :-
- Routing in the VLSI design course is making physical connections between signal pins using metal layers.
- Following Clock Tree Synthesis (CTS) and optimization, the routing step determines the exact pathways for interconnecting standard cells, macros, and I/O pins.
- The layout creates electrical connections using metals and vias that are determined by the logical connections in the netlist (i.e.; logical connectivity converted as physical connectivity).

- Cell library characterization is a process of analyzing a circuit using static and dynamic methods to generate models suitable for chip implementation flows.
- Library characterization is a process of simulating a standard cell using analog simulators to extract input load, speed, and power data in a way that the downstream tools can process it all.
- This can be done via a specific analog simulator whose output is used to generate the characterization data, or by using a library characterization tool.

### Cell Design Flow :-
#### Standard Cells :-
- Standard cells are pre-designed, pre-characterized, and pre-verified functional blocks that encapsulate a specific logic function, such as AND gates, flip-flops, or latches.
- These cells adhere to a predefined height and are designed to seamlessly interconnect, allowing for the creation of intricate digital circuits.

- Standard cells are palced in libraries.
- Libraries consist of cells of different functionality, VT and sizes also.
  
![image](https://github.com/user-attachments/assets/02c31d0f-fc90-4893-9f81-1796e4063e52)
![image](https://github.com/user-attachments/assets/4da97f47-174b-477a-994f-1ef81f346925)
![image](https://github.com/user-attachments/assets/d95f3700-4d8d-489e-b9b9-221d7cdb8a31)
![image](https://github.com/user-attachments/assets/382ea206-c941-4805-8831-299223416a0f)
#### Charaterization Flow :-
- Cell library characterization is a process of analyzing a circuit using static and dynamic methods to generate models suitable for chip implementation flows.
- Library characterization is a process of simulating a standard cell using analog simulators to extract input load, speed, and power data in a way that the downstream tools can process it all.
- This can be done via a specific analog simulator whose output is used to generate the characterization data, or by using a library characterization tool.
### General Timing Characterization Parameter :-
#### Timing Threshold Definitions :
![image](https://github.com/user-attachments/assets/f0da193c-ebfe-40bf-b2ad-4aada44b5611)
- The threshold voltage, often denoted as Vth or VGS(th), represents the minimum voltage that needs to be applied to the gate of an MOSFET to establish a conductive channel between its source and drain terminals.
- This conductive channel paves the way for current flow, transforming the transistor from an insulator to a conductor.
- SLEW is defined as
   1.] The time it takes for a signal to transition from one voltage level to another.
   2.] The rate at which a signal (its voltage) transitions from one logic level to another or simply the rate of change of voltage with respect to time.
- The slew (slew rate) is also known as transition delay.
#### Propagation Delay :
- The propagation delay of a logic gate is defined as the time it takes for the effect of a change in input to be evident at the output.
- In other words, propagation delay is the time it takes for the input to reach the output.
- Propagation delay in VLSI is normally described as the time difference between when the transitional input reaches 50% of its final value and when the output reaches 50% of its final value. This demonstrates the influence of input change.
- In the above case, 50% is defined as the logic threshold at which output (or, more specifically, any signal) is presumed to flip states. It is represented by the symbol ‘tpd’. It is also known as gate delay.
  
![image](https://github.com/user-attachments/assets/d5d2ca25-7920-4077-a9cd-da9b698fa310)
![image](https://github.com/user-attachments/assets/5c6c004c-2477-436a-9dc9-5e6b3b5faed0)
#### Transition Time :
- Transition delay or slew is defined as the time taken by signal to rise from 10 %( 20%) to the 90 %( 80%) of its maximum value. This is known as “rise time”.
- Similarly “fall time” can be defined as the time taken by a signal to fall from 90 %( 80%) to the 10 %( 20%) of its maximum value.
- Transition is the time it takes for the pin to change state.
  
![image](https://github.com/user-attachments/assets/a25aeea4-4f5b-4d26-ab18-6886a40d4ed5)
![image](https://github.com/user-attachments/assets/101dec2c-5cf4-419a-8e24-29d75470c78e)
</details>

### Inception of Layout and CMOS Fabrication Process :-
 <details>
  <summary>
Expand or Collapse
  </summary>

### 16-Mask CMOS Process :-
- CMOS can be obtained by integrating both NMOS and PMOS transistors over the same silicon wafer. In N–well technology an n-type well is diffused on a p-type substrate whereas in P- well it is vice- verse.

  **1.] Selecting a Substrate :**
- First we choose a substrate as a base for fabrication. For N- well, a P-type silicon substrate is selected.
  
 ![image](https://github.com/user-attachments/assets/a314bdbe-004e-48c9-b76c-c0f46354be03)

  **2.] Creating an Active Region for Transistors :**
- Oxidation: The selective diffusion of n-type impurities is accomplished using SiO2 as a barrier which protects portions of the wafer against contamination of the substrate.
- SiO2 is laid out by oxidation process done exposing the substrate to high-quality oxygen and hydrogen in an oxidation chamber at approximately 10000c.
- Growing of Photoresist: At this stage to permit the selective etching, the SiO2 layer is subjected to the photolithography process.
- In this process, the wafer is coated with a uniform film of a photosensitive emulsion.
- Masking: This step is the continuation of the photolithography process. In this step, a desired pattern of openness is made using a stencil. This stencil is used as a mask over the photoresist.
- The substrate is now exposed to UV rays the photoresist present under the exposed regions of mask gets polymerized.
- Removal of Unexposed Photoresist: The mask is removed and the unexposed region of photoresist is dissolved by developing wafer using a chemical such as Trichloroethylene.
  
 ![image](https://github.com/user-attachments/assets/13076362-bd8d-4d9b-94ab-3be06043d0b7)
 ![image](https://github.com/user-attachments/assets/ba9b61ac-f37f-4fa4-b547-8d954e96a988)
 ![image](https://github.com/user-attachments/assets/fc6e0d47-d201-45ad-a875-95aa735ed44a)
- The process is referred to as LOCOS( Local Oxidation of Silicon ).
- Si3N4 is stripped out using hot phosphoric acid.

  **3.] N-Well and P-Well Formation :**
- Etching: The wafer is immersed in an etching solution of hydrofluoric acid, which removes the oxide from the areas through which dopants are to be diffused.
- Removal of Whole Photoresist Layer: During the etching process, those portions of SiO2 which are protected by the photoresist layer are not affected.
- The photoresist mask is now stripped off with a chemical solvent (hot H2SO4).
- Formation of N-well: The n-type impurities are diffused into the p-type substrate through the exposed region thus forming an N- well.
  
![image](https://github.com/user-attachments/assets/f02b8180-00a1-42d9-aca0-28edcfcb8948)
![image](https://github.com/user-attachments/assets/1f60ac31-6dbb-4878-8347-7e305f48c86b)
![image](https://github.com/user-attachments/assets/9a63f0ab-19e6-4ec4-b541-19ccc6cfa61d)
![image](https://github.com/user-attachments/assets/869bd3df-89ce-4452-9e08-88269bef340d)

 **4.] Formation of Gate Terminal :**
 
![image](https://github.com/user-attachments/assets/8e56c1a7-d35f-4cba-b419-265d25264ee7)
![image](https://github.com/user-attachments/assets/651c63ca-ef74-48bf-88be-f010b58292cf)
- Removal of SiO2: The layer of SiO2 is now removed by using hydrofluoric acid.
  
![image](https://github.com/user-attachments/assets/8a9450db-2725-4e43-af2b-e96c67168edd)
- Deposition of Polysilicon: The misalignment of the gate of a CMOS transistor would lead to the unwanted capacitance which could harm circuit.
- So to prevent this “Self-aligned gate process” is preferred where gate regions are formed before the formation of source and drain using ion implantation.
  
![image](https://github.com/user-attachments/assets/4eda7dac-d2ec-4780-8dfa-645e02667cf6)
![image](https://github.com/user-attachments/assets/05fda87e-f0d7-420f-882d-b8528afb5ef7)

  **5.] Lightly Doped Drain( LDD ) Formation :**
  
![image](https://github.com/user-attachments/assets/22576294-eb2a-492f-89d2-d3119caf4d7d)
![image](https://github.com/user-attachments/assets/afcdcc48-b190-41a5-90a8-1c6a416fb2d7)
![image](https://github.com/user-attachments/assets/534bcdb9-97eb-4c21-96d7-6037bb03bb6c)
- Phosphorous implant :
  
![image](https://github.com/user-attachments/assets/1613ad59-4dce-4854-aacd-7f264db536ae)
- Boron implant :
  
![image](https://github.com/user-attachments/assets/b5a4a8a4-b058-4ee7-a0cf-c573631a3bae)
- Plasma anisotopic etching :
  
![image](https://github.com/user-attachments/assets/e348f673-2363-439f-b8ad-74b34f4e6d06)

  **6.] Source and Drain Formation :**

 ![image](https://github.com/user-attachments/assets/e3308f55-f7a0-4fd6-a078-eed61ede9c10)
 ![image](https://github.com/user-attachments/assets/680a547d-b9a9-4e04-a6ad-2ae9a4c97437)
 ![image](https://github.com/user-attachments/assets/bf141534-cdfc-460f-b23b-c19adea73a13)

  **7.] Local Interconnect Formation :**
  - Deposition of Titanium :

![image](https://github.com/user-attachments/assets/e65faca1-6748-4c77-ab3e-6597512d026e)
![image](https://github.com/user-attachments/assets/29602f4d-1dd9-4e11-b7ee-74d810ecacf2)
![image](https://github.com/user-attachments/assets/be39056b-4960-4874-be2c-49f05a8a45a9)

 **8.] Higher Level Metal Formation :**

 ![image](https://github.com/user-attachments/assets/161feb26-9a55-45e1-bba9-fbb2c8ba6bd1)
 ![image](https://github.com/user-attachments/assets/bcc27594-5209-4864-8537-83e3781e3d46)
 ![image](https://github.com/user-attachments/assets/3d5d6158-f664-4b17-8a5a-9b4385e122a3)
 ![image](https://github.com/user-attachments/assets/149185c4-94df-4ed4-bfbe-dd36ac50a7bb)
 ![image](https://github.com/user-attachments/assets/fe6c7cc7-da5d-48c2-b96c-510b8901bc72)

 - Final output :
![image](https://github.com/user-attachments/assets/f7286acd-68f1-4eda-8b92-e8597334263e)

</details>

### Timing Modelling Using Delay Tables :-

 <details>
  <summary>
Expand or Collapse
  </summary>

### Power Aware Clock Tree Synthesis :-

![image](https://github.com/user-attachments/assets/1504ba66-291d-402c-9140-ef78b4b15931)
- Assuming a slew of ’40ps’ for the first buffer and capacitance of 60fF on node ‘A’, the delay of the first buffer
can be easily evaluated using below NLDM table and it comes out to be x9.
- And similarly, for the second level of buffering, the delay of the buffers ‘2’ and ‘3’ comes out to be y15
assuming a slew of ’60ps’ and capacitance of ’50fF’ at node ‘B’ and ‘C’
- This in turn results to ‘zero’ skew at clock endpoints.

![image](https://github.com/user-attachments/assets/3a322ce1-caf3-4e39-b0c9-88aa87829fb7)
- There are 3 advantages of using AND gate as buffer
    1.] you tend to use identical buffer at level 2 i.e. AND gate as buffer
    2.] you save power, by turning on/off the EN pin of AND gate 3 and disabling clock to whole bunch of
      flops connected to its output
    3.] you maintain zero skew while doing above 2.

 </details>

### Setup Timing Analysis :-

<details>
  <summary>
Expand or Collapse
  </summary>

- Setup time is the minimum amount of time the data signal should be held steady before the clock event so that the data are reliably sampled by the clock.
- Hold time is the minimum amount of time the data signal should be held steady after the clock event so that the data are reliably sampled.
- In digital designs, each and every flip-flop has some restrictions related to the data with respect to the clock in the form of windows in which data can change or not.
- There is always a region around the clock edge in which input data should not change at the input of the flip-flop. This is because, if the data changes within this window, we cannot guarantee the output.
- The output can be the result of either of the previous input, the new input or metastability.
-  Setup time is defined as the minimum amount of time before the clock's active edge that the data must be stable for it to be latched correctly. In other words, each flip-flop (or any sequential element, in general) needs some time for the data to remain stable before the clock edge arrives, such that it can reliably capture the data. This duration is known as setup time.



 </details>
 
  </details>
 

  
  
  
## GETTING FAMILIAR TO OPENSOURCE EDA TOOLS
<details>
  <summary>
Expand or Collapse
  </summary>

### RUNNING OPENLANE IN INTERACTIVE MODE:-
<details>
<summary>
Expand or Collapse
  </summary>

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
</details>

### Commands to Run Synthesis :-
<details>
<summary>
Expand or Collapse
  </summary>
  
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


$Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}$

##### CALCULATION OF FLOP RATIO USING DATA FROM SYNTHESIS STATISTICS REPORT :-

![image](https://github.com/user-attachments/assets/ef8f1059-6a0d-4739-8742-28ed44768dae)
![image](https://github.com/user-attachments/assets/04279ed1-15d0-40a8-af90-d4fc00af8a89)


$$Flop\ Ratio = \frac{1613}{14876} = 0.10842968539$$


$PERCENTAGE\ OF\ D\ FLIP\ FLOPS' = 0.10842968539 * 100 = 10.842968539$

</details>

### RUNNING FLOORPLAN IN OPENLANE 
<details>
<summary>
Expand or Collapse
  </summary>
  
### COMMANDS:-
```bash

# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane
# Run the docker command
docker
```
```bash
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan
```

![floor1](https://github.com/user-attachments/assets/af2f666b-a937-4c57-8312-e63bf48a61a7)
![floor2](https://github.com/user-attachments/assets/88d8879c-a32d-4f82-b4bf-fc6f9244582d)
- Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-01_13-06/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
### Floorplan.def in MAGIC :-

![image](https://github.com/user-attachments/assets/50ba615c-4ef8-4b05-b4b0-e449309ff887)
### Equidistant Placement of PINS :-

![image](https://github.com/user-attachments/assets/2a1881b1-c4ae-4b00-8b6b-025df2965a74)
### PIN LAYER Is As Set In config.tcl :-

![image](https://github.com/user-attachments/assets/da86b69d-0fe0-4bea-bf57-a77f30c67cba)
### DECAP Cells and TAP Cells :-

![image](https://github.com/user-attachments/assets/a17d02ba-4122-4f75-b4d0-cea2d547ba5c)
### Unplaced Standard Cells :-

![image](https://github.com/user-attachments/assets/bbda129d-10fd-4fa6-bfd4-cff764d41d05)
</details>

### RUNNING PLACEMENT IN OPENLANE
<details>
<summary>
Expand or Collapse
  </summary>
  
```bash
# After floor planning, the next step is placement. run the following command
% run_placement
# The placement process starts
```
  
#### Load Placement.DEf in Magic :-
```bash

# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/28-01_15-09/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
#### Placement.def in Magic :-

![image](https://github.com/user-attachments/assets/900661c3-ceb5-4224-b5d1-90dfbddff636)
##### Legally placed Standard Cells :-

![image](https://github.com/user-attachments/assets/ea9f1f6a-8b99-43b7-a4fb-e362f10db77f)

</details>

### VTC SPICE Simulations :-
<details>
<summary>
Expand or Collapse
  </summary>

### SPICE Deck :-
- It is the connectivity information about a netlist.
- 
![image](https://github.com/user-attachments/assets/7e91e54a-0316-4e51-8cce-7798aa5f9dfb)

#### Steps to Run VTC SPICE Simulations :-
##### *Writing a SPICE Deck*
 **1.] Defining the component connectivity**
 **2.] Defining the the component values**
 **3.] Identify the nodes**
 **4.] Name the nodes**

![image](https://github.com/user-attachments/assets/44136b52-c023-4302-ac47-89b6c71288d5)![image](https://github.com/user-attachments/assets/0e6a842c-16d2-4bfd-8ce2-0ec5d621cd97)
##### SPICE Deck :-
```bash
*** MODEL Descriptions ***
*** NETLIST Description ***
M1 out in vdd vdd pmos W=0.375u L=0.25u
M2 out in 0 0 nmos W=0.375u L=0.25u

cload out 0 10f

Vdd vdd 0 2.5
Vin in 0 2.5
*** SIMULATION Commands ***
.op
.dc Vin 0 2.5 0.05
.LIB  "tsmc_025um_model.mod" CMOS_MODELS
.end
```
### Switching Threshold :-
- The switching threshold, Vm, is defined as the point where Vin = Vout. 
- Switching threshold can be set by the ratio of relative driving strengths of the PMOS and NMOS transistors.
- To move Vm upwards, a larger value of ratio is required, which means making the PMOS wider.
- Increasing the strength of the NMOS, on the other hand, moves the switching threshold closer to GND.

- The effect of changing the Wp/Wn ratio is to shift the transient region of the VTC.
- Increasing the width of the PMOS or the NMOS moves VM towards VDD or GND respectively.
- This property can be very useful, as asymmetrical transfer characteristics are actually desirable in some designs.
 </details>

### Lab Steps to Git Clone VSDSTDCELLDESIGN :-
<details>
<summary>
Expand or Collapse
  </summary>

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```
![image](https://github.com/user-attachments/assets/ea12a4c4-810c-4567-aab4-92128ddbc1e1)
![image](https://github.com/user-attachments/assets/18f9a55a-b5ba-4b94-898d-2333f07b9deb)
![image](https://github.com/user-attachments/assets/e32c950d-ed82-42ba-abf7-62109819da35)
#### Identification of NMOS and PMOS :-

![image](https://github.com/user-attachments/assets/b6f65345-3606-4c8e-b04a-90aaba3a9620)
![image](https://github.com/user-attachments/assets/2073158a-2397-4ff3-8442-5c9999c0bbce)
#### DRC Error :-

![image](https://github.com/user-attachments/assets/345a6630-780f-447e-8f47-13ae5b29cfa3)
![image](https://github.com/user-attachments/assets/b12b69aa-4f8a-486b-826e-e22e880f2eac)
#### DRC = 0

![image](https://github.com/user-attachments/assets/294c9a8b-477b-4829-966c-ce29aeaae7d4)
</details>

### Lab Steps to Create Std Cell Layout and Extract Spice Netlist :-
<details>
<summary>
Expand or Collapse
  </summary>
  
![image](https://github.com/user-attachments/assets/f3be22ef-a273-4d72-9d36-225a8c754c48)
- Spice File Created and Opened :
  
![image](https://github.com/user-attachments/assets/5f46c1a7-2784-41a4-a558-f87162cfbe69)
- Mearsurement of grid in layout :
  
![image](https://github.com/user-attachments/assets/95c04678-14be-4761-a9d7-bd88e92b82bd)
- Changes made in Spice file :
  
![image](https://github.com/user-attachments/assets/20174a51-13e0-49cc-ae9b-cbba5983bda4)
- Running NGSPICE :
  
![image](https://github.com/user-attachments/assets/c32424e1-e5bc-4893-91f6-6acf64b4d4a0)
- Calculating the Transition Time :
$$Rise\ transition\ time = Time\ taken\ for\ output\ to\ rise\ to\ 80\% - Time\ taken\ for\ output\ to\ rise\ to\ 20\%$$
$$20\%\ of\ output = 660\ mV$$
$$80\%\ of\ output = 2.64\ V$$
- 20%
  
![image](https://github.com/user-attachments/assets/c176d61b-2f6d-4cb8-92e4-6dfc0e9b0745)
- 80%

![image](https://github.com/user-attachments/assets/95aab0c8-7853-408b-b337-05f86d57a494)
$$Fall\ transition\ time = 4.0955 - 4.0536 = 0.0419\ ns = 41.9\ ps$$
- Calculating the Cell Fall Delay :
$$Fall\ Cell\ Delay = Time\ taken\ for\ output\ to\ fall\ to\ 50\% - Time\ taken\ for\ input\ to\ rise\ to\ 50\%$$
$$50\%\ of\ 3.3\ V = 1.65\ V$$

![image](https://github.com/user-attachments/assets/97c422ba-e176-4516-8420-dafa776d7ab4)![image](https://github.com/user-attachments/assets/4413e7ee-acb3-45da-843a-d2d4add935b2)
$$Fall\ Cell\ Delay = 4.07 - 4.05 = 0.02\ ns = 20\ ps$$

</details>

### Timing Modelling Using Delay Tables :-

<details>
<summary>
Expand or Collapse
  </summary>

### Lab Steps to Convert Grid Info to Track Info :-
- Tracks.info :
  
![image](https://github.com/user-attachments/assets/a63fa156-0e5c-45fa-9d1f-d0c902975bbc)
- New Lef File :

![image](https://github.com/user-attachments/assets/57334d7e-e3fe-4080-a093-ea1d0b80de6f)
- Copy Lef File to Picorv32a Src :
```bash
# Copy lef file
cp sky130_vsdinv.lef ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# Copy lib files
cp libs/sky130_fd_sc_hd__* ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/

# List and check whether it's copied
ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

![image](https://github.com/user-attachments/assets/f8a6a054-a3f1-462d-96ca-9378187a3bd7)
- Copying the sky130_fd_sc_hd__* File to Picorv32a Src :

![image](https://github.com/user-attachments/assets/472ed297-438d-4ab1-9da7-f1a4cd69a7c9)
![image](https://github.com/user-attachments/assets/65dd43fd-e5f2-4978-b9b8-cb4e0949c873)

- Run openlane flow synthesis with newly inserted custom inverter cell :

![434](https://github.com/user-attachments/assets/693268e8-af08-4490-a632-5eaaf31706cd)
![435](https://github.com/user-attachments/assets/eee98f21-9185-4c31-b6ba-a9119c78c3e6)
![436](https://github.com/user-attachments/assets/e144ca8e-e86c-41ca-a5a7-b8ca024080af)

- Noting down current design values generated before modifying parameters to improve timing.

![image](https://github.com/user-attachments/assets/34d8a1b0-cced-41ee-8622-45c3aa80ec2f)
![image](https://github.com/user-attachments/assets/4652b28a-d3d0-4f0e-8c62-ee4babdaab55)

- Commands to view and change parameters to improve timing and run synthesis :-
```bash
# We have to prep design so as to update variables
prep -design picorv32a -tag 24-03_10-03 -overwrite

# Addiitional commands to include newly added lef to openlane flow merged.lef
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled
echo $::env(SYNTH_BUFFERING)

# Command to display current value of variable SYNTH_SIZING
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
# After synthesis, run floorplan
run_floorplan
# After floorplan, run placement
run_placement
```

![image](https://github.com/user-attachments/assets/5dd29443-e4d5-4180-a75b-9f0b3e3a7a82)
![image](https://github.com/user-attachments/assets/6ab79513-c028-4239-bf02-fad81f7b4b17)
![image](https://github.com/user-attachments/assets/038df1ba-2165-4ae9-bec0-17d717cd300c)
![image](https://github.com/user-attachments/assets/61529463-3176-41bf-acae-bb02f738294a)


- Custom inverter inserted in placement def :

![image](https://github.com/user-attachments/assets/a14ed660-051a-418f-93f2-7a2b371c62af)
</details>

### Post-Synthesis Timing Analysis With OpenSTA Tool :-
<details>
<summary>
Expand or Collapse
  </summary>

- Commands to kickstart the OpenLANE flow, include new lef and perform synthesis :
```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane
docker
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Adiitional commands to include newly added lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
![image](https://github.com/user-attachments/assets/3708afdc-19ea-448c-b301-8ba42bfbf0ed)
```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Command to invoke OpenSTA tool with script
sta pre_sta.conf
```
![image](https://github.com/user-attachments/assets/088be44b-17e7-4b2b-b6db-c09f1d7b00e2)
![image](https://github.com/user-attachments/assets/086f32db-fc56-433e-bfd3-9f6254591fa8)
![image](https://github.com/user-attachments/assets/4904f2a6-4bda-4e8b-b08b-6a9dd3ab2b2b)
![image](https://github.com/user-attachments/assets/a1ef96d8-5ce1-47e8-b60d-23b3fdf6564a)

- Here more fanout is causing more delay.








</details>
</details>






  
  
  
  
