# VLSI Physical Design for ASICs
## OBJECTIVE
The objective of a course in VLSI (Very Large Scale Integration) Physical Design for ASICs (Application-Specific Integrated Circuits) is to provide us with the knowledge and skills needed to design and implement custom integrated circuits efficiently and effectively. ASICs are specialized integrated circuits designed for specific applications, and physical design in the context of VLSI refers to the process of translating a high-level logic or functional description of a circuit into a physical layout that can be fabricated as a semiconductor device.
<img width="500" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/f2d490a5-7763-4946-a1a1-906e59c7c87c">

  
## TABLE OF CONTENTS
<details>
<summary>Introduction to RISCV ISA and GNU Compiler Toolchain</summary>
<br>
	
[](https://github.com/spurthimalode/PES_ASIC_CLASS#links-for-easy-navigation)
<details>
<summary>Installation</summary>
	
+ git clone https://github.com/kunalg123/riscv_workshop_collaterals
+ cd riscv_workshop_collaterals
+ chmod 755 run.sh
+ ./run.sh
</details>
<details>
<summary> Introduction </summary>
	<img width="500" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/b93f1302-4b14-47af-bf81-76761782a4c9"><br>

- **ISA (Instruction Set Architecture)**
  - ISA defines the interface between a computer's hardware and its software, specifically how the processor and its components interact with the software instructions that drive the execution of tasks.
  - It encompasses a set of instructions, addressing modes, data types, registers, memory organization, and the mechanisms for executing and managing instructions.

- **RISC-V (Reduced Instruction Set Computing - Five)**.
  - It is an open-source Instruction Set Architecture (ISA) that has gained significant attention and adoption in the world of computer architecture and semiconductor design.
  - RISC architectures simplify the instruction set by focusing on a smaller set of instructions, each of which can be executed in a single clock cycle. This approach usually leads to faster execution of individual instructions. 

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
```
leafpad sum1ton.c
```
``` 
#include<stdio.h>

int main(){
	int i, sum=0, n=50;
	for (i=1;i<=n; ++i) {
	sum +=i;
	}
	printf("Sum of numbers from 1 to %d is %d \n",n,sum);
	return 0;
}
```
<img width="545" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/ab16be32-477c-4a39-b5e8-0a721e415f05">

By means of the GCC compiler, the program was processed, leading to the generation of its output.

```
gcc sumton.c
.\a.out
```

<img width="545" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/0ec1c331-563e-41ec-9ffa-01bef5084ab8">

## RISCV GCC Compiler and Dissemble

Using the riscv gcc compiler, we compiled the C program.
```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```

Using `ls -ltr sum1ton.o`, we can check if the object file is created.

To get the dissembled ALP code for the C program, 
```
riscv64-unknown-elf-objdump -d sum1ton.o | less
```

In order to view the main section, type 
`/main`.

Using -O1 optimisation, the number of instructions are 14.

<img width="453" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/b1ffdd31-93cb-4dfc-80e4-6a63056643c2">


Using -Ofast optimisation, we can see that the number of instructions have been reduced to 11.

<img width="453" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/43d95b26-1d17-45f3-b0dc-53ee4e099eb7">

## Spike Simulation and Debug

`spike pk sum1ton.o`serves as a means to verify that the instructions generated from the compiled `sum1ton.o` program are functioning correctly and producing the expected output

<img width="523" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/b1260696-31a0-47d3-9b28-9f809dc19be8">

For debugging use
```
spike -d pk sum1ton.c
```

The contents of the registers can be viewed.

<img width="523" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/24b1c88a-5709-4640-8e9f-f706b077bc8b">

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
```
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
<img width="531" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/eaf0f457-a8f0-4afe-80f1-dfc44f798884">

Write a C program that shows the maximum and minimum values of 64bit signed numbers.
 ```
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
<img width="531" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/aa0fe64b-43a1-4977-b76d-9ca59cf78f67">


</details>
<details>
<summary>Introduction to ABI and Basic Verification Flow</summary>
<br>

[](https://github.com/spurthimalode/PES_ASIC_CLASS#links-for-easy-navigation)
**Introduction to ABI and Basic Verification Flow**
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

<img width="240" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/b64ebad9-e52a-4bf5-8df9-86c0d112ebe3">


In Big-Endian representation, it would be stored as follows in memory:

<img width="240" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/5c1b4ced-8989-4557-b35a-cba5c4523dcc">

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

<img width="330" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/3e299a64-2dea-455d-bb98-d96c36f97af8">


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
   ```
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
*Assembly File*
`load.s`
 ```
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
### Simulate C Program using Function Call
*Compilation:* To compile C code and Assembly file use the command

```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o custom1to9.o custom1to9.c load.s
```

this would generate object file `custom1to9.o`.

*Execution:* To execute the object file run the command 

```
spike pk custom1to9.o
```

<img width="600" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/c7b675b7-1d34-4a90-8a36-5cf0983fff40">



## Lab to Run C-Program on RISCV-CPU

```
git clone https://github.com/kunalg123/riscv_workshop_collaterals.git
```

```
cd riscv_workshop_collaterals
```
<img width="517" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/31bb6262-7c4b-4599-a726-798a1123ff0f">

```
ls -ltr
```

```
cd labs
```

```
ls -ltr
```
<img width="517" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/e064ce16-1c8d-4814-bdb7-50edac41c3fb">

```
chmod 777 rv32im.sh
```

```
./rv32im.sh
```

<img width="517" alt="image" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/9ea54069-9163-4451-8f6c-cfa0bd77f261">
</details>

<details>
<summary>RTL Design Using Verilog With SKY130 Technology</summary>
<br>

<details>
<summary>DAY 0-- Installation</summary>
<br>
	
**Yosys**

I installed Yosys using the following commands:
     
```
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys-master 
$ sudo apt install make 
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make 
$ sudo make install
```
     
Below is the screenshot showing sucessful launch:

<img width="550" alt="yosys" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/6f0f5c60-cf08-4687-ab29-b7f165d29978">


**Iverilog**

I installed iverilog using the following command:

```
sudo apt-get install iverilog
```

Below is the screenshot showing sucessful launch:

<img width="550" alt="iverilog" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/f67dc762-b04f-4a5b-9c3d-b5494ceb27b8">

 **Gtkwave**

 I installed gtkwave using the following command:

```
sudo apt-get install gtkwave
```

Below is the screenshot showing sucessful launch:

<img width="550" alt="gtkwave" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/2a5bc1c5-e2a7-41c2-97e3-8977fae03792">

**Ngspice**

I downloaded the tarball from https://sourceforge.net/projects/ngspice/files/ to a local directory and unpacked it using the following commands:

```
tar -zxvf ngspice-41.tar.gz
cd ngspice-41
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
make
sudo make install

```
Below is the screenshot showing sucessful launch:

<img width="550" alt="ngspice" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/38d33e5c-570c-4487-bbaa-a8697e79d026">


**Magic**

I installed magic using the following commands:

```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
sudo apt install magic
```
Below is the screenshot showing sucessful launch:

<img width="550" alt="magic" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/02a7c239-a654-4cc2-850e-60ba9bf61fca">

**OpenSTA**

I installed and built OpenSTA (including the needed packages) using the following commands:

```
sudo apt-get install cmake clang gcctcl swig bison flex
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git
cd OpenSTA
mkdir build
cd build
cmake ..
make

```
Below is the screenshot showing sucessful launch:

<img width="1085" alt="opensta" src="---">

**OpenLANE**

I installed and built OpenLANE (including the needed packages) using the following commands:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io

sudo docker run hello-world

sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 

# After reboot
docker run hello-world

```
Below is the screenshot showing sucessful launch:

<img width="1058" alt="openlane" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/bc506b38-f849-42dc-b97e-88b2be3eb648">
</details>

<details>
<summary>DAY 1--Introduction to Verilog RTL design and Synthesis</summary>
	
**RTL Design**: In simple terms RTL design or Register Transfer Level design is a method in which we can transfer data from one register to another. In RTL design we write code for Combinational and Sequential circuits in HDL(Hardware Description Language) like Verilog or VerilogHDL which can model logical and hardware operation. RTL design can be one code or set of verilog codes. **One key note is that we need to write RTL design with optimized and synthesizable (realizable as physical gates)**.

**Sample RTL design outline:**

	module module_name (port list);
		//declarations;
		//initializations;
		//continuos concurrent assigments;
		//procedural blocks;
	endmodule

**Test Bench**: Using Verilog we can write a test bench to apply stimulus to the RTL design and verify the results of the design by instantiating design with in test bench. Up-front verification becomes very important as design size increases in size and complexity while any project progresses. This ensures simulation results matches with post synthesis results. A test bench can have two parts, the one generates input signals for the model to be tested while the other part checks the output signals from the design under test. It can be represented as follows.

<img width="1058" src="https://user-images.githubusercontent.com/104454253/166088950-634be5a4-7d5a-4b43-9990-711f8f660aaf.JPG">

**Simulation**: RTL design is checked for adherence to its design specification using simulation by giving sample inputs. This helps finding and fixing bugs in the RTL design in the early stages of design development. 

**Simulator**: Simulator is the tool used for this process. It looks for changes on input signals to evaluate outputs. No change in output if there is no change in input signals
Here is the flow of frondend design:

<img width="1058" src="https://user-images.githubusercontent.com/104454253/166088866-80a4e792-7db7-4bf2-b3b5-b4b9b92452a8.JPG">

<details>
 <summary> Introduction to open source simulator iverilog and gtkwave </summary>
	
**iverilog**: iverilog stands for Icarus Verilog. Icarus Verilog is an implementation of the Verilog hardware description language. It supports the 1995, 2001 and 2005 versions of the standard, portions of SystemVerilog, and some extensions.

**Gtkwave**: GTKWave is a fully featured GTK+ based wave viewer for Unix, Win32, and Mac OSX which reads LXT, LXT2, VZT, FST, and GHW files as well as standard Verilog VCD/EVCD files and allows their viewing. 

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/dc9562f3-f04e-464c-972f-b87026a86205)

To load the '.vcd' file 
```
gtkwave tb_good_mux.vcd
```
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/59553a01-e01e-46ec-998f-fa7d565030e1)

In this session, I've performed simulation of multiplexer. I've added both the RTL design code and test bench code in iverilog to generate vcd file which I used in gtkwave generator to get the output waveformes after simulation. The output was generated by taking the inputs from the testbench code. 


Here is the code :<br />
```
	module good_mux (input i0 , input i1 , input sel , output reg y); 
		always @ (*)
		begin
			if(sel)
			y <= i1;
			else 
			y <= i0;
		end
	endmodule


	`timescale 1ns / 1ps
	module tb_good_mux;
	// Inputs
	reg i0,i1,sel;
	// Outputs
	wire y;
      		// Instantiate the Unit Under Test (UUT), name based instantiation
		good_mux uut (.sel(sel),.i0(i0),.i1(i1),.y(y));
		//good_mux uut (sel,i0,i1,y);  //order based instantiation
	initial begin
		$dumpfile("tb_good_mux.vcd");
		$dumpvars(0,tb_good_mux);
		// Initialize Inputs
		sel = 0;
		i0 = 0;
		i1 = 0;
		#300 $finish;
	end
	always #75 sel = ~sel;
	always #10 i0 = ~i0;
	always #55 i1 = ~i1;
	endmodule
```

</details>
<details>
 <summary> Introduction to Yosys synthesizer </summary>

**Synthesis**: Synthesis transforms the simple RTL design into a gate-level netlist with all the constraints as specified by the designer. In simple language, Synthesis is a process that converts the abstract form of design to a properly implemented chip in terms of logic gates.

Synthesis takes place in multiple steps:
- Converting RTL into simple logic gates.
- Mapping those gates to actual technology-dependent logic gates available in the technology libraries.
- Optimizing the mapped netlist keeping the constraints set by the designer intact.

**Synthesizer**: It is a tool we use to convert out RTL design code to netlist. Yosys is the tool I've used in this workshop.
Here is the flow of above processess.

<img wigth="250" length="250" src="https://user-images.githubusercontent.com/104454253/166097298-41d913ee-640d-4e1e-9e70-5bf427f35ef4.JPG">

**Yosys**:Yosys is a framework for RTL synthesis and more. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains. Yosys is the core component of most our implementation and verification flows.

I was given an overview of the operation of the tool and the files we'll need to provide the tool to give the required netlist. We give RTL design code, .lib file which has all the building blocks of the netlist. Using these two files, Yosys synthesizer generates a netlist file. .lib basically is a collection of logical modules like, And, Or, Not etc.... These are equivalent gate level representation of the RTL code. 

Below are the commands to perform above synthesis.

- RTL Design  - read_verilog
- .lib        - read_liberty
- netlist file- write_verilog

**Operational flow of Yosys Synthesizer**

<img width="550" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/fbd43bc0-318e-417c-aa76-eda2046fa632">


**Verification of Synthesized design**: In order to make sure that there are no errors in the netlist, we'll have to verify the synthesized circuit. The netlist verification flow can be seen in the below image:

<img width="550" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/6a531a7a-9d4a-431c-88ef-6f9fe713fbe6">

The gtkwave output for the netlist should match the output waveform for the RTL design file. As netlist and design code have same set of inputs and outputs, we can use the same testbench and compare the waveforms.

**Introduction to logic synthesis**: Below is the snippet RTL code and equivalent digital circuit:

<img width="550" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/3b9796d9-4b34-470b-a16a-c87cd1ebb250">

In the above image, mapping of code and digital circuit is done using Synthesis.

**.lib**: It is a collection of logical modules like, And, Or, Not etc...It has different flvors of same gate like 2 input AND gate, 3 input AND gate etc... with different performace speed.

**Need for different flavours of gate**: In order to make a faster circuit, the clock frequency should be high. For that the time period of the clock should be as low as possible. However, in a sequential circuit, clock period depends on three factors so that data is not lost or to be glitch free.

For the below circuit the three factors are
- Clock to Q of flipflop A
- Propagation delay of combinational circuit
- Setuptime of flipflop B
- 
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/e51b53cd-7791-4069-94b5-ebc7ced078de)

The equation is as follows

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/5d358426-01fc-492e-8e3c-f4a0705cb9e4)


As per the above equation, for a smaller propagation delay, we need faster cells.
But again, why do we have faster cells? This is to ensure that there are no HOLD time violations at B flipflop.
**This complete collection forms .lib**

**Faster Cells vs Slower Cells**: 
Load in digital circuit is of **Capacitence**. Faster the charging or dicharging of capacitance, lesser is the celll delay. However, for a quick charge/ discharge of capacitor, we need transistors capable of sourcing more current i.e, we need WIDE TRANSISTORS. 

Wider transistors have lesser delay but consume more area and power. Narrow transistors are other way around. Faster cells come with a cost of area and power.

**Selection of the Cells**: We'll need to guide the Synthesizer to choose the flavour of cells that is optimum for implementation of logic circuit. Keeping in view of previous observations of faster vs slower cells,to avoid hold time violations, larger circuits, sluggish circuits, we offer guidance to synthesizer in the form of **Constraints**.

Below is an illustration of Synthesis.

<img width="550" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/088d8f2b-1712-486d-83d4-40b1bfeb7c3b">

### Labs on Yosys introduction

Create a directory named VLSI and git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

<img width="1058" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/5a3b0770-97b4-43d3-ac1f-5f66d61acbf8">

Invoking yosys

<img width="1058" src="https://github.com/spurthimalode/pes_asic_class/assets/142222859/54df98f0-9a21-4c66-a06c-c3713368b88b">

Snippet below illustrates reading .lib, design and choosing the module to synthesize:
```
read_liberty -lib ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux.v
```
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/38610267-fdd2-49cd-affb-ae8c6e16a97e)

To generate the netlist:
 ```
abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/058e2024-7dcb-43f6-936f-bd23ea7b4fa7)

Below is the snippet showing the synthesis results and synthesized circuit for multiplexer.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/4428cbf9-4148-4ac3-a5b2-2f2e13d8db21)

To write the netlist
```
write_verilog good_mux_netlist.v
!gvim good_mux_netlist.v
```
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/2208f39f-7914-4428-9fc6-96f31cdb1c83)


To write a simplified netlist
```
write_verilog -noattr good_mux_netlist.v
!gvim good_mux_netlist.v
```
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/b90c10a7-9fa0-4993-a03f-03fb9f1ce67b)

</details>
</details>

<details>
<summary>Day 2 --Timing libs, hierarchical, flat synthesis, efficient flop coding styles</summary>
<details>
<summary>Introduction to timing .libs</summary>

### LAB- Introduction to dot Lib
This lab guides us through the .lib files where we have all the gates coded in. According to the below parameters the libraries will be characterized to model the variations.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/eb0fc533-1a6f-4ebf-b904-0686cf8f8892)

With in the lib file, the gates are declared as follows to meet the variations due to process, temperatures and voltages.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/325bfcd9-cda3-43c6-9350-402a682dfb4e)

For the above example, for all the 32 cominations i.e 2^5 (5 is no.of variables), the delay, power and all the related parameters for each gate are mentioned.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/efb45b5b-493f-4396-958a-0235eef36cdf)


This image displays the power consumtion comparision.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/ce9e3443-76d1-411e-bc34-e4b20c0324dc)

Below image is the delay order for the different flavor of gates.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/b1d09682-fa52-477e-8781-48f4974981b7)


 </details>

  <details>
<summary> LAB- Hierarchical synthesis and flat synthesis </summary>

**multiple_module**<br />

	module sub_module2 (input a, input b, output y);
		assign y = a | b;
	endmodule
	
	module sub_module1 (input a, input b, output y);
		assign y = a&b;
	endmodule


	module multiple_modules (input a, input b, input c , output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
	endmodule

This is the schematic as per the connections in the above module.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/809949fb-654e-4aa8-9e2c-865a96600951)

Run the below mentioned commands:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog multiple_modules.v
yosys> synth -top multiple_modules
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show multiple_modules 

```
Below is the snippet showing the synthesis results and synthesized hierarchical circuit for multiple_module.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/eae68e2a-9a22-42c0-bbd6-190d247a057b)

Below is the snippet showing  the hierarchical netlist code for the  multiple_modules:
```
write_verilog -noattr multiple_modules_hier.v
!vim multiple_modules_hier.v
```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/f3597b73-8932-4b31-a705-d5bf6c231f27)

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/92383f5a-9f44-4160-a5fa-9e927aeb592a)

Flattened netlist:

In flattened netlist, the hierarcies are flattend out and there is single module i.e, gates are instantiated directly instead of sub_modules. 
```
write_verilog -noattr multiple_modules_fast.v
!vim multiple_modules_fast.v
```
Below is the snippet showing the synthesis results and synthesized flatten circuit for multiple_module.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/97d9f407-0695-4c4c-bfde-2447857a78ae)

Below is the snippet showing  the flatten netlist code for the  multiple_modules:

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/bbe806c5-7cdb-430a-bd05-2f024d7bb5eb)

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/0937733e-2ba5-4af6-8f7f-c15e553df818)

**submodule synthesis**

Run the below commands:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog multiple_modules.v
yosys> synth -top sub_module1
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```
Below is the snippet showing the synthesis results and synthesized  circuit for sub_module1.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/d8fe8988-86b2-475f-8405-f6a628c0d017)

</details>
<details>
	<summary>Various Flop coding styles and optimization</summary>
	
**Why Flops and Flop coding styles**

In this session, the discussion was about how to code various types of flops and various styles of coding a flop.

**Why a Flop?**

 In a combinational circuit, the output changes after the propagation delay of the circuit once inputs are changed. During the propagation of data, if there are different paths with different propagation delays, there might be a chance of getting a glitch at the output.<br />
 If there are multiple combinational circuits in the design, the occurances of glitches are more thereby making the output unstable.<br />
To curb this drawback, we are going for flops to store the data from the cominational circuits. When a flop is used, the output of combinational circuit is stored in it and is propagated only at the posedge or negedge of the clock so that the next combinational circuit gets a glitch free input thereby stabilising the output.
 
 We use initialize signals or control pins called **set** and **reset** on a flop to initialize the flop, other wise a garbage value to sent out to the next combinational circuit. These control pins can be synchronous or asynchronous.

### Lab- flop synthesis simulations


**d-flipflop with asynchronous reset**- Here the output **q** goes low whenever reset is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to low.

	 module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
		always @ (posedge clk , posedge async_reset)
		begin
			if(async_reset)
				q <= 1'b0;
			else	
				q <= d;
		end
	endmodule

**Simulation**:
Code for Simulation:
```
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/1c23836e-b521-4573-a3ad-b2006fd3d28a)

**Synthesized circuit**:
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_asyncres.v
yosys> synth -top dff_asyncres
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/a1ca389d-937d-4ee1-8d8d-eacc33a16f5e)


**d-flipflop with asynchronous set**- Here the output **q** goes high whenever set is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to high.
 

	module dff_async_set ( input clk ,  input async_set , input d , output reg q );
		always @ (posedge clk , posedge async_set)
		begin
			if(async_set)
				q <= 1'b1;
			else
				q <= d;
		end
	endmodule

**Simulation**:
Code for Simulation:
```
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/cd60741a-de48-464e-97d7-77f23ad6c3e5)

**Synthesized circuit**:
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_async_set.v
yosys> synth -top dff_async_set
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/ff454afc-a315-4af6-8a09-1acffae38b60)


**d-flipflop with synchronous reset**- Here the output **q** goes low whenever reset is high and at the positive edge of the clock. Here the reset of the output depends on the clock.



	module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
		always @ (posedge clk )
		begin
			if (sync_reset)
				q <= 1'b0;
			else	
				q <= d;
		end
	endmodule

**Simulation**:
Code for Simulation:
```
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/324a6568-2300-4d4d-b0b7-7a53d67a39bb)

**Synthesized circuit**:
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_syncres.v
yosys> synth -top dff_syncres
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/0dfdb64f-9550-4877-971c-4c3f944f3277)

**d-flipflop with synchronous and asynchronbous reset**- Here the output **q** goes low whenever asynchronous reset is high where output doesn't depend on clock and also when synchronous reset is high and posedge of clock occurs.


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

**Simulation**:
Code for Simulation:
```
iverilog dff_asyncres_syncres.v tb_dff_asyncres_syncres.v

./a.out
gtkwave tb_dff_asyncres_syncres.vcd
```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/c25e19ea-c5ff-4f81-adff-d4e3d0dfa078)

**Synthesized circuit**:
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_asyncres_syncres.v
yosys> synth -top dff_asyncres_syncres
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/b0083a53-4f76-4c96-ad39-69acbe74c638)

</details>

<details>
	
<summary> Interesting optimisations </summary>

This lab session deals with some automatic and interesting optimisations of the circuits based on logic. In the below example, multiplying a number with 2 doesn't need any additional hardeware and only needs connecting the bits from **a** to **y** and grounding the LSB bit of y is enough and the same is realized by Yosys.

	module mul2 (input [2:0] a, output [3:0] y);
		assign y = a * 2;
	endmodule

**Synthesized circuit**:

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/46726dc9-c2df-4240-a0db-6c0476ec104e)


**Netlist for the above schematic**

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/23ad2a7d-4494-4f29-9e07-9f73ca2c4214)


Special case of multiplying **a** with **9**. 

The schematic for the same is shown below:

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/4e9a643c-ca9c-48ba-8a98-19277c4f16bc)


**Netlist for the above schematic**

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/1a36f861-d422-4af8-afa8-d77f5341e357)

</details>

</details>

<details>
<summary>Day 3--Combinational and sequential optmizations</summary>
	
## Logic Optimisation

- *Combinational Logic Optimisation*
	- Constant Propogation
	- Boolean logic Optimisation
- *Sequential Logic Optimisation*
	- Sequential constant propogation
	- State optimisation
	- Retiming
	- Sequential logic cloning

<details>
<summary>Combinational Logic Optimisation</summary>
	
Command to optimize the circuit by yosys is ``yosys> opt_clean -purge``

 ## Example 1
 ```
 module opt_check (input a , input b , output y);
		assign y = a?b:0;
	endmodule
```
Use the below commands to get the optimized circuit:
```
yosys:read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:read_verilog opt_check.v
yosys:synth -top opt_check
yosys:opt_clean -purge
yosys:abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:show

```
**Optimized circuit**

![opt-check](https://github.com/spurthimalode/pes_asic_class/assets/142222859/a384418d-3ded-40b6-89b4-ef80b15dc30a)


## Example 2
```
module opt_check2 (input a , input b , output y);
		assign y = a?1:b;
	endmodule
```
Use the below commands to get the optimized circuit:
```
yosys:read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:read_verilog opt_check2.v
yosys:synth -top opt_check2
yosys:opt_clean -purge
yosys:abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:show

```
**Optimized circuit**

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/67f6deac-ed24-4826-8631-ac2197a12117)


 **Example 3**
```
	module opt_check3 (input a , input b, input c , output y);
		assign y = a?(c?b:0):0;
	endmodule
```
Use the below commands to get the optimized circuit:
```
yosys:read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:read_verilog opt_check3.v
yosys:synth -top opt_check3
yosys:opt_clean -purge
yosys:abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:show

```
**Optimized circuit**

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/e7ec7102-64ed-4401-aa28-bcf2c6fd329f)

 **Example-4**
```
	module opt_check4 (input a , input b , input c , output y);
		assign y = a?(b?(a & c ):c):(!c);
	endmodule
```
Use the below commands to get the optimized circuit:
```
yosys:read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:read_verilog opt_check4.v
yosys:synth -top opt_check4
yosys:opt_clean -purge
yosys:abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:show

```
**Optimized circuit**

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/ebc10cba-3743-45cb-a78e-2af7d0bc5a94)

**Example 5**
```
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
Use the below commands to get the optimized circuit:
```
yosys:read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:read_verilog multiple_module_opt.v
yosys:synth -top multiple_module_opt
yosys:abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:flatten
yosys:opt_clean -purge
yosys:show

```

**Optimized circuit**
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/813f55c3-ef22-4d72-b186-4f425b3c4240)


**Example 6**
```
module sub_module(input a , input b , output y);
		assign y = a & b;
	endmodule

	module multiple_module_opt2(input a , input b , input c , input d , output y);
		wire n1,n2,n3;
		sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
		sub_module U2 (.a(b), .b(c) , .y(n2));
		sub_module U3 (.a(n2), .b(d) , .y(n3));
		sub_module U4 (.a(n3), .b(n1) , .y(y));
	endmodule
```
Use the below commands to get the optimized circuit:
```
yosys:read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:read_verilog multiple_module_opt2.v
yosys:synth -top multiple_module_opt2
yosys:abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys:flatten
yosys:opt_clean -purge
yosys:show

```
**Optimized circuit**
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/5f68ccd1-f282-4f83-8da8-ef0084db9819)
</details>

<details>
<summary>Sequential Logic Optimization</summary>
	
**Example 1**

```
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
**Simulation**
Code for Simulation:
```
iverilog dff_const1.v tb_dff_const1.v
./a.out
gtkwave ttb_dff_const1.vcd
```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/2c9c713e-69f0-4e40-b7b1-95dbf69490f5)

**Synthesis**<br />
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_const1.v
yosys> synth -top dff_const1
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/a4fd820f-237f-43e4-979d-52a570689a8e)

**Example 2**

```
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


**Simulation**
Code for Simulation:
```
iverilog dff_const2.v tb_dff_const2.v
./a.out
gtkwave ttb_dff_const2.vcd
```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/98183793-169c-4c1b-9846-15f6523bb31a)

**Synthesis**
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_const2.v
yosys> synth -top dff_const2
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/c881e953-a920-4946-ac65-a60361584a73)

**Example 3**
```
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
**Simulation***
Code for Simulation:
```
iverilog dff_const3.v tb_dff_const3.v
./a.out
gtkwave ttb_dff_const3.vcd
```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/5dba1ba8-33c2-49e7-a388-59f4d1fea34d)

**Synthesis**
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_const3.v
yosys> synth -top dff_const3
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/6dd70b1a-33f5-4d28-b25f-51b0acaa9456)

**Example 4**
```
		module dff_const4(input clk, input reset, output reg q);
		reg q1;

		always @(posedge clk, posedge reset)
		begin
			if(reset)
			begin
				q <= 1'b1;
				q1 <= 1'b1;
			end
		else
			begin
				q1 <= 1'b1;
				q <= q1;
			end
		end
		endmodule
```

**Simulation**
Code for Simulation:
```
iverilog dff_const4.v tb_dff_const4.v
./a.out
gtkwave ttb_dff_const4.vcd
```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/84dea230-b439-414d-aa27-5d6d16f8117e)

**Synthesis**
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_const4.v
yosys> synth -top dff_const4
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/7c7fc484-2612-44b1-9ddf-fd8f95baafff)

**Example5**
```
		module dff_const5(input clk, input reset, output reg q);
		reg q1;
		always @(posedge clk, posedge reset)
			begin
				if(reset)
				begin
					q <= 1'b0;
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

**Simulation**
Code for Simulation:
```
iverilog dff_const5.v tb_dff_const5.v
./a.out
gtkwave ttb_dff_const5.vcd
```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/dd7cf383-197f-4898-aba7-769042d3d424)

**Synthesis**
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog dff_const5.v
yosys> synth -top dff_const5
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/07c44886-2591-40a3-88d6-13242f163c21)

</details>
<details>
<summary> Sequential optimisation of unused outputs </summary>

**Example1**
```
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
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog counter_opt.v
yosys> synth -top counter_opt
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```
![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/244f757e-8a1a-4bfa-947b-3f87b76a98b3)

**Updated counter logic-** 
```
	module counter (input clk , input reset , output q);
		reg [2:0] count;
		assign q = {count[2:0]==3'b100};
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
Code for Synthesis:
```
$ yosys
yosys> read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
yosys> read_verilog counter.v
yosys> synth -top counter
yosys> dfflibmap -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys>abc -liberty ../my_lib/verilog_model/sky130_fd_sc_hd__tt_025C_1v80.lib
yosys> show

```
All the other blocks in synthesizer are for incrementing the counter but the output is only from the three input NOR gate.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/43a8c99a-048d-4f9f-83b1-b156f1e6cc46)

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/3047b48d-c80e-43f1-9840-579b0f3ea434)

</details>
</details>

<details> 
<summary>Day 4--GLS,blocking vs non-blocking and Synthesis-Simulation mismatch</summary>

### GLS Concepts And Flow Using Iverilog

**What is GLS- Gate Level Simulation?**:<br />
GLS is generating the simulation output by running test bench with netlist file generated from synthesis as design under test. Netlist is logically same as RTL code, therefore, same test bench can be used for it.

**Why GLS?**:<br />
We perform this to verify logical correctness of the design after synthesizing it. Also ensuring the timing of the design is met.

Below picture gives an insight of the procedure. Here while using iverilog, we also include gate level verilog models to generate GLS simulation.

![image](https://github.com/spurthimalode/pes_asic_class/assets/142222859/ef05e2b8-48f4-46ed-911c-1a17d331a780)

### Synthesis Simulation Mismatch

There are three main reasons for Synthesis Simulation Mismatch:<br />
- Missing sensitivity list in always block
- Blocking vs Non-Blocking Assignments
- Non standard Verilog coding


</details>
 
## DETAIL DESCRIPTION OF COURSE CONTENT
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
