# EXP-7  4 KB-ROM Memory-with-Read-and-Write-Operations

## Aim:

&emsp;&emsp;To design and simulate a 4KB ROM memory with read and write operations using Verilog HDL and verify the functionality through a testbench in the Vivado 2023.1 simulation environment.<br>

## Apparatus Required:

&emsp;&emsp;Vivado 2023.1 or equivalent Verilog simulation tool.<br>

&emsp;&emsp;Computer system with a suitable operating system.<br>

## Procedure:

## Launch Vivado 2023.1:

&emsp;&emsp;Open Vivado and create a new project.<br>

## Design the Verilog Code for ROM:

&emsp;&emsp;Write the Verilog code for a 4KB ROM memory with read and write capabilities.<br>

## Create the Testbench:

&emsp;&emsp;Write a testbench to simulate both the read and write operations, verifying that the data is correctly written to and read from the memory.<br>

## Add the Verilog Files:

&emsp;&emsp;Add the ROM Verilog module and the testbench file to the project.<br>

## Run Simulation:

&emsp;&emsp;Run the behavioral simulation in Vivado and check the memory's read and write operations.<br>

## Observe the Waveforms:

&emsp;&emsp;Analyze the waveform to verify that the memory read and write operations work as expected.<br>

## Save and Document Results:

&emsp;&emsp;Capture the waveform and include the simulation results in the final report.<br>

## Verilog Code for 4KB ROM Memory with Read and Write Operations:

&emsp;&emsp;In this design, we will implement a 4KB ROM. Since ROM is typically read-only, we will simulate the behavior as if it's writable, but in actual hardware, ROM is typically pre-programmed.<br>

&emsp;&emsp;4KB = 4096 Bytes = 4096 x 8 bits <br>

&emsp;&emsp;The address width for 4KB memory is 12 bits (2^12 = 4096).<br>


```
module rom_memory (
    input wire clk,
    input wire write_enable,   // Signal to enable write operation
    input wire [11:0] address, // 12-bit address for 4KB memory
    input wire [7:0] data_in,  // Data to write into ROM
    output reg [7:0] data_out  // Data read from ROM
);

    // Declare ROM with 4096 memory locations (each 8 bits wide)
    reg [7:0] ram[0:4095];

    always @(posedge clk) begin
        if (write_enable) begin
            // Write operation: Write data into the ROM at the given address
            rom[address] <= data_in;
        end
        // Read operation: Read data from the ROM at the given address
        data_out <= rom[address];
    end
endmodule
```

## Testbench for 4KB ROM Memory:

```
`timescale 1ns / 1ps
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

    // Monitor the values for verification
    initial begin
        $monitor("Time = %0t | Write Enable = %b | Address = %h | Data In = %h | Data Out = %h", 
                 $time, write_enable, address, data_in, data_out);
    end
endmodule
```
## Output Waveform:

![Screenshot 2024-10-17 141913](https://github.com/user-attachments/assets/d9892720-1a48-4487-95f0-1b5407abd6e3)



## Conclusion:

&emsp;&emsp;In this experiment, a 4KB ROM memory with read and write operations was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the memory operations and observing the output waveforms. The experiment demonstrates how to implement memory operations in Verilog, effectively modeling both the reading and writing processes for ROM.
