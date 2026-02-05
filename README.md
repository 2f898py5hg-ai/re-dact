# RE-DACT Artifacts for USENIX'26 Cycle 2

Public repository containing artifacts associated with the paper titled **"RE-DACT: In Search of Robust Hardware Redaction against Practical Reverse Engineering Attacks"**.

## Overview

**RE-DACT** is a robust fine-grain redaction methodology that aims protect hardware intellectual properties (IPs) against practical reverse engineering (RE) attacks where the adversary has access to a library of known designs, similar to known-plaintext attack (KPA) in cryptography. To address this threat, **RE-DACT** employs novel heuristic-driven randomizations, $\mathbb{RT}$, that introduce significant functional and structural transformations into the redacted designs, inspired by the indistinguishability of a cryptosystem.

## Artifact Details

The artifacts shared in this repository correspond to the results/metrics reported in the paper for the evaluation benchmarks used during overhead and security analysis.

The shared artifacts include the following:
* Gate-level netlists for original and redacted IPs in Verilog.
* Corresponding SDC files containing synthesis constraints.
* Functional bitstream for the redacted logic.
* Synthesis reports for QoR and power consumption.
* Standard cell library used for synthesis and simulation.

### Security Analysis Artifacts

The security evaluation of **RE-DACT** is performed on multiple IPs from the following open-source benchmark suites: [ITC99](https://github.com/cad-polito-it/I99T), [ISCAS89](https://www.pld.ttu.ee/~maksim/benchmarks/iscas89/verilog/), [CEP](https://github.com/mit-ll/CEP/tree/master/generators/mitll-blocks/src/main/resources/vsrc).

For each redacted IP **RD**$i$ $(i \in \{1,2,3,4,5,6\})$, the following variants are included:
* 1 original design (unredacted): **OD**
* 4 redacted variants for functional indistinguishability: **RD-F**$j$ $(j \in \{0,1,2,3\})$
* 5 redacted variants for structural indistinguishability: **RD-S**$k$ $(k \in \{0,1,2,3,4\})$

It should be noted that the variants **RD-F**$0$ and **RD-S**$0$ are identical and lack any randomized transformations from $\mathbb{RT}$. These redacted IP variants correspond to fine-grain redaction from prior art.

The security analysis artifacts are included in the "security_analysis" directory:
```console
security_analysis/
├── rd1.b21/
│   ├── od/
│   │   ├── syn.out.sdc
│   │   ├── syn.out.v
│   │   ├── syn.power.txt
│   │   └── syn.qor.txt
│   ├── rd_f0/
│   │   ├── bitstream.txt
│   │   ├── syn.out.sdc
│   │   ├── syn.out.v
│   │   ├── syn.power.txt
│   │   └── syn.qor.txt
│   ├── rd_f1/...
│   ├── rd_f2/...
│   ├── rd_f3/...
│   ├── rd_s0 -> rd_f0/
│   ├── rd_s1/...
│   ├── rd_s2/...
│   ├── rd_s3/...
│   └── rd_s4/...
├── rd2.b22/...
├── rd3.s38417/...
├── rd4.s38584/...
├── rd5.md5/...
└── rd6.sha256/...
```

### Overhead Analysis Artifacts

The overhead evaluation of **RE-DACT** is performed on multiple IPs from the following open-source benchmark suites: [CEP](https://github.com/mit-ll/CEP/tree/master/generators/mitll-blocks/src/main/resources/vsrc), [Ibex](https://github.com/lowRISC/ibex), [OR1200](https://github.com/openrisc/or1200), [PicoRV32](https://github.com/YosysHQ/picorv32), [RISCV](https://github.com/ultraembedded/riscv), [VTR](https://github.com/verilog-to-routing/vtr-verilog-to-routing/tree/master/vtr_flow/benchmarks/verilog).

Similar to the security analysis artifacts, 1 **OD** and 9 **RD** variants are included for each redacted IP evaluated for overhead analysis.

The overhead analysis artifacts are included in the "overhead_analysis" directory, grouped under "cep" and "cpu" categories:
```console
overhead_analysis/
├── cep/
│   ├── aes_192.one_round_r1_and_final_round/
│   │   ├── od/
│   │   │   ├── syn.out.sdc
│   │   │   ├── syn.out.v
│   │   │   ├── syn.power.txt
│   │   │   └── syn.qor.txt
│   │   ├── rd_f0/
│   │   │   ├── bitstream.txt
│   │   │   ├── syn.out.sdc
│   │   │   ├── syn.out.v
│   │   │   ├── syn.power.txt
│   │   │   └── syn.qor.txt
│   │   ├── rd_f1/...
│   │   ├── rd_f2/...
│   │   ├── rd_f3/...
│   │   ├── rd_s0 -> rd_f0/
│   │   ├── rd_s1/...
│   │   ├── rd_s2/...
│   │   ├── rd_s3/...
│   │   └── rd_s4/...
│   ├── des3.des3/...
│   ├── dft.perm74590/...
│   ├── fir.FIR_filter/...
│   ├── gps.one_round_r1_r2/...
│   ├── idft.perm42359/...
│   ├── iir.IIR_filter/...
│   ├── md5.md5/...
│   ├── rsa.montprod/...
│   └── sha256.sha256_core/...
└── cpu/
    ├── arm_core.a25_execute/...
    ├── ibex.ibex_core/...
    ├── or1200.or1200_cpu/...
    ├── picorv32.picorv32/...
    └── riscv.riscv_core/...
```

### Bitstream Configuration

The redacted IP needs to be configured using the functional bitstream in "bitstream.txt" required to configure the redacted logic and restore the original functionality. For proper configuration, the bitstream must be assigned to the *N*-bit input bus named *"cfg_bits"*, where the bit value (0/1) at line *M* in "bitstream.txt" is mapped to the input pin *cfg_bits[M-1]*.

### Standard Cell Library

Both original and redacted netlists are mapped to Silvaco's 15nm Open-Cell Library ([NanGate 15nm](https://si2.org/open-cell-and-free-pdk-libraries/)). The library files used for synthesis and simulation steps are included in the "lib" directory:
```console
libs/
├── NanGate_15nm_OCL_conditional.v
├── NanGate_15nm_OCL_functional.v
├── NanGate_15nm_OCL_typical_conditional_ccs.db
└── NanGate_15nm_OCL_typical_conditional_ccs.lib
```

## License

This repository is distributed under the Apache-2.0 license. See [LICENCE](./LICENCE) for more information.
