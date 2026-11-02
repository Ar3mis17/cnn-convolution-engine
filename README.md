<<<<<<< HEAD
# cnn-convolution-engine
Verilog implementation of a CNN convolution engine featuring a 3×3 Sobel-X filter, four-bank SRAM architecture, address generation, and output-memory management. Verified using Cadence Xcelium.
=======
# CNN Convolution Engine — SystemVerilog

A Verilog/SystemVerilog implementation of a CNN convolution datapath using a four-bank SRAM architecture, address generation, a convolution engine, and output-memory storage.

## Overview

The design scans a 256 × 256 image and computes a 3 × 3 Sobel-X convolution over valid windows. The resulting output dimensions are 254 × 254, producing 64,516 convolution results.

The self-checking testbench preloads the four SRAM banks with test pixels, loads the Sobel-X kernel through the weight interface, starts the scan, and compares each streamed result against an independently calculated reference value and expected output coordinate.

## Design Components

- **Convolution top level:** Integrates the read path, convolution engine, and output memory.
- **Convolution engine:** Performs the 3 × 3 weighted pixel computation.
- **Memory read path:** Coordinates image reads from four SRAM banks.
- **Address generator:** Generates bank and address control for the image scan.
- **SRAM banks:** Model the banked image-memory organization.
- **Output memory:** Stores convolution results.
- **Self-checking testbench:** Initializes the image and kernel, checks output data and coordinates, and reports pass/fail status.

## Repository Files

| File | Description |
|---|---|
| `P2cnn_conv_top.v` | Top-level CNN convolution module |
| `P2conv_engine.v` | 3 × 3 convolution datapath |
| `P2out_mem.v` | Output-memory module |
| `cnn_l1_read_top.v` | SRAM read-path controller |
| `addr_gen.v` | Bank and address generation logic |
| `sram_bank.v` | SRAM bank model |
| `P2tb_cnn_conv_top.v` | Self-checking top-level testbench |

## Simulation

The project was simulated using Cadence Xcelium.

Run the testbench from the directory containing these files:

```bash
xrun -64bit -sv -timescale 1ns/1ps -access +rwc -top P2tb_cnn_conv_top P2tb_cnn_conv_top.v -l xrun.log
```

The testbench includes the dependent RTL files using Verilog `` `include `` directives, so compile the top-level testbench file rather than separately compiling the included modules. Compiling both the testbench and its included modules separately can cause duplicate module-definition errors.

To open the waveform GUI, add `-gui`:

```bash
xrun -64bit -sv -timescale 1ns/1ps -access +rwc -top P2tb_cnn_conv_top P2tb_cnn_conv_top.v -gui
```

## Verification Results

The supplied Xcelium simulation log reports:

```text
Total conv results observed : 64516 (expected 64516)
Errors                      : 0
>>>>>> TEST PASSED <<<<<<
Simulation complete via $finish(1) at time 650485 NS + 0
```

**Result:** 64,516 convolution outputs observed, zero reported errors, and the testbench completed successfully.

*The verification results are reproduced from the supplied simulation log. The simulation was not rerun while preparing this repository.*

## Tools

- Verilog / SystemVerilog
- Cadence Xcelium
- SimVision (for waveform inspection)
>>>>>>> f20a875 (Initial Commit)
