# VLSI Physical Design for ASICs
The objective of VLSI (Very Large Scale Integration) physical design for ASICs (Application-Specific Integrated Circuits) is to transform a digital circuit's logical representation into a physical layout that meets various performance, power, area, and manufacturability requirements.
# SKILL OUTCOMES
+ Architectural Design
+ RTL Design / Behavioral Modeling
+ Floorplanning
+ placement
+ clock Tree Synthesis
+ Routing

# TABLE OF CONTENTS
## DAY 1 
**Introduction to RISCV ISA and GNU Compiler Toolchain**
+ Introduction to Basic Keywords
  - [Introduction](#introduction)
  - [From Apps to Hardware](#from-apps-to-hardware)
  - [Detail Description of Course Content](#detail-decription-of-course-content)

+ Labwork for RISCV Toolchain
  - [C Program](#c-program)
  - [RISCV GCC Compiler and Dissemble](#riscv-gcc-compiler-dissemblr)
  - [Spike Simulation and Debug](#spike-simulation-and-debug)

+ Integer Number Representation  
  - [64-bit Unsigned Numbers](#64-bit-unsigned-numbers)
  - [64-bit Signed Numbers](#64-bit-signed-numbers)
  - [Labwork For Signed and Unsigned Numbers](#labwork-for-signed-and-unsigned-numbers)

## DAY 2 
**Introduction to ABI and Basic Verification Flow**
+ Application Binary Interface
  - [Introduction to ABI](#introduction-to-abi)
  - [Memmory Allocation for Double Words](#memmory-allocation-for-double-words)
  - [Load, Add and Store Instructions](#load,-add-and-store-instructions)
  - [32-Registers and their ABI Names](#32-registers-and-their-abi-names)

+ Labwork using ABI Function Calls
  - [Algorithm for C Program using ASM](#algorithm-for-c-program-using-asm)
  - [Review ASM Function Calls](#review-asm-function-calls)
  - [Simulate C Program using Function Call](#simulate-c-program-using-function-call)
# Introduction to Basic Keywords
## Introduction
- **ISA (Instruction Set Archhitecture)**
  - ISA defines the interface between a computer's hardware and its software, specifically how the processor and its components interact with the software instructions that drive the execution of tasks.
  - It encompasses a set of instructions, addressing modes, data types, registers, memory organization, and the mechanisms for executing and managing instructions.

- **RISC-V (Reduced Instruction Set Computing - Five)**.
  - It is an open-source Instruction Set Architecture (ISA) that has gained significant attention and adoption in the world of computer architecture and semiconductor design.
  - RISC architectures simplify the instruction set by focusing on a smaller set of instructions, each of which can be executed in a single clock cycle. This approach usually leads to faster execution of individual instructions. 

<img width="536" alt="image" src="https://github.com/Veda1809/pes_asic_class/assets/142098395/4eabe0b7-4581-419b-88e7-84c7ac1dac8e">

## From Apps to Hardware
1. **Apps:** Application software, often referred to simply as "applications" or "apps," is a type of computer software that is designed to perform specific tasks or functions for end-users.
2. **System software:** System software refers to a category of computer software that acts as an intermediary between the hardware components of a computer system and the user-facing application software. It provides essential services, manages hardware resources, and enables the execution of application programs. System software plays a critical role in maintaining the overall functionality, security, and performance of a computer system.'
3. **Operating System:** The operating system is a fundamental piece of software that manages hardware resources and provides various services for both users and application programs. It controls tasks such as memory management, process scheduling, file system management, and user interface interaction. Examples of operating systems include Microsoft Windows, macOS, Linux, and Android.

4. **Compiler:** A compiler is a type of software tool that translates high-level programming code written by developers into assembly-level language.

5. **Assembler:** An assembler is a software tool that translates assembly language code into machine code or binary code that can be directly executed by a computer's processor.

6. **RTL:** RTL serves as an abstraction level in the design process that represents the behavior of a digital circuit in terms of registers and the operations that transfer data between them.

 7. **Hardware:** Hardware refers to the physical components of a computer system or any electronic device. It encompasses all the tangible parts that make up a computing or electronic device and enable it to perform various tasks.

## Detail Description of Course Content
**Pseudo Instructions:** Pseudo-instructions are used to simplify programming, improve code readability, and reduce the number of explicit instructions a programmer needs to write. They are especially useful for common programming patterns that involve multiple instructions.
`Ex: li, mv`.

**Base Integer Instructions:** The term "base integer instructions" refers to the fundamental set of instructions that form the foundation for performing basic arithmetic, logical, and data movement operations.
`Ex: add, sub, and, or, xor, sll`.

**Multiply Extension Intructions:** The RISC-V architecture includes a set of multiply and multiply-accumulate (MAC) extension instructions that enhance the instruction set to perform efficient multiplication and multiplication-accumulate operations.
`Ex: mul, mulh, mulhu, mulhsu`.

**Single and Double Precision Floating Point Extension:** The RISC-V architecture includes floating-point extensions that provide support for both single-precision (32-bit) and double-precision (64-bit) floating-point arithmetic operations. These extensions are often referred to as the "F" and "D" extensions, respectively. Floating-point arithmetic is essential for handling real numbers with fractional parts and for performing accurate calculations involving decimal values.

**Application Binary Interface:** ABI stands for "Application Binary Interface." It is a set of rules and conventions that govern how software components interact with each other at the binary level. The ABI defines various aspects of program execution, including how function calls are made, how parameters are passed and returned, how memory is allocated and managed, and more.

**Memory Allocation and Stack Pointer** 
- Memory allocation refers to the process of assigning and managing memory segments for various data structures, variables, and objects used by a program. It involves allocating memory space from the system's memory pool and releasing it when it is no longer needed to prevent memory leaks.
- The stack pointer is a register used by a program to keep track of the current position of the program's execution on the call stack. 

# Labwork for RISCV Toolchain
## C Program
We wrote a C program for calculating the sum from 1 to n using a text editor, leafpad.

`leafpad sumton.c`
``` c
#include<stdio.h>

int main(){
	int i, sum=0, n=26;
	for (i=1;i<=n; ++i) {
	sum +=i;
	}
	printf("Sum of numbers from 1 to %d is %d \n",n,sum);
	return 0;
}
```

Using the gcc compiler, we compiled the program to get the output.



![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/3b7135eb-5f0b-4508-9809-824a63fb04c5)

## RISCV GCC Compiler and Dissemble

Using the riscv gcc compiler `riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sumton.o sumton.c`, we compiled the C program.
![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/dbef876e-cc04-4757-acb4-1a08137a6452)g


To get the assembly code for the C program, `riscv64-unknown-elf-objdump -d sumton.o | less` .

In order to view the main section, type `/main`.

Here, since we used `-O1` optimisation, the number of instructions are 14.

![image](https://github.com/RohithNagesh/pes_asic_class/assets/103078929/827b74dd-879d-4720-8a4a-5ba09e28abc3)

When we use `-Ofast` optimisation, we can see that the number of instructions have been 11

![image](https://github.com/RohithNagesh/pes_asic_class/assets/103078929/f8489527-7c38-49b1-994a-dcb895a0e3c8)

## Spike Simulation and Debug

`spike pk sum1ton.o` is used to check whether the instructions produced are right to give the correct output.

![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/53c13c14-9b66-4f21-9663-42c33707422e)

`spike -d pk sum1ton.c` is used for debugging.

The contents of the registers can also be viewed.

![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/277c629a-d793-4c55-ac15-aefea9e55c2c)

# Integer Number Representation 

## Unsigned Numbers
- Unsigned numbers, also known as non-negative numbers, are numerical values that represent magnitudes without indicating direction or sign.
- Range: 0 to 2^(N) - 1.

## Signed Numbers
- Signed numbers are numerical values that can represent both positive and negative magnitudes, along with zero.
- Range : -(2^(N-1)) to 2^(N-1) - 1.
 
## Labwork
**Unsigned 64-bit Number**

``` c
#include <stdio.h>
#include <math.h>

int main(){
	unsigned long long int max = (unsigned long long int) (pow(2,64) -1);
	unsigned long long int min = (unsigned long long int) (pow(2,64) *(-1));
	printf("lowest number represented by unsigned 64-bit integer is %llu\n",min);
	printf("highest number represented by unsigned 64-bit integer is %llu\n",max);
	return 0;
}
```
![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/a9c8aa53-ac0e-4827-95ca-9703737861a0)

**Signed 64-bit Number**
``` c
#include <stdio.h>
#include <math.h>

int main(){
	long long int max = (long long int) (pow(2,63) -1);
	long long int min = (long long int) (pow(2,63) *(-1));
	printf("lowest number represented by signed 64-bit integer is %lld\n",min);
	printf("highest number represented by signed 64-bit integer is %lld\n",max);
	return 0;
}
```
![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/c1b545e8-d8b4-471f-beaf-4c653cb91fa4)

# Application Binary Interface
## Introduction to ABI
+ An Application Binary Interface (ABI) is a set of rules and conventions that dictate how binary code interacts with and communicates with other binary code, typically at the level of machine code or compiled code. In simpler terms, it defines the interface between two software components or systems that are written in different programming languages, compiled by different compilers, or running on different hardware architectures.
+ The ABI is crucial for enabling interoperability between different software components, such as different libraries, object files, or even entire programs. It allows components compiled independently and potentially on different platforms to work seamlessly together by adhering to a common set of rules for communication and data representation.
## Memmory Allocation for Double Words
64-bit number (or any multi-byte value) can be loaded into memory in little-endian or big-endian. It involves understanding the byte order and arranging the bytes accordingly
1. **Little-Endian:**
In little-endian representation, you store the least significant byte (LSB) at the lowest memory address and the most significant byte (MSB) at the highest memory address.
2. **Big-Endian:**
In big-endian representation, you store the most significant byte (MSB) at the lowest memory address and the least significant byte (LSB) at the highest memory address.
#### For example, consider the 64-bit hexadecimal value 0x0123456789ABCDEF. 
In Little-Endian representation, it would be stored as follows in memory:

![image](https://github.com/RohithNagesh/pes_asic_class/assets/103078929/307fabf6-7f58-4337-8171-6d62d99a4386)

In Big-Endian representation, it would be stored as follows in memory:

![image](https://github.com/RohithNagesh/pes_asic_class/assets/103078929/aa53e082-5878-4e3f-948a-f6f080ed0ed2)
## Load, Add and Store Instructions
Load, Add, and Store instructions are fundamental operations in computer architecture and assembly programming. They are often used to manipulate data within a computer's memory and registers.
1. **Load Instructions:**
Load instructions are used to transfer data from memory to registers. They allow you to fetch data from a specified memory address and place it into a register for further processing.

Example `ld x6, 8(x5)`

In this Example
- `ld` is the load double-word instruction.
- `x6` is the destination register.
- `8(x5)` is the memory address pointed to by register `x5` (base address + offset).
2. **Store Instructions:**
Store instructions are used to write data from registers into memory.They store values from registers into memory addresses

Example `sd x8, 8(x9)`

In this Example
- `sd` is the store double-word instruction.
- `x8` is the source register.
- `8(x9)` is the memory address pointed to by register `x9` (base address + offset).
3. Add Instructions:
  Add instructions are used to perform addition operations on registers. They add the values of two source registers and store the result in a destination register.

Example `add x9, x10, x11`

In this Example
- `add` is the add instruction.
- `x9` is the destination register.
- `x10` and `x11` are the source registers.
## 32-Registers and their ABI Names
The choice of the number of registers in a processor's architecture, such as the RISC-V RV64 architecture with its 32 general-purpose registers, involves a trade-off between various factors. While modern processors can have more registers but increasing the number of registers could lead to larger instructions, which would take up more memory and potentially slow down instruction fetch and decode.
#### ABI Names
ABI names for registers serve as a standardized way to designate the purpose and usage of specific registers within a software ecosystem. These names play a critical role in maintaining compatibility, optimizing code generation, and facilitating communication between different software components. 

![image](https://github.com/RohithNagesh/pes_asic_class/assets/103078929/b735fc44-0c08-40e8-8303-c338647dbd9f)
# Labwork using ABI Function Calls
## Algorithm for C Program using ASM
- Incorporating assembly language code into a C program can be done using inline assembly or by linking separate assembly files with your C code.
- When you call an assembly function from your C code, the C calling convention is followed, including pushing arguments onto the stack or passing them in registers as required.
- The program executes the assembly function, following the assembly instructions you've provided.
![image](https://github.com/RohithNagesh/pes_asic_class/assets/103078929/1d76b7ef-cac9-4331-9190-31af36525e0c)
## Review ASM Function Calls
- You write your C code in one file and your assembly code in a separate file.
- In the assembly file, you declare assembly functions with appropriate signatures that match the calling conventions of your platform.

**C Program**
`custom1to9.c`
  ``` c
  #include <stdio.h>
  
  extern int load(int x, int y);
  
  int main()
  {
    int result = 0;
    int count = 9;
    result = load(0x0, count+1);
    printf("Sum of numbers from 1 to 9 is %d\n", result);
  }
```
**Asseembly File**
`load.s`
``` s
.section .text
.global load
.type load, @function

load:

add a4, a0, zero
add a2, a0, a1
add a3, a0, zero

loop:

add a4, a3, a4
addi a3, a3, 1
blt a3, a2, loop
add a0, a4, zero
ret
```
## Simulate C Program using Function Call
**Compilation:** To compile C code and Asseembly file use the command `riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o custom1to9.o custom1to9.c load.s` this would generate object file `custom1to9.o`.

**Execution:** To execute the object file run the command `spike pk custom1to9.o`
![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/1074edc4-fbaa-4ff4-9b50-c445a1e9aa58)

# DAY 3


 # Labs using iverilog and gtkwave
## Introduction to lab
- Make new directory `mkdir vsd`
- `git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git`
- This should create a folder `sky130RTLDesignAndSynthesisWorkshop` in `VDS` directory
- You could see two folders under `sky130RTLDesignAndSynthesisWorkshop`
   1. my_lib: It contains all the standard cell libraries and verilog module
   2. verilog_files: It contains all the source code and testbench required for the lab

![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/528e5009-d6e7-4f55-b71e-7851b4eb6905)

 ## Introduction iverilog gtkwave
- Go to verilog_files directory
- Load Design and Testbench using the command `iverilog good_mux.v tb_good_mux.v`

![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/a27ed156-3272-4b05-a9b3-2e89c737cf17)

 - Upon loading sucessfully `a.out` will be generated
- Execute the generated file it would dump `gtkwave tb_good_mux.vcd` file
- Load the vcd file to simulator using the command `gtkwave tb_good_mux.vcd`
 

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/20a97bf8-3c1f-4eec-b668-c380501ec3e2)


# Introduction to Yosys and Logic synthesis
## Introduction to yosys
Yosys is an synthesizer which is used to convert RTL to netlist

![image](https://github.com/RohithNagesh/pes_asic_class/assets/103078929/bceda35c-e373-485b-8092-e210e37543f8)

**`netlist` is the representation of `DESIGN` in the form of standard cells present in `.lib`**

- To verify synthesis Netlist need to be fed to iverilog along with testbench
- vcd file generated from iverilog need to be fed to gtkwave simulator
- The output we get should be same as the output we got during RTL simulator

  ![image](https://github.com/benedict04/pes_asic_class/assets/109859485/8923c296-62d3-44db-bc7a-cbf6b3008b5d)

  - To verify synthesis Netlist need to be fed to iverilog along with testbench
  - vcd file generated from iverilog need to be fed to gtkwave simulator
  - The output we get should be same as the output we got during RTL simulator

    ![image](https://github.com/benedict04/pes_asic_class/assets/109859485/c3bfa8c5-502c-449a-995e-f157818124d3)

    ## Introduction to logic synthesis

 Fast and slow versions of gates are essential in digital circuit design to balance between clock frequency and timing constraints. Fast gates have shorter propagation delays and are used to reduce setup and hold time violations, allowing for higher clock frequencies. Slow gates, with longer delays, can be used to intentionally slow down critical paths or address timing issues. The Tclk formula helps calculate the maximum clock frequency while considering these factors.

# Lab using Yosys and Sky130 PDKs

+ Go to verilog_files directory
+ once you get to verilog_files directory, Invoke yosys by using the command `yosys`

![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/c904f1dd-eb42-454a-a9e4-32d067cab8c8)


  1. Read library: `read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

  ![image](https://github.com/benedict04/pes_asic_class/assets/109859485/c59301cd-5838-4876-b0e0-8671010ac3a1)

  2. Read design: `read_verilog good_mux.v`

  ![image](https://github.com/benedict04/pes_asic_class/assets/109859485/8935d05a-7850-4bbd-9986-5dd6f1c182e8)

  3. Synthesis: `synth -top good_mux`

 ![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/511fbaaa-3cac-4125-be7a-e6be391c3eac)


  4. Generate netlist: `abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`
 
 ![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/1257e10e-61d1-4a69-b3c6-5f98f5a54407)


  5.  Logic realized: `show`

 ![image](https://github.com/vivek8904/Pes_Asic_class/assets/130572147/520f992f-2354-4439-841a-33bf14b580f6)


  6. Write netlist: `write_verilog -noattr good_mux_netlist.v`
    
   ![image](https://github.com/benedict04/pes_asic_class/assets/109859485/dde17d38-5e63-48b8-919d-a7a6d09571db)

  ### good_mux_netlist.v
``` v
/* Generated by Yosys 0.32+51 (git sha1 6405bbab1, gcc 12.3.0-1ubuntu1~22.04 -fPIC -Os) */

module good_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule
```
# Introduction to timing dot libs
### sky130_fd_sc_hd__tt_025C_1v80.lib
### PVT
- Process
- Voltage
- Temperature

### Libraries
- Standard cells contains various pre-designed modules or blocks and they are chracterizeed for specific temperature and voltage conditions.
- Timing Information from libraries are useful signals propogate correctly knowing the delays in each block
- Power characteristics
- I/O pins
- characterization data helps designer study behaviour of the model under different operating conditions (voltage, temperature)

## Hierarchical Synthesis
``` v
  /* Generated by Yosys 0.32+51 (git sha1 6405bbab1, gcc 12.3.0-1ubuntu1~22.04 -fPIC -Os) */

  module multiple_modules(a, b, c, y);
    input a;
    wire a;
    input b;
    wire b;
    input c;
    wire c;
    wire net1;
    output y;
    wire y;
    sub_module1 u1 (
      .a(a),
      .b(b),
      .y(net1)
    );
    sub_module2 u2 (
      .a(net1),
      .b(c),
      .y(y)
    );
  endmodule
 
  module sub_module1(a, b, y);
    wire _0_;
    wire _1_;
    wire _2_;
    input a;
    wire a;
    input b;
    wire b;
    output y;
    wire y;
    sky130_fd_sc_hd__and2_0 _3_ (
      .A(_1_),
      .B(_0_),
      .X(_2_)
    );
    assign _1_ = b;
    assign _0_ = a;
    assign y = _2_;
  endmodule

  module sub_module2(a, b, y);
    wire _0_;
    wire _1_;
    wire _2_;
    input a;
    wire a;
    input b;
    wire b;
    output y;
    wire y;
    sky130_fd_sc_hd__or2_0 _3_ (
      .A(_1_),
      .B(_0_),
      .X(_2_)
    );
    assign _1_ = b;
    assign _0_ = a;
    assign y = _2_;
  endmodule
  ```

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/6e325e90-9fa7-4a30-9d64-4d231b132f9d)

## Flat Synthesis
``` v
  /* Generated by Yosys 0.32+51 (git sha1 6405bbab1, gcc 12.3.0-1ubuntu1~22.04 -fPIC -Os) */

  module multiple_modules(a, b, c, y);
    wire _0_;
    wire _1_;
    wire _2_;
    wire _3_;
    wire _4_;
    wire _5_;
    input a;
    wire a;
    input b;
    wire b;
    input c;
    wire c;
    wire net1;
    wire \u1.a ;
    wire \u1.b ;
    wire \u1.y ;
    wire \u2.a ;
    wire \u2.b ;
    wire \u2.y ;
    output y;
    wire y;
    sky130_fd_sc_hd__and2_0 _6_ (
      .A(_1_),
      .B(_0_),
      .X(_2_)
    );
    sky130_fd_sc_hd__or2_0 _7_ (
      .A(_4_),
      .B(_3_),
      .X(_5_)
    );
    assign _4_ = \u2.b ;
    assign _3_ = \u2.a ;
    assign \u2.y  = _5_;
    assign \u2.a  = net1;
    assign \u2.b  = c;
    assign y = \u2.y ;
    assign _1_ = \u1.b ;
    assign _0_ = \u1.a ;
    assign \u1.y  = _2_;
    assign \u1.a  = a;
    assign \u1.b  = b;
    assign net1 = \u1.y ;
  endmodule
  ```

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/a3b8c1b8-b2ef-4e88-ba5f-56c69e7bd07d)


![image](https://github.com/benedict04/pes_asic_class/assets/109859485/6834150e-dd66-4799-be3b-c4c11c10cc83)

# Various Flop Coding Styles and optimization
## Flop coding styles
**Asynchronous Reset D Flip-Flop**
When this input is asserted regardless of clock signal the flip flop output is immediately forced to predefined state.
### dff_async_set.v
``` v
module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule
```
**Synchronous Reset D Flip-Flop**
The reset input is active during specific phase of clock cycle. When the reset signal is asserted during this specific clock edge the flipflop output is forced to predefined state.
### dff_syncres.v
``` v
module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
**D Flip-Flop with Asynchronous Reset and Synchronous Reset**
- This flip-flop combines both asynchronous and synchronous reset features.
- When the asynchronous reset input is activated (set to '1'), the stored value is immediately forced to '0'.
- When the synchronous reset input is activated (set to '1') at the positive edge of the clock signal, the stored value is forced to '0'.
- Otherwise, on the positive edge of the clock signal, the stored value is updated with the data input.
### dff_asyncres_syncres.v
``` v
module dff_asyncres_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule
```
## Flop synthesis simulations 
**Asynchronous Reset D Flip-Flop**


![image](https://github.com/benedict04/pes_asic_class/assets/109859485/a36808e7-d503-4daf-bdc7-3127ec1afa22)


![image](https://github.com/benedict04/pes_asic_class/assets/109859485/56b65976-75cc-40e6-9645-57c5336b2902)

**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/7ed23de9-3bcd-4912-a9db-242e4840f8c2)


**Asynchronous set D Flip-Flop**
**Simulation**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/f0a30274-4a77-40e0-9926-234e284797e3)


![image](https://github.com/benedict04/pes_asic_class/assets/109859485/e9b072ab-5466-4659-a44e-124b89723b9a)

**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/b58276ff-9ae7-4077-986f-10ceec8935ec)

 ## Interesting optimisations 
 **mult_2.v**
``` v
module mul2 (input [2:0] a, output [3:0] y);
	assign y = a * 2;
endmodule
```
**Synthesis**


![image](https://github.com/benedict04/pes_asic_class/assets/109859485/5b1e1a58-6b46-4931-bcae-020b7d7538c3)

**mul2_netlist.v**
``` v
/* Generated by Yosys 0.32+51 (git sha1 6405bbab1, gcc 12.3.0-1ubuntu1~22.04 -fPIC -Os) */

module mul2(a, y);
  input [2:0] a;
  wire [2:0] a;
  output [3:0] y;
  wire [3:0] y;
  assign y = { a, 1'h0 };
endmodule
```
**mult_8.v**
``` v
module mult8 (input [2:0] a , output [5:0] y);
	assign y = a * 9;
endmodule
```
**Synthesis**




**mult8_netlist.v**
``` v
/* Generated by Yosys 0.32+51 (git sha1 6405bbab1, gcc 12.3.0-1ubuntu1~22.04 -fPIC -Os) */

module mult8(a, y);
  input [2:0] a;
  wire [2:0] a;
  output [5:0] y;
  wire [5:0] y;
  assign y = { a, a };
endmodule
```
# Introduction to optimizations
### Types of combinational optimizations
+ **Constant Propagation:** This technique identifies and replaces variables or expressions with their constant values, reducing redundancy in code and improving performance.

   + **Boolean Optimization:** This method simplifies boolean expressions or logic circuits by reducing the number of logical gates or terms while preserving the same logical behavior, which is useful in digital circuit design and logical reasoning.

### Types of sequential optimizations
+ **State Optimization:** Reduces the number of states in finite state machines (FSMs) by merging equivalent states, simplifying the circuit.
       + **Sequential Logic Cloning:** Replicates portions of sequential logic to alleviate bottlenecks and improve circuit throughput.
       + **Retiming:** Adjusts the placement of flip-flops within a circuit to optimize timing, balance critical paths, and enhance overall performance.

  # Combinational optimization
  **opt_check.v**
``` v
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```
**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/4ad48885-d181-4b65-a62f-fc0b3fb1566c)

**opt_check2.v**
``` v
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```
**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/44ce193b-2f7c-40bd-80be-40ee80f2c944)

**opt_check3.v**
``` v
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```
**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/3f0bd163-4f0a-4508-ba42-052f75facd2d)

**multiple_module_opt.v**
``` v
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule


module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule


module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 


endmodule
```
**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/6b693e92-56cc-42af-931a-1d2dd986052c)


# Sequential logic optimizations
**dff_const1.v**
``` v
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end

endmodule
```
**Simulate**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/eec6485c-46da-443a-b762-dfe73f1b5727)


![image](https://github.com/benedict04/pes_asic_class/assets/109859485/a2c7a534-f5f4-428c-a361-d29abcab56ca)

**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/42bd26f1-e629-49f4-9756-9461e10440ed)

**dff_const2.v**
``` v
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end

endmodule
```
**Simulate**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/183d4483-0e64-4f4a-8947-45ba4dfad07e)

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/e52639f1-2ca9-4d00-b2f4-c2c36339990b)

**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/e188fe50-ddb8-4d97-b9ff-80351e627e49)

**dff_const3.v**
``` v
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule
```

**Simulate**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/f7d87667-53cf-4282-a3cf-6b1866c69816)

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/6c85cc7d-190d-48f1-8896-3b7d49515f57)

**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/bffc33f5-2754-4580-afeb-99d6abe19cc2)

# Sequential optimzations for unused outputs
**counter_opt.v**
``` v
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```
**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/b1f6b80d-0ab5-4615-8729-3cd6ffd8b5d8)


**counter_opt2.v**
``` v
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```
**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/a26d44df-d19c-485f-8894-b08085f38b57)

# Labs on GLS and Synthesis-Simulation Mismatch

**ternary_operator_mux.v**
``` v
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
endmodule
```
**RTL Simulation**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/039f7c72-bb08-48e2-8a21-354146eda35e)

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/2ff6334e-2a6e-493c-8845-ac08e2807641)

**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/0943d01b-1263-4cb4-b83a-40d8b051269d)

**GLS**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/e2a7e70b-2f6e-49f8-b2ff-032decd5df88)


** bad_mux.v**
``` v
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```
**RTL Simulation**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/5aa90411-941c-4c7e-a6d2-0c4f6c550318)

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/100da2a8-479a-48f3-bc4a-1f668ef85ade)

**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/cb0773a4-4fee-45ac-80b6-89145b36c37d)

**GLS**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/a4363e2b-c854-4846-aecf-45903d3cfd4c)

# Labs on synth-sim mismatch for blocking statement

**blocking_caveat.v**
``` v
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule
```
**RTL Simulation**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/4aed3a57-672b-435f-8f7b-8b5c6d0e99f1)


![image](https://github.com/benedict04/pes_asic_class/assets/109859485/c88002f9-b95f-47bd-85b1-b99cd6c7b195)

**Synthesis**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/956d4fe2-9eb8-404e-872c-2ff62a702807)

**GLS**

![image](https://github.com/benedict04/pes_asic_class/assets/109859485/d9f92b5e-4487-451e-8652-cd35e5eb0edc)


![image](https://github.com/benedict04/pes_asic_class/assets/109859485/3c12227a-a34d-4801-92e1-1a77fd42d03c)


