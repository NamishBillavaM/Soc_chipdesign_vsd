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

    </details>
  
  </details>
 



## PRACTICAL
  <details>
  <summary>
Expand or Collapse
  </summary>
    </details>
  
  
  
  
