# AXI4 Slave Verification using UVM

This repository contains the Verilog RTL for an AXI4 Slave with a simple memory model and a comprehensive UVM testbench designed to verify its protocol compliance and functionality. The verification of industry-standard bus protocols like AXI is a cornerstone of modern SoC design.

---



### Project Overview

The primary goal is to create a robust, constrained-random verification environment to test the AXI4 slave's ability to handle read and write burst transactions. The UVM testbench emulates an AXI Master, driving all five AXI channels to interact with the slave.

-   **DUT**: An `axi_slave` module that implements the necessary logic to respond to AXI4 transactions. It contains a 256-byte internal memory and supports:
    -   Concurrent Read and Write operations.
    -   Burst transactions (`INCR` type demonstrated).
    -   Configurable burst length (`AWLEN`, `ARLEN`) and data size (`AWSIZE`, `ARSIZE`).
-   **Verification Environment**: A UVM testbench that acts as an **AXI Master**.
    -   **Multi-Channel Driver**: The `axi_driver` contains tasks to drive the relevant AXI channels in the correct sequence for both write (`do_write`) and read (`do_read`) transactions.
    -   **Constrained-Random Sequences**: The `axi_base_seq` generates a mix of read and write `axi_trans` items with random addresses, lengths, and data payloads.
    -   **(Future Work)**: A complete environment would also include a comprehensive monitor and a predictive scoreboard to verify data integrity on every transaction.

---

### File Structure

-   `rtl/axi_slave_design.v`: Contains the Verilog RTL for the AXI4 Slave module.
-   `tb/axi_slave_tb.sv`: Contains the complete SystemVerilog/UVM testbench that emulates an AXI Master.

---

### Key Verification Concepts Illustrated

-   **Multi-Channel Protocol Verification**: The complexity of AXI lies in managing its five independent channels (Write Address, Write Data, Write Response, Read Address, Read Data). The driver demonstrates the handshaking logic required for this.
-   **Burst Transactions**: The testbench generates and drives burst transactions, which involve an address phase followed by multiple data phases, exercising the slave's internal address incrementing and beat counting logic.
-   **Layered Stimulus Generation**: A high-level sequence (`axi_base_seq`) generates abstract transactions (`axi_trans`), which the driver then translates into the detailed, cycle-by-cycle signal activity of the AXI protocol.

---

### How to Run

1.  Compile both `rtl/axi_slave_design.v` and `tb/axi_slave_tb.sv` using a simulator that supports SystemVerilog and UVM.
2.  Set `tb` as the top-level module for the simulation.
3.  Execute the simulation. You can observe the AXI transactions being driven to the DUT in the simulation log or waveform viewer.
