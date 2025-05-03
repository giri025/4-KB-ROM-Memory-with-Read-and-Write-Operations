## GIRI R
## 212223060068
# 4 KB-ROM-Memory-with-Read-and-Write-Operations
## Aim
To design and simulate a 4KB ROM memory with read and write operations using Verilog HDL and verify the functionality through a testbench in the Vivado 2023.1 simulation environment.

## Apparatus Required
Vivado 2023.1 or equivalent Verilog simulation tool.
Computer system with a suitable operating system.
## Procedure
Launch Vivado 2023.1:

Open Vivado and create a new project.
Design the Verilog Code for ROM:

Write the Verilog code for a 4KB ROM memory with read and write capabilities.
Create the Testbench:

Write a testbench to simulate both the read and write operations, verifying that the data is correctly written to and read from the memory.
Add the Verilog Files:

Add the ROM Verilog module and the testbench file to the project.
Run Simulation:

Run the behavioral simulation in Vivado and check the memory's read and write operations.
Observe the Waveforms:

Analyze the waveform to verify that the memory read and write operations work as expected.
Save and Document Results:

Capture the waveform and include the simulation results in the final report.
Verilog Code for 4KB ROM Memory with Read and Write Operations
In this design, we will implement a 4KB ROM. Since ROM is typically read-only, we will simulate the behavior as if it's writable, but in actual hardware, ROM is typically pre-programmed.
## Program 
4KB = 4096 Bytes = 4096 x 8 bits
The address width for 4KB memory is 12 bits (2^12 = 4096).
~~~
module rom_memory ( input wire clk, input wire write_enable,  input wire [11:0] address, 
 input wire [7:0] data_in,  output reg [7:0] data_out  );
reg [7:0] rom[0:4095];

always @(posedge clk) begin
    if (write_enable) begin
        rom[address] <= data_in;
    end
    data_out <= rom[address];
end
endmodule
~~~


## Testbench for 4KB ROM Memory

~~~
module rom_memory_tb;

// Inputs
reg clk;
reg write_enable;
reg [11:0] address;
reg [7:0] data_in;

// Outputs
wire [7:0] data_out;

// Instantiate the ROM module
rom_memory uut (
    .clk(clk),
    .write_enable(write_enable),
    .address(address),
    .data_in(data_in),
    .data_out(data_out)
);

// Clock generation
always #5 clk = ~clk;  // Toggle clock every 5 ns

// Test procedure
initial begin
    // Initialize inputs
    clk = 0;
    write_enable = 0;
    address = 0;
    data_in = 0;

    // Write data into memory
    #10 write_enable = 1; address = 12'd0; data_in = 8'hA5;  // Write 0xA5 at address 0
    #10 write_enable = 1; address = 12'd1; data_in = 8'h5A;  // Write 0x5A at address 1
    #10 write_enable = 1; address = 12'd2; data_in = 8'hFF;  // Write 0xFF at address 2
    #10 write_enable = 1; address = 12'd3; data_in = 8'h00;  // Write 0x00 at address 3

    // Disable write and start reading from memory
    #10 write_enable = 0; address = 12'd0;
    #10 address = 12'd1;
    #10 address = 12'd2;
    #10 address = 12'd3;

    // Stop the simulation
    #10 $stop;
end
~~~
## Output
![image](https://github.com/user-attachments/assets/a0b72729-c5b8-47ed-b5e1-45845e56d155)

## Conclusion
In this experiment, a 4KB ROM memory with read and write operations was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the memory operations and observing the output waveforms. The experiment demonstrates how to implement memory operations in Verilog, effectively modeling both the reading and writing processes for ROM.
