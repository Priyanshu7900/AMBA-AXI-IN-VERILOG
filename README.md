# AMBA‑AXI‑IN‑VERILOG

## About  
This repository contains a Verilog implementation of the **AMBA AXI (Advanced eXtensible Interface)** bus protocol — including example masters and slaves, peripheral cores, and AXI‑bus system integrations. It is intended as a hands‑on learning and demonstration repository for AXI interface design, master/slave interactions, and peripheral IP integration.

---

## Features  
- AXI master and slave modules (AXI4 or AXI4‑Lite)  
- Peripheral cores connected via AXI: register blocks, memory‑mapped I/O, simple bus‑accessible devices  
- Bus‑level behaviors including burst transfers, different data widths, handshake signaling  
- Directed testbenches for verifying functionality under AXI protocol rules  
- Simulation results (waveform captures, summaries) included for reference  

---

## Repository Structure  
```
AMBA‑AXI‑IN‑VERILOG/
│
├── AXI_MASTER_SLAVE_TOP/                     # Basic AXI master‑slave example  
│   ├── axi_master.v                          # AXI master module  
│   ├── axi_slave.v                           # AXI slave module  
│   ├── top.v                                 # Top‑level integrating master + slave  
│   ├── top_tb.v                              # Testbench for the top module  
│   └── simulation_results.pdf                 # Waveform capture and results  
│
├── AXI_WITH_BURST_TRANSFERS/                 # Example design: burst reads/writes via AXI  
│   ├── axi_master_burst.v                    # Master with burst capability  
│   ├── axi_slave_burst.v                     # Slave supporting burst operations  
│   ├── top.v                                 # Top‑level integration  
│   ├── top_tb.v                              # Testbench for burst example  
│   └── burst_waveforms.pdf                   # Simulation results  
│
├── AXI_MEM_MAPPED_PERIPHERAL_CORE/           # AXI peripheral cores (registers, simple I/O)  
│   ├── axi_reg_core.v                        # Read/write register core over AXI  
│   ├── axi_gpio_core.v                       # GPIO peripheral accessible via AXI  
│   ├── axi_mem_map_top.v                     # Top‑level connecting master + peripherals  
│   ├── axi_mem_map_tb.v                      # Testbench for peripheral integration  
│   └── results.pdf                            # Simulation summary  
│
├── AXI_WITH_WAIT_STATES_ERROR_HANDLING/      # AXI bus behaviors: wait‑states and error response  
│   ├── axi_slave_wait.v                       # Slave with inserted wait states  
│   ├── axi_slave_error.v                      # Slave demonstrating error response (SLVERR)  
│   ├── top_wait_error.v                       # Top‑level integrating special slave behaviors  
│   ├── top_wait_error_tb.v                    # Testbench for wait/error example  
│   └── wait_error_waveforms.pdf               # Simulation captures  
│
└── README.md                                  # This document  
```

---

## Description of Major Folders

### AXI_MASTER_SLAVE_TOP  
Basic example of an AXI master communicating with a simple AXI slave. Ideal for understanding the handshake signals (AW, W, B, AR, R channels) and basic read/write operation in AXI.

### AXI_WITH_BURST_TRANSFERS  
Demonstrates how burst read/write operations work in AXI (e.g., transferring multiple data beats per transaction). Useful for learning about `AXI_LEN`, `AXI_SIZE`, `AXI_BURST` settings, and how to handle multiple data beats.

### AXI_MEM_MAPPED_PERIPHERAL_CORE  
Contains AXI‑connected peripheral modules (such as register blocks or GPIO) that can be accessed by the AXI master. Demonstrates memory‑mapped I/O via AXI protocol.

### AXI_WITH_WAIT_STATES_ERROR_HANDLING  
Shows advanced bus behaviors: how a slave can insert wait‑states (by holding `AWREADY`, `WREADY`, `ARREADY` low), and how error responses (`SLVERR`, `DECERR`) are handled. Good for protocol robustness studies.

---

## Simulation Instructions (ModelSim / Questa)  
Example flow:

```bash
# Create work library  
vlib work  
vmap work work  

# Compile sources and testbench (example for base master‑slave)  
vlog AXI_MASTER_SLAVE_TOP/axi_master.v AXI_MASTER_SLAVE_TOP/axi_slave.v AXI_MASTER_SLAVE_TOP/top.v AXI_MASTER_SLAVE_TOP/top_tb.v  

# Run simulation (console mode)  
vsim -c top_tb -do "run ‑all; quit"  
```

For GUI mode to view waveforms:

```bash
vsim top_tb  
run ‑all  
```

---

## Key AXI Signals to Verify  
| Signal       | Description                                           |
|--------------|--------------------------------------------------------|
| `AWVALID`/`AWREADY` | Address write channel handshake                   |
| `WVALID`/`WREADY`   | Write data channel handshake                       |
| `BVALID`/`BREADY`   | Write response channel                             |
| `ARVALID`/`ARREADY` | Address read channel handshake                     |
| `RVALID`/`RREADY`   | Read data channel handshake                         |

Ensure each channel follows the valid/ready protocol and that burst, size, and response signals are correctly handled.

---

## How to Extend  
- Add additional AXI peripheral modules (e.g., memory controller, DMA).  
- Implement support for AXI4‑Lite and full AXI4 (if not already included).  
- Enhance testbenches with constrained‑random stimulus and coverage collection.  
- Integrate with FPGA board: connect to a memory‐mapped peripheral on hardware.  
- Build a UVM verification environment to verify AXI transactions in depth.

---

## References  
- [ARM AMBA AXI Protocol Specification (IHI 0022E)](https://developer.arm.com/documentation/ihi0022/latest)  
- AXI tutorials and user guides from FPGA vendors (Xilinx, Intel/Altera)  

---

## Notes  
- The modules and testbenches here are **for learning and simulation purposes only**, not for direct production use without further verification and sizing.  
- While the core protocol is implemented, timing closure, reset behaviour, and full synthesizability are not guaranteed.  
- Always review signal naming, constraints, and simulation behaviour when adapting to your own environment.

---  
