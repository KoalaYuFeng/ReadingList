# My Paper Reading List

This repository is a curated list of academic papers that I have read or plan to read, covering various topics in [FPGA, Graph Processing, Neural Network]. The list is organized by categories, with each paper indexed for easy reference.

## Table of Contents
### Multiple FPGA for graph processing framework
- [ForeGraph: Exploring Large-scale Graph Processing on Multi-FPGA Architecture](#foregraph)
- [GraVF-M: Graph Processing System Generation for Multi-FPGA Platforms](#gravf-m)
### Multi-CPU/GPU for graph processing framework
- [Gemini: A Computation-Centric Distributed Graph Processing System](#gemini)
- [Gluon: A Communication-Optimizing Substrate for Distributed Heterogeneous Graph analytics](#gluon)
- [Gluon-Async: A Bulk-Asynchronous System for Distributed and Heterogeneous Graph Analytics](#gluon-async)
- [CuSP: A Customizable Streaming Edge Partitioneer for Distributed Graph Analtytics](#cusp)
- [A Study of Graph Analytics for Massive Datasets on Distributed Multi-GPUs](#Analysis)
### FPGA in HBM for graph processing framework
- [ScalaGraph: A Scalable Accelerator for Massively Parallel Graph Processing](#scalagraph)
- [ReGraph: Scaling Graph Processing on HBM-enabled FPGAs with Heterogeneous Pipelines](#regraph)
### FPGA in networking
- [FpgaNIC: An FPGA-based Versatile 100Gb SmartNIC for GPUs](#fpganic)
- [ACCL: FPGA-Accelerated Collectives over 100 Gbps TCP-IP](#accl)
- [ACCL+: an FPGA-Based Collective Engine for Distributed Applications](#accl+)
### FPGA in accelerating Transformer/Llama
- [FlightLLM: Efficient Large Language Model Inference with a Complete Mapping Flow on FPGAs](#flightllm)
- [DFX: A Low-latency Multi-FPGA Appliance for Accelerating Transformer-based Text Generation](#dfx)
- [SSR: Spatial Sequential Hybrid Architecture for Latency Throughput Tradeoff in Transformer Acceleration](#SSR)
### FPGA in virtualization
- [Nimblock: Scheduling for Fine-grained FPGA Sharing through Virtualization](#nimblock)
- [Coyote: Do OS abstractions make sense on FPGAs](#coyote)
### Others
- [Others](#others)

---

### Multiple FPGA for graph processing framework

1. **[ForeGraph]**
    - **Title:** [ForeGraph: Exploring Large-scale Graph Processing on Multi-FPGA Architecture]
    - **Published In:** [FPGA, 2017]
    - **Summary:** Graph processing on Multi-FPGA platform. Simulation-based design. 2D-graph partition method with edge shuffling. Vertex data in BRAM and edge data in DRAM. Remote data access across mutli-FPGA boards (edge-cut graph partition). 
    - **Pros:** This is the first work to do graph processing on multi-FPGA with convincing results. Max: TW dataset (1.4B edges) on 4 FPGAs.
    - **Cons:** Do not have any details about the message combination (No communcaiton optimization method, communication inefficiency). Edge shuffling also need preprocessing efforts. Also exists workload imbalance among Multi-FPGAs.
    - **Tags:** `#GraphProcessing` `#FPGA`

2. **[GraVF-M]**
    - **Title:** [GraVF-M: Graph Processing System Generation for Multi-FPGA Platforms]
    - **Published In:** [TRETS, 2019]
    - **Summary:** Graph processing on Multi-FPGA platform. Based on PCIe interconnection with limited FPGA numbers (single machine). Hardware implementation design. Metis graph partition method. No open-sourced code.
    - **Pros:** Floating barrier is used to eliminate the sync cost among super-steps in BSP model under vertex-centric graph processing algorithm.
    - **Cons:** No detailed description about the graph processing PE architecture. Performance is not optimal. No quatified explaination about the workload imbalance and communication cost. The performance model in this paper takes no consideration of the power-law distribution of graph data. (inaccurate)
    - **Tags:** `#GraphProcessing` `#FPGA`

---

### Multi-CPU/GPU for graph processing framework

1. **[Gemini]**
    - **Title:** [Gemini: A Computation-Centric Distributed Graph Processing System]
    - **Published In:** [OSDI, 2016]
    - **Summary:** Graph processing on Multi-CPU platform, baseline. Based on network interconnection, vertex-centric graph processing model, NUMA-aware. Hardware implementation design, open sourced.
    - **Pros:** Efficient graph processing and can fully utilize memory bandwidth when the graph data is concentrated. Locality-aware chunking is an efficient and light-weight partition method to achieve load balance.
    - **Cons:** Not good at communication optimization (overlap efficiency is not high), especially in irregular distributed graph data, the communication cost is obvious. Performance fluctuates based on data distribution. Workload stealing is only enabled between threads, not between nodes.
    - **Tags:** `#GraphProcessing` `#CPUs`

2. **[Gluon]**
    - **Title:** [Gluon: A Communication-Optimizing Substrate for Distributed Heterogeneous Graph analytics]
    - **Published In:** [PLDI, 2018]
    - **Summary:** Graph processing on Multi CPU/GPU platform. Open-sourced. vertex-centric + sync. Follow the reduce + boardcast pattern to update vertex.
    - **Pros:** partition strategies (UVC,CVC,IEC,OEC) are used to reduce communication overhead. The performance under above 32 nodes of gloun is better than Gemini. Scaling is good when node number is larger than 32.
    - **Cons:** The performance under 8-16 nodes of gloun is worse than Gemini. For PR, the final performance per node is worse than Gemini.
    - **Tags:** `#GraphProcessing` `#CPU/GPU`

3. **[Gluon-Async]**
    - **Title:** [Gluon-Async: A Bulk-Asynchronous System for Distributed and Heterogeneous Graph Analytics]
    - **Published In:** [PACT, 2019]
    - **Summary:** Graph processing on Multi CPU/GPU platform. The first async distributed GPU graph processing system. Baseline. Open-sourced. lock-free + non-blocking + bulk-async runtime. 
    - **Pros:** Cartesian vertex cut is used for graph partition and LCI is used to replace MPI for CPU data communication since LCI is more efficient.
    - **Cons:** Do not have enough performance improvemet. Only 1.5x compared with gluon.
    - **Tags:** `#GraphProcessing` `#CPU/GPU`

4. **[Cusp]**
    - **Title:** [CuSP: A Customizable Streaming Edge Partitioner for Distributed Graph Analytics]
    - **Published In:** [IPDPS, 2019]
    - **Summary:** Graph partition method used in Gloun and Gloun-async. These paper are from the same group. Open-sourced + baseline. Can be used for vertex cut graph partition.
    - **Comments:** Faster than XtraPulp.
    - **Tags:** `#GraphProcessing` `#Partition`

2. **[Analysis]**
    - **Title:** [A Study of Graph Analytics for Massive Datasets on Distributed Multi-GPUs]
    - **Published In:** [IPDPS, 2020]
    - **Summary:** Graph processing on Multi GPU platform. Also from the smae group. 
    - **Pros:** The Cartesian vertex cut (CVC) is quite suitable for graph partition in multiple GPUs. The overhead between CPU and GPU data communication in each super step for data sync is high.
    - **Cons:** (need profiling and summarize)
    - **Tags:** `#GraphProcessing` `#analysis`
---

### FPGA in networking

1. **[FpgaNIC]**
    - **Title:** [FpgaNIC: An FPGA-based Versatile 100Gb SmartNIC for GPUs]
    - **Published In:** [ATC, 2022]
    - **Summary:** GPU-centric network interface card. GPU can communicate its own data with network interface in FPGA side.  
    - **Pros:** The data transmission between GPU memory and FPGA do not need CPU cycles. Allow in-data path processing. Full stack for implemetation + open sourced.
    - **Cons:** Still rely on PCIe to transmit data between GPU memory and network stack. No comparsion with GPU Direct and Rocev2. (RDMA)
    - **Tags:** `#Networking` `#FPGA`

---

### FPGA in Transformer and LLM

1. **[DFX]**
    - **Title:** [DFX: A Low-latency Multi-FPGA Appliance for Accelerating Transformer-based Text Generation]
    - **Published In:** [HotChips && MICRO, 2022]
    - **Summary:** Accelerate transformer using multiple-FPGAs (U280), and the performance is better than nvidai V100. Not open-sourced. 
    - **Pros:** Better performance than GPU V100 and is the first work to accelerate transformer using multi-FPGAs.
    - **Cons:** The sync cost is high, leads to reduced scale-out performance (scalablity 1.5x). And need sync twice in model execution, after multi-head attention + after FC attention.
    - **Tags:** `#Accelerator` `#FPGA` `#Transformer`

2. **[FlightLLM]**
    - **Title:** [FlightLLM: Efficient Large Language Model Inference with a Complete Mapping Flow on FPGAs]
    - **Published In:** [FPGA, 2024]
    - **Summary:** Single FPGA for LLAMA 7B acceleration, open-sourced.
    - **Pros:** [TODO]
    - **Cons:** [TODO]
    - **Tags:** `#Accelerator` `#FPGA`

3. **[SSR]**
    - **Title:** [SSR: Spatial Sequential Hybrid Architecture for Latency Throughput Tradeoff in Transformer Acceleration]
    - **Published In:** [FPGA, 2024]
    - **Summary:**  Single FPGA for transformer acceleration, open-sourced.
    - **Pros:** [TODO]
    - **Cons:** [TODO] 
    - **Tags:** `#Accelerator` `#FPGA` `#Transformer`

---


### FPGA in virtualization

1. **[Nimblock]**
    - **Title:** [Nimblock: Scheduling for Fine-grained FPGA Sharing through Virtualization]
    - **Published In:** [ISCA, 2023]
    - **Summary:** Fine-grained FPGA resource sharing through virtualizaion. The goal of scheduling method is to minimize the average response time of N pending applications.
    - **Pros:** Implement a overlap to support FPGA virtualization. Support workload scheduling with priority, (preemption). Scheduling with time-multiplex and space-multiplex. Pipeline across batches to achieve the reduction of responce time. Avoid latency of reconfiguration based on large batches.
    - **Cons:** (potential improvments): add os support, resource pool, run under multi FPGA in cloud environment.
    - **Tags:** `#Virtualization` `#FPGA`

2. **[coyote]**
    - **Title:** [Do OS abstractions make sense on FPGAs]
    - **Published In:** [OSDI, 2020]
    - **Summary:** user logic interface for every application. round-robin scheduling + TLB in each vFPGA for virtual memory + multi-channel stripping (currently unknown).
    - **Pros:** low resource overhead + 20ms per vFPGA partial reconfiguration + bandwidth + memory overhead (TODO).
    - **Discussions:** Discussion about the preemption on FPGA. 1. Following classical OS design principles. 2. System-wide state (network flows) can not be done efficiently.
    - **Tags:** `#Virtualization` `#FPGA`

---

### Efficient system for LLM [TODO]

1. **[vLLM]**
    - **Title:** [vLLM: Easy, Fast, and Cheap LLM Serving with PagedAttention]
    - **Published In:** [SOSP, 2023]
    - **Summary:** 
    - **Pros:** 
    - **Cons:** 
    - **Tags:** `#LLM` `#GPU/CPU`

2. **[Powerinfer]**
    - **Title:** [PowerInfer: Fast Large Language Model Serving with a Consumer-grade GPU]
    - **Published In:** []
    - **Summary:** 
    - **Pros:** 
    - **Discussions:** 
    - **Tags:** `#CPU/GPU` `#LLM`

2. **[Survey-LLM]**
    - **Title:** [Full Stack Optimization of Transformer Inference: a Survey]
    - **Published In:** []
    - **Summary:** 
    - **Pros:** 
    - **Discussions:** 
    - **Tags:** `#transformer` `#LLM`
---

### Others

<!-- Use this section for papers that don't fit into the above categories, numbering papers sequentially -->

