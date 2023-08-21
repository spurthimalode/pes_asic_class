# VLSI Physical Design for ASICs
## OBJECTIVE
The objective of a course in VLSI (Very Large Scale Integration) Physical Design for ASICs (Application-Specific Integrated Circuits) is to provide us with the knowledge and skills needed to design and implement custom integrated circuits efficiently and effectively. ASICs are specialized integrated circuits designed for specific applications, and physical design in the context of VLSI refers to the process of translating a high-level logic or functional description of a circuit into a physical layout that can be fabricated as a semiconductor device.

<details>
<summary>SKILL OUTCOMES </summary>
+ Architectural Design
+ RTL Design / Behavioral Modeling
+ Floorplanning
+ Placement
+ Clock Tree Synthesis
+ Routing
</details>

## INSTALLATION
+ git clone https://github.com/kunalg123/riscv_workshop_collaterals
+ cd riscv_workshop_collaterals
+ chmod 755 run.sh
+ ./run.sh
  
## TABLE OF CONTENTS
<details>
<summary>DAY 1</summary>
<br>
	
[](https://github.com/spurthimalode/PES_ASIC_CLASS#links-for-easy-navigation)
*Introduction to RISCV ISA and GNU Compiler Toolchain*
+ Introduction to Basic Keywords
+ Labwork for RISCV Toolchain
+ Integer Number Representation 
<details>
<summary> Introduction </summary>
	
- *ISA (Instruction Set Archhitecture)*
  - ISA defines the interface between a computer's hardware and its software, specifically how the processor and its components interact with the software instructions that drive the execution of tasks.
  - It encompasses a set of instructions, addressing modes, data types, registers, memory organization, and the mechanisms for executing and managing instructions.

- *RISC-V (Reduced Instruction Set Computing - Five)*.
  - It is an open-source Instruction Set Architecture (ISA) that has gained significant attention and adoption in the world of computer architecture and semiconductor design.
  - RISC architectures simplify the instruction set by focusing on a smaller set of instructions, each of which can be executed in a single clock cycle. This approach usually leads to faster execution of individual instructions. 

<img width="536" alt="image" src="https://github.com/Veda1809/pes_asic_class/assets/142098395/4eabe0b7-4581-419b-88e7-84c7ac1dac8e">
    </details>
<details>
<summary>Apps to Hardware</summary>
	  
1. *Apps:* Application software, often referred to simply as "applications" or "apps," is a type of computer software that is designed to perform specific tasks or functions for end-users.
2. *System software:* System software refers to a category of computer software that acts as an intermediary between the hardware components of a computer system and the user-facing application software. It provides essential services, manages hardware resources, and enables the execution of application programs. System software plays a critical role in maintaining the overall functionality, security, and performance of a computer system.'
3. *Operating System:* The operating system is a fundamental piece of software that manages hardware resources and provides various services for both users and application programs. It controls tasks such as memory management, process scheduling, file system management, and user interface interaction. Examples of operating systems include Microsoft Windows, macOS, Linux, and Android.
4. *Compiler:* A compiler is a type of software tool that translates high-level programming code written by developers into assembly-level language.
5. *Assembler:* An assembler is a software tool that translates assembly language code into machine code or binary code that can be directly executed by a computer's processor.
6. *RTL:* RTL serves as an abstraction level in the design process that represents the behavior of a digital circuit in terms of registers and the operations that transfer data between them.
7. *Hardware:* Hardware refers to the physical components of a computer system or any electronic device. It encompasses all the tangible parts that make up a computing or electronic device and enable it to perform various tasks.
  </details>
  
## Labwork for RISCV Toolchain
### Problem Statement
Write a C program for calculating the sum from 1 to N using leafpad text editor.

c
leafpad sum1ton.c

 c
#include<stdio.h>

int main(){
	int i, sum=0, n=50;
	for (i=1;i<=n; ++i) {
	sum +=i;
	}
	printf("Sum of numbers from 1 to %d is %d \n",n,sum);
	return 0;
}

<img width="545" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/80f7e05b-1cf5-4df9-a4e1-e454c720dade">

By means of the GCC compiler, the program was processed, leading to the generation of its output.

c
gcc sumton.c
.\a.out


<img width="545" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/2a2ba71c-3925-46e1-b65d-d21198bbc6da">

## RISCV GCC Compiler and Dissemble

Using the riscv gcc compiler, we compiled the C program.

c
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c


Using `ls -ltr sum1ton.o`, we can check if the object file is created.

To get the dissembled ALP code for the C program, 

c
riscv64-unknown-elf-objdump -d sum1ton.o | less


In order to view the main section, type 
`/main`.

Using -O1 optimisation, the number of instructions are 14.

<img width="453" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/5ed25dcf-4fe6-4077-a408-15573ebd9351">


Using -Ofast optimisation, we can see that the number of instructions have been reduced to 11.

<img width="453" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/5ae7b61f-eb1c-4840-8d83-a5c26b2ba0ec">

## Spike Simulation and Debug

`spike pk sum1ton.o`serves as a means to verify that the instructions generated from the compiled `sum1ton.o` program are functioning correctly and producing the expected output

<img width="523" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/c0e4a29e-77c8-49d5-aa2e-95c6a836e35f">

For debugging use
c
spike -d pk sum1ton.c
 

The contents of the registers can be viewed.

<img width="523" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/83fce150-9e10-4362-8921-42c293cf40bd">

- press ENTER : to show the first line and successive ENTER to show successive lines
- reg 0 a2 : to check content of register a2 0th core
- q : to quit the debug process

## Integer Number Representation 

### Unsigned Numbers
- Unsigned numbers, also known as non-negative numbers, are numerical values that represent magnitudes without indicating direction or sign.
- Range: [0, (2^n)-1 ]

### Signed Numbers
- Signed numbers are numerical values that can represent both positive and negative magnitudes, along with zero.
- Range : Positive : [0 , 2^(n-1)-1]
          Negative : [-1 to 2^(n-1)]

### Problem Statement

Write a C program that shows the maximum and minimum values of 64bit unsigned numbers.

 c
#include <stdio.h>
#include <math.h>

int main(){
	unsigned long long int max = (unsigned long long int) (pow(2,64) -1);
	unsigned long long int min = (unsigned long long int) (pow(2,64) *(-1));
	printf("lowest number represented by unsigned 64-bit integer is %llu\n",min);
	printf("highest number represented by unsigned 64-bit integer is %llu\n",max);
	return 0;
}

<img width="531" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/1e08c5b2-0346-4013-bcae-1c0306c2884d">

Write a C program that shows the maximum and minimum values of 64bit signed numbers.
 c
#include <stdio.h>
#include <math.h>

int main(){
	long long int max = (long long int) (pow(2,63) -1);
	long long int min = (long long int) (pow(2,63) *(-1));
	printf("lowest number represented by signed 64-bit integer is %lld\n",min);
	printf("highest number represented by signed 64-bit integer is %lld\n",max);
	return 0;
}

<img width="531" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/a3ed2f0a-ae3c-459b-838f-d9d9e644ba4f">


</details>
<details>
<summary>DAY 2</summary>
<br>

[](https://github.com/spurthimalode/PES_ASIC_CLASS#links-for-easy-navigation)
*Introduction to ABI and Basic Verification Flow*
+ Application Binary Interface
  
  <img width="553" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/4411b4fa-c502-43c9-8de9-46c71199f34b">

 An Application Binary Interface (ABI) is a set of rules and conventions that dictate how binary code interacts with and communicates with other binary code, typically at the level of machine code or compiled code. In simpler terms, it defines the interface between two software components or systems that are written in different programming languages, compiled by different compilers, or running on different hardware architectures.
+ The ABI is crucial for enabling interoperability between different software components, such as different libraries, object files, or even entire programs. It allows components compiled independently and potentially on different platforms to work seamlessly together by adhering to a common set of rules for communication and data representation.
### Memory Allocation for Double Words
64-bit number (or any multi-byte value) can be loaded into memory in little-endian or big-endian. It involves understanding the byte order and arranging the bytes accordingly
1. *Little-Endian:*
In little-endian representation, you store the least significant byte (LSB) at the lowest memory address and the most significant byte (MSB) at the highest memory address.
2. *Big-Endian:*
In big-endian representation, you store the most significant byte (MSB) at the lowest memory address and the least significant byte (LSB) at the highest memory address.
#### For example, consider the 64-bit hexadecimal value 0x0123456789ABCDEF. 
In Little-Endian representation, it would be stored as follows in memory:

<img width="353" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/b64ebad9-e52a-4bf5-8df9-86c0d112ebe3">


In Big-Endian representation, it would be stored as follows in memory:

<img width="353" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/5c1b4ced-8989-4557-b35a-cba5c4523dcc">

## Load, Add and Store Instructions
Load, Add, and Store instructions are fundamental operations in computer architecture and assembly programming. They are often used to manipulate data within a computer's memory and registers.
1. *Load Instructions:*
Load instructions are used to transfer data from memory to registers. They allow you to fetch data from a specified memory address and place it into a register for further processing.

Example `ld x6, 8(x5)`

In this Example
- `ld` is the load double-word instruction.
- `x6` is the destination register.
- `8(x5)` is the memory address pointed to by register `x5` (base address + offset).
2. *Store Instructions:*
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
### 32-Registers and their ABI Names
The choice of the number of registers in a processor's architecture, such as the RISC-V RV64 architecture with its 32 general-purpose registers, involves a trade-off between various factors. While modern processors can have more registers but increasing the number of registers could lead to larger instructions, which would take up more memory and potentially slow down instruction fetch and decode.
#### ABI Names
ABI names for registers serve as a standardized way to designate the purpose and usage of specific registers within a software ecosystem. These names play a critical role in maintaining compatibility, optimizing code generation, and facilitating communication between different software components. 

<img width="430" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/3e299a64-2dea-455d-bb98-d96c36f97af8">


## Labwork using ABI Function Calls
### Algorithm for C Program using ASM
- Incorporating assembly language code into a C program can be done using inline assembly or by linking separate assembly files with your C code.
- When you call an assembly function from your C code, the C calling convention is followed, including pushing arguments onto the stack or passing them in registers as required.
- The program executes the assembly function, following the assembly instructions you've provided.

### Review ASM Function Calls
- We wrote C code in one file and your assembly code in a separate file.
- In the assembly file, we declared assembly functions with appropriate signatures that match the calling conventions of your platform.

*C Program*
`custom1to9.c`
   c
  #include <stdio.h>
  
  extern int load(int x, int y);
  
  int main()
  {
    int result = 0;
    int count = 9;
    result = load(0x0, count+1);
    printf("Sum of numbers from 1 to 9 is %d\n", result);
  }

*Assembly File*
`load.s`
 s
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

### Simulate C Program using Function Call
*Compilation:* To compile C code and Assembly file use the command

c
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o custom1to9.o custom1to9.c load.s


this would generate object file `custom1to9.o`.

*Execution:* To execute the object file run the command 

c
spike pk custom1to9.o


<img width="517" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/bb810c8c-caf3-416a-a709-f6d023a91d99">

</details>


## DETAIL DESCRIPTION OF COURSE CONTENT
*Pseudo Instructions:* Pseudo-instructions are used to simplify programming, improve code readability, and reduce the number of explicit instructions a programmer needs to write. They are especially useful for common programming patterns that involve multiple instructions.
`Ex: li, mv`.

*Base Integer Instructions:* The term "base integer instructions" refers to the fundamental set of instructions that form the foundation for performing basic arithmetic, logical, and data movement operations.
`Ex: add, sub, and, or, xor, sll`.

*Multiply Extension Intructions:* The RISC-V architecture includes a set of multiply and multiply-accumulate (MAC) extension instructions that enhance the instruction set to perform efficient multiplication and multiplication-accumulate operations.
`Ex: mul, mulh, mulhu, mulhsu`.

*Single and Double Precision Floating Point Extension:* The RISC-V architecture includes floating-point extensions that provide support for both single-precision (32-bit) and double-precision (64-bit) floating-point arithmetic operations. These extensions are often referred to as the "F" and "D" extensions, respectively. Floating-point arithmetic is essential for handling real numbers with fractional parts and for performing accurate calculations involving decimal values.

*Application Binary Interface:* ABI stands for "Application Binary Interface." It is a set of rules and conventions that govern how software components interact with each other at the binary level. The ABI defines various aspects of program execution, including how function calls are made, how parameters are passed and returned, how memory is allocated and managed, and more.

*Memory Allocation and Stack Pointer* 
- Memory allocation refers to the process of assigning and managing memory segments for various data structures, variables, and objects used by a program. It involves allocating memory space from the system's memory pool and releasing it when it is no longer needed to prevent memory leaks.
- The stack pointer is a register used by a program to keep track of the current position of the program's execution on the call stack.
