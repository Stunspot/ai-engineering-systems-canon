# AI-ENG-J — Throughput Mechanics - Memory Pressure, KV Cache, and Hardware Realities

Serving machine learning outputs under production-scale workload pressure is fundamentally a physical transport discipline. In the execution of transformer-based architectures, model intelligence is bound by memory movement before computation.1 Every inference request—encompassing input prompts, retrieved context, dynamic tool definitions, active session states, and generated tokens—is a physical memory allocation that competes for finite high-bandwidth memory (HBM), local cache lines, interconnect routing capacity, and clock cycles.1  
The critical inquiry for the platform architect is not the abstract speed of a model on synthetic benchmarks, but rather the volume of useful inference the system can deliver per unit time, per unit memory, and per unit cost.1 This capacity is defined against a highly heterogeneous, stochastic distribution of input lengths, context payloads, output requirements, concurrency spikes, cache hit rates, and latency service level objectives (SLOs).  
Throughput is governed by memory movement before model intelligence. Inference capacity is constrained by how prompts, contexts, KV cache, batches, generated tokens, and serving schedulers occupy and move through hardware under load.

## **Conceptual Glossary**

To ground high-dimensional AI system architecture in physical realities, the following taxonomy establishes the structural, computational, and memory management vocabulary utilized across the runtime domain:

| Term | Domain | Definition | Systemic Implication |
| :---- | :---- | :---- | :---- |
| **Throughput Mechanics** | Systems Execution | The physical science of serving useful model outputs under workload pressure by optimizing memory bandwidth, cache allocations, and scheduling policies.1 | Establishes throughput as a hardware-constrained execution process rather than an abstract mathematical call.1 |
| **Prefill Phase** | Compute Kernels | The initial forward pass of inference where the model processes all input prompt tokens simultaneously to build the initial KV cache.2 | Highly parallel and compute-bound; dominates the user-facing Time-to-First-Token (TTFT).2 |
| **Decode Phase** | Compute Kernels | The subsequent autoregressive phase where the model generates output tokens sequentially, one token per forward pass iteration.2 | Memory-bandwidth-bound; dominates the generation rate and Inter-Token Latency (ITL).2 |
| **Time-to-First-Token (TTFT)** | Latency Metrics | The elapsed duration between the physical ingestion of a request and the delivery of the first generated token to the client.2 | Primarily determined by queue wait times, context tokenization, and prefill phase execution speed.2 |
| **Inter-Token Latency (ITL)** | Latency Metrics | The time elapsed between the emission of successive output tokens during the autoregressive decode phase.2 | Governed by effective memory bandwidth and active concurrent batch memory footprint sizes.1 |
| **Key-Value (KV) Cache** | VRAM Allocation | The saved tensor activations of key and value states for past tokens across all attention layers, preventing redundant recalculation.6 | The primary dynamic driver of GPU memory pressure under concurrent serving loads.1 |
| **Context Window** | Model Specs | The maximum sequence length (input plus output tokens) that a model is mathematically configured to process in a single invocation. | Differs drastically from the *sustainable context window* supported under concurrent production batch loads.1 |
| **Paged Attention** | Virtual Memory | An attention-kernel memory allocation strategy that partitions the KV cache into fixed-size physical blocks scattered non-contiguously in VRAM.6 | Minimizes internal and external fragmentation to near-zero, enabling dynamic allocation and copy-on-write sharing.6 |
| **Continuous Batching** | Scheduler Policy | Iteration-level scheduling that allows requests to join and exit the execution batch at individual token boundaries.5 | Eliminates padding waste and dramatically scales GPU utilization compared to static or dynamic batching.5 |
| **Queue Depth** | Queueing Theory | The total number of pending inference requests waiting in the ingestion buffer to be admitted to the scheduler.5 | Directly impacts TTFT and dictates system stability boundaries under stochastic arrival spikes.3 |
| **Backpressure** | Flow Control | Traffic-throttling mechanisms that reject, delay, or route incoming requests when queue depths or memory utilization exceed capacity limits. | Prevents system collapse and cascading out-of-memory (OOM) failures under extreme demand burstiness.12 |
| **Prompt Caching** | Cache Architecture | Storing pre-computed KV states of stable, identical prompt segments (e.g., system instructions) in memory to bypass prefill compute.8 | Drastically reduces TTFT for applications with highly structured or repetitive inputs.8 |
| **Semantic Caching** | Cache Architecture | Caching complete natural language answers indexed by the semantic similarity of incoming queries to historic requests.15 | Eliminates full model evaluation for identical intents, but carries correctness, fresh-state, and permission risks.16 |
| **Prefix Reuse** | Cache Architecture | Reusing computed attention blocks of shared prefix strings (e.g., tool definitions) across multiple concurrent request paths.8 | Key mechanical benefit of SGLang's RadixAttention trie system.8 |
| **High-Bandwidth Memory (HBM)** | GPU Architecture | Co-packaged, ultra-wide-bus memory stacked directly on the GPU substrate to deliver massive data throughput.1 | The foundational physical factor constraining decode-phase speeds and sequence concurrency.1 |
| **Video Random Access Memory (VRAM)** | GPU Architecture | The aggregate physical memory pool directly addressable by the GPU, comprising high-speed HBM or GDDR modules.1 | Divided strictly between model weight parameters, runtime contexts, CUDA graphs, and the dynamic KV cache pool.1 |
0| **Dynamic Random Access Memory (DRAM)** | Host Architecture | Standard system RAM residing on the host motherboard, accessible to the GPU over the PCIe system bus.9 | Serves as the backup swap tier for evicted KV cache blocks under extreme memory pressure.6 |
| **Memory Bandwidth** | Data Transport | The rate at which data can be read from or written to physical storage by a processor, measured in Terabytes per second (TB/s).1 | The physical bottleneck governing decode performance; scales directly with GPU generational upgrades.1 |
| **Offloading** | Memory Management | The process of transferring inactive KV cache blocks or model weights from GPU VRAM to system DRAM or SSD storage.9 | Enables execution of ultra-large contexts or batches but introduces severe transfer-latency penalties over PCIe.9 |
| **Capacity Margin** | Capacity Sizing | The reserved physical compute, memory, and bandwidth headroom kept unallocated to absorb stochastic workload bursts and failovers. | Crucial for preventing preemption loops, cache thrashing, and SLA violations under dynamic production loads.3 |

## **The Physical Inference Lifecycle**

An incoming production request moves through a sequence of hardware-constrained state transitions. In typical multi-tenant, high-throughput engines, every step of this journey is governed by memory allocation budgets, metadata lookups, and hardware-level synchronization.

```        
+--------------------------------------------------------------------------------------+
|                         PHYSICAL INFERENCE LIFECYCLE                                 |
+--------------------------------------------------------------------------------------+
|                                                                                      |
|  [Incoming Request]                                                                  |
|          |                                                                           |
|          v                                                                           |
|  [Request Router / Model Gateway]                                                    |
|          |                                                                           |
|          +--> Route by tenant, model target, priority, and prefix-cache locality     |
|          |                                                                           |
|          v                                                                           |
|  [Ingestion Queue]                                                                   |
|          |                                                                           |
|          +--> Schedule via FCFS, Priority, VLT, deadline, or workload lane policy    |
|          |                                                                           |
|          v                                                                           |
|  [Context Compiler Assembly]                                                         |
|          |                                                                           |
|          +--> Merge system prompt, user input, history, tools, memory, evidence      |
|          |                                                                           |
|          v                                                                           |
|  [Tokenization]                                                                      |
|          |                                                                           |
|          +--> Serialize compiled strings into integer token arrays                   |
|          |                                                                           |
|          v                                                                           |
|  [Prefix / Radix Cache Lookup]                                                       |
|          |                                                                           |
|          +--> Match token prefix against existing KV-cache block paths               |
|          |                                                                           |
|          v                                                                           |
|     +----+---------------------------------------------+                             |
|     |                                                  |                             |
|     v                                                  v                             |
|  [Prefix Miss]                                  [Prefix Hit / Partial Hit]           |
|     |                                                  |                             |
|     |                                                  +--> Lock matched radix nodes |
|     |                                                  +--> Increment ref counts     |
|     |                                                  +--> Reuse matched KV blocks  |
|     |                                                  +--> Prefill only suffix      |
|     |                                                  |                             |
|     v                                                  |                             |
|  [Prefill Engine Execution] <--------------------------+                             |
|          |                                                                           |
|          +--> Run parallel prompt-token forward pass                                 |
|          +--> Generate initial K/V tensors for uncached tokens                       |
|          |                                                                           |
|          v                                                                           |
|  [KV Cache Allocation]                                                               |
|          |                                                                           |
|          +--> Map logical token blocks to physical PagedAttention blocks             |
|          +--> Register block-table entries in the serving runtime                    |
|          |                                                                           |
|          v                                                                           |
|  [Continuous Batching Scheduler]                                                     |
|          |                                                                           |
|          +--> Admit, drop, and reorder active sequences at token boundaries          |
|          |                                                                           |
|          v                                                                           |
|     +----+---------------------------------------------+                             |
|     |                                                  |                             |
|     v                                                  v                             |
|  [Free Blocks Available]                       [VRAM / KV Block Pressure]            |
|     |                                                  |                             |
|     |                                                  v                             |
|     |                                       [Iteration-Level Preemption]             |
|     |                                                  |                             |
|     |                                                  +--> Evict victim sequence    |
|     |                                                  +--> Recompute later, or      |
|     |                                                  +--> Swap KV blocks to DRAM   |
|     |                                                  +--> Return victim to queue   |
|     |                                                  |                             |
|     +---------------------------+----------------------+                             |
|                                 |                                                    |
|                                 v                                                    |
|  [Decode Engine Execution]                                                           |
|          |                                                                           |
|          +--> Generate one output token per autoregressive iteration                 |
|          +--> Read model weights and active KV cache from HBM each step              |
|          +--> Apply constrained decoding / compressed FSM checks when required       |
|          |                                                                           |
|          v                                                                           |
|  [Streaming / Response Emission]                                                     |
|          |                                                                           |
|          +--> Serialize token deltas and stream chunks to client socket              |
|          +--> Track TTFT, ITL, TPOT, throughput, and delivery stalls                 |
|          |                                                                           |
|          v                                                                           |
|  [Completion Detection]                                                              |
|          |                                                                           |
|          +--> Stop on EOS token, max-token limit, schema boundary, or cancellation   |
|          |                                                                           |
|          v                                                                           |
|  [KV Cache Release / Retention Policy]                                               |
|          |                                                                           |
|          +--> Decrement active reference counts                                      |
|          +--> Reclaim unreferenced physical blocks                                   |
|          +--> Retain reusable prefix nodes if cache policy allows                    |
|          |                                                                           |
|          v                                                                           |
|  [Telemetry Persistence]                                                             |
|          |                                                                           |
|          +--> Persist queue depth, cache hits, free blocks, preemptions, OOM risk    |
|          +--> Export latency spans, memory metrics, bandwidth use, and cost traces   |
|                                                                                      |
+--------------------------------------------------------------------------------------+
```

The physical operations, memory footprints, and potential bottlenecks associated with each phase of this lifecycle are mapped systematically:

| Phase | Physical Operations | VRAM/DRAM footprint | Hardware Bottleneck | Primary Failure Risk | Telemetry Signal |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Routing** | Ingests HTTP/gRPC requests; executes prefix hashing; matches target model nodes.8 | Negligible (CPU host memory only). | Network interface card (NIC) throughput; gateway CPU. | Stale route assignments; routing to cold-cache instances.8 | Gateway latency; routing cache hit rate. |
| **Queueing** | Places requests in scheduler-managed arrays (FCFS, Priority, or Deadline queues).5 | Minimal host DRAM memory overhead.9 | Host CPU memory bus; scheduler thread lock contention. | Queue saturation; Head-of-Line (HOL) blocking.5 | Active queue depth; queue wait duration (W_q). |
| **Tokenization** | Translates raw strings to token ID integers via CPU-based tokenization engines.8 | Minimal host DRAM allocation. | Host CPU single-thread execution limits.20 | Tokenizer thread blocking; high serialization overhead.1 | Tokenizer processing duration; tokenization rate. |
| **Context Assembly** | Resolves variables; merges system prompt templates, few-shot examples, and retrieved evidence packets.8 | Host DRAM to GPU HBM transfer buffer. | PCIe bus bandwidth during Host-to-Device (H2D) transfers.9 | Context bloat; prompt engineering payload overhead.8 | Context assembly duration; input payload size. |
| **Prefill** | Executes parallel matrix multiplications over the compiled prompt sequence to generate initial keys and values.2 | High transient activation allocations; initial KV cache block generation.1 | GPU Tensor Core compute capacity (FP16/FP8 FLOPS).1 | Out-of-memory (OOM) failures under extreme context lengths.8 | Time-to-First-Token (TTFT); prefill FLOPS. |
| **KV Cache Allocation** | Registers generated key-value tensors into PagedAttention physical blocks via the block table manager.6 | Dynamically locks blocks within the HBM cache pool.6 | GPU memory allocation lock speed; block space fragmentation.6 | Block starvation; allocation failures under concurrency.5 | Free block queue depth; block allocation latency. |
| **Decode** | Executes sequential vector-matrix multiplications to generate a single token per iteration.2 | Reads full model weights and growing KV caches from HBM on every step.1 | GPU HBM effective read bandwidth (beta).1 | Token generation stalls; degraded generation throughput.1 | Inter-Token Latency (ITL); tokens-per-second. |
| **Streaming** | Serializes generated token deltas; pushes SSE/chunked streams to client sockets. | Negligible. | Network stack write buffers; thread context switching. | Output streaming stutter; client-side TCP window stalls.2 | Time-to-client-delivery; network packet drop rates. |
| **Validation** | Parses output tokens against structural FSM constraints (JSON schemas, regex).8 | Host DRAM or local GPU register state checks.15 | CPU validation thread speed or GPU parsing kernels.15 | FSM state transition blocking; validation rejection loops.15 | Validation parser latency; schema rejection rates. |
| **Completion** | Evaluates EOS token detections or structural boundary matching; halts iteration. | Releases local processing state. | Scheduler status update performance. | Completion sequence failures. | Overall request execution duration. |
| **Cache Release** | Decrements reference count of used radix nodes; marks unreferenced physical blocks as free.14 | Frees physical blocks back to the global allocator pool.6 | Allocator mutex lock release speed. | Cache leak conditions; delayed memory reclamation. | Free block reclamation rate. |
| **Telemetry Logging** | Formats and exports execution spans, hardware profiles, and consumption statistics. | Asynchronous non-blocking host DRAM buffers. | Local storage write IOPS or external observability network limits. | Metric export delays; telemetry buffer overflow. | Observability pipeline latency; metric packet drops. |

## **Prefill, Decode, and Latency Anatomy**

The asymmetric execution profiles of the prefill and decode phases dictate distinct system bottlenecks and tuning parameters.2 During the prefill phase, all N_in prompt tokens are evaluated in parallel.2 This multi-token execution permits substantial matrix-matrix multiplication (GEMM) parallelization, which exposes high arithmetic intensity—the ratio of floating-point computations executed per byte of memory read.1  
Because the arithmetic intensity of prefill operations sits well to the right of the hardware ridge point (e.g., 591 FLOPs/Byte on an NVIDIA H100 SXM5), the execution is strictly **compute-bound**.1 The limiting physical factor is the total floating-point throughput (FLOPS) of the GPU's Tensor Cores.1 Consequently, scaling compute capacity directly compresses TTFT.1

Arithmetic Intensity (FLOPs/Byte)  
```
  ▲  
  │                     Compute-Bound Zone (Flat Ceiling)  
  │                   ┌───────────────────────────────────  
  │                  / (H100 Ridge Point: ~591 FLOPs/Byte)   
  │                 /   
  │                /  ◄── Prefill Phase (GEMM-heavy, high intensity)   
  │               /  
  │              /  
  │             / ◄── Decode Phase (GEMV-heavy, 1-10 FLOPs/Byte)   
  │            /  
  │  _________/ Memory-Bound Zone (Slanted Slope)   
  └─────────────────────────────────────────────────────────────►  
                                                 Workload Size
```

During the decode phase, the computational workload transitions from a matrix-matrix structure to sequential vector-matrix multiplications (GEMV).4 At each autoregressive iteration, the model must read all parameters (weights) and the entire, growing KV cache history of the sequence from high-bandwidth memory to process a single query token.1  
Because this movement involves transferring tens of gigabytes of data to perform a fraction of the computational math, the arithmetic intensity plummets to a range of 1 to 10 FLOPs/Byte.2 This places execution firmly in a **memory-bound** regime under the slanted roofline.1  
In this state, Tensor Cores sit underutilized, frequently stalled while waiting for memory controllers to stream data from HBM.2 Adding raw FLOP capacity does not improve decode-stage speeds; inter-token latency is directly governed by effective memory bandwidth (beta).1

### **Mathematical Prefill/Decode Cost Model**

Let a transformer model be parameterized by parameter count P, layer count L, key-value head count H_kv, head dimension d, and numerical representation precision b in bytes.1 For a given execution batch B, let the input prompt length be N_in and the active sequence context length be N_context.1  
The memory footprint of the model weights is defined by:  
M_weights = P * b  
At a single decode iteration step, the system must stream the model weights and the active KV cache pool across the memory bus.1 The aggregate data transfer volume per decode step (M_step) is modeled as:  
M_step = M_weights + (2 * B * L * H_kv * d * N_context * b) 1  
Under memory-bandwidth-bound constraints, the physical latency of a single decode iteration step (t_step) is limited by the effective memory bandwidth of the hardware (beta_effective):  
t_step ~ M_step / beta_effective = (P * b + 2 * B * L * H_kv * d * N_context * b) / beta_effective 1  
To account for scheduler overhead, network latency, and queueing delays under production workloads, the user-facing latency metrics of Time-to-First-Token (TTFT) and Inter-Token Latency (ITL) are defined by the following formulations:  
TTFT(r) = W_queue(r) + t_tokenize(r) + (ceiling(N_in / C_chunk) * t_prefill_step(B, C_chunk)) + t_network 5  
ITL(r) = t_decode_step(B_active, N_mean_context) + t_network_delta 11  
Total Latency(r) = TTFT(r) + sum_{i=1}^{N_out-1} ITL_i(r) 11  
Where C_chunk is the maximum chunked prefill size limit configured in the scheduler.8 This parameter prevents large input prompts from blocking the batch scheduler and causing severe latency spikes for concurrent decode sequences.8

### **Latency Budget Profiles**

Because workloads differ in their latency tolerance and user experience constraints, systems must be configured to prioritize different optimization targets:

| Workload Class | Target TTFT SLO | Target ITL SLO | Primary Bottleneck | Optimal Tuning Profile |
| :---- | :---- | :---- | :---- | :---- |
| **Real-Time Interactive** (e.g., Voice Bots, Autocomplete) | < 150 ms | < 15 ms | Prefill initialization speed; immediate thread scheduling.2 | Minimize chunked prefill sizes; isolate batch lanes; provision dedicated GPU replicas.5 |
| **Interactive Chat** (e.g., Customer Service, Co-pilots) | < 400 ms | < 30 ms | Queue wait time; decode bandwidth under high concurrency.1 | Enable RadixAttention prefix caching; utilize continuous batching; enforce dynamic batching caps.8 |
| **Agent Tool Loops** (e.g., Multi-Step Reasoners) | < 100 ms | < 20 ms | Multiple sequential round-trips; tool schema prefill overhead.8 | Prioritize RadixAttention prefix caching for tool schemas; locate tool structures at prompt root.8 |
| **Document Analysis** (e.g., Legal PDF Summarizers) | < 1500 ms | < 50 ms | Massive prefill compute over long sequences; memory pressure.2 | Enable chunked prefill; utilize FP8 KV cache formats; scale HBM capacity horizontally.1 |
| **Batch Processing** (e.g., Offline Extractions) | < 10000 ms | < 100 ms | Raw aggregate token throughput; physical memory saturation limits.5 | Maximize concurrency limits; disable chunked prefill; use maximum static memory allocations.8 |

## **KV Cache: The Hidden Capacity Governor**

While model weights require a static allocation in VRAM, the Key-Value (KV) cache is highly dynamic, scaling directly with sequence lengths and concurrent request volume.7 During autoregressive generation, the attention mechanism requires access to the computed key and value states of all past tokens in the sequence to compute the attention weights for the next token.6 To avoid recomputing these projections at every autoregressive step, they are persisted in memory.6  
The physical memory footprint V_kv (in bytes) occupied by the KV cache for a single sequence of context length N_context is calculated as follows:  
V_kv = 2 * L * H_kv * d * N_context * b 1  
The leading coefficient of 2 accounts for the distinct storage of key and value states.1  
For a concrete demonstration, consider the Llama-3-70B model served with FP8 precision (b = 1) and Grouped-Query Attention (GQA).1 The model architectural parameters are:

* Layer Count (L): 80  
* KV Head Count (H_kv): 8  
* Head Dimension (d): 128

For a single sequence of 16,384 context tokens (16K):  
V_kv_16K = 2 * 80 * 8 * 128 * 16384 * 1 byte = 26,843,545,600 bytes ~ 25.0 GB 1  
At a target serving concurrency of B = 16 active requests with identical length:  
V_kv_fleet = 16 * 25.0 GB = 400.0 GB 1  
When utilizing standard multi-head attention (MHA) where H_kv equals the query head count (H_query = 64), this memory footprint scales eightfold, demanding 3.2 TB of HBM for the same concurrency. This highlights why architectural features like GQA are critical to modern inference hardware economics.1  
Historically, serving frameworks allocated a contiguous block of physical memory for each request corresponding to the absolute maximum context window (e.g., N_max = 8192 or 32768).6 This native strategy introduced three distinct vectors of severe memory fragmentation and waste 6:

1. **Internal Fragmentation**: Memory is allocated for the maximum sequence length, but the client terminates generation early.6 If 2048 tokens are reserved but only 300 are generated, the remaining 1748 token allocations remain idle, locked, and unusable by other requests.6  
2. **Reservation Waste**: Memory is reserved for future tokens that have not yet been generated.6 In a sequence currently at token 100, the memory for tokens 101 through 2048 is actively held empty, representing a structural zero-utilization penalty.6  
3. **External Fragmentation**: Non-contiguous gaps form in GPU memory as requests with varying sequence lengths are initialized, completed, and evicted.6 Because memory allocations are historically contiguous, a new request requiring 1024 continuous token allocations will fail to initialize if the free memory is split into separate, non-adjacent blocks of 512.6

Under these contiguous allocation schemes, empirical measurements show that actual GPU memory utilization hovers between 20% and 40%, meaning up to 80% of valuable HBM sits idle, restricting batch sizes and causing artificial throughput bottlenecks.6

## **Paged Attention, Cache Virtualization, and Eviction**

To reclaim fragmented memory, modern serving runtimes employ cache virtualization.7 Inspired by virtual memory page tables in classical operating systems, PagedAttention decouples the logical sequence of a request's KV cache from its physical placement in HBM.6  
The physical HBM reserved for KV cache is partitioned into a global pool of fixed-sized physical blocks.6 Each block contains KV projections for a small, fixed number of tokens, typically designated as a block size of B_size = 16 or 32 tokens.7  
When a request arrives, its logical sequence of tokens is divided into logical blocks.6 The scheduler's memory manager maps these logical blocks to non-contiguous physical blocks anywhere in the global pool, maintaining this mapping inside a sequence-specific block table.6

Logical Blocks (Sequence Order):  
 ───► Maps to physical block   
 ──► Maps to physical block   
 ──► Maps to physical block   
 ──► Maps to physical block 

Physical HBM Block Pool (Scattered non-contiguously):  
         
                                  (Log 2)                                     (Log 0)  
    
                                             (Log 1)

                                  (Log 3)

During attention execution, the CUDA attention kernel is modified to perform dynamic gather operations.6 Instead of reading a single continuous memory stride, the kernel resolves physical memory addresses on the fly 6:  
Logical Block Index (idx_block) = floor(i / B_size) 6  
Offset within Block (offset) = i % B_size 6  
Physical Address = PoolAddress[idx_block] * B_size + offset 6  
This indirection introduces a negligible overhead of a single memory lookup per block but reduces KV cache memory waste to less than 4% by eliminating external fragmentation and restricting internal fragmentation to the final block of a sequence.6

### **Sequence Scheduler Mechanics**

At every single forward pass (iteration-level execution), the scheduler must evaluate the state of the physical block pool to determine if the active batch can proceed.6 Due to the autoregressive nature of decoding, every running sequence requires an additional token allocation at each step.10 If a sequence crosses a block boundary (i.e., N_seq_len % B_size == 1), the scheduler must allocate a new physical block from the free queue.27  
If the free block queue is depleted and cannot satisfy the allocation demands of the active batch, the system faces an out-of-memory (OOM) condition.10 Rather than allowing a catastrophic hard-crash of the CUDA driver, the scheduler triggers preemptive eviction.5  
Because the key-value states of a sequence are tightly coupled, PagedAttention executes an "all-or-nothing" eviction policy.10 The scheduler selects active requests (typically starting from the lowest-priority, most recent arrivals under a First-Come, First-Served queue) and evicts their entire KV block set.5 The evicted sequence is transitioned to a suspended state and placed back into the scheduling queue.10  
Preemption is executed via one of two distinct strategies:

1. **Recompute**: The entire KV cache of the victim request is discarded.5 When GPU memory pressure eases, the request is re-admitted, and the system executes a full prefill pass from scratch to rebuild the KV cache.5 This avoids PCIe bandwidth overhead but consumes significant GPU compute cycles during re-admission.5  
2. **Swap**: The physical blocks of the victim request are serialized and copied from high-speed GPU HBM to host CPU DRAM over the PCIe bus.5 When the request is resumed, these blocks are transferred back to the GPU.9 While this preserves computed states, host-to-device (H2D) and device-to-host (D2H) copy latencies can introduce severe latency stalls (often hundreds of milliseconds over PCIe 4.0 x16 links).9

The core scheduler scheduling loop, depicting the iteration-level execution validation and preemption triggers, is outlined as follows 10:

```Python  
# Reference implementation based on vllm.core.scheduler._schedule()  
# Coordinates physical block allocation and checks preemption boundaries.

def schedule_iteration(self) -> SchedulerOutputs:  
    running_pool: List =  
    preempted_pool: List =  
      
    # Process running queue sequentially  
    while self.running_queue:  
        seq_group = self.running_queue.pop(0)  
          
        # Verify physical block availability for the next step   
        while not self.block_manager.can_append_slot(seq_group):  
            # Resolve physical block starvation via immediate preemption   
            if self.running_queue:  
                # Select the lowest-priority running sequence as eviction victim   
                victim = self.running_queue.pop(-1)  
                self._execute_preemption(victim, mode=self.preemption_config.mode)  
                preempted_pool.append(victim)  
            else:  
                # Evict the current sequence if no lower-priority victims remain   
                self._execute_preemption(seq_group, mode=self.preemption_config.mode)  
                preempted_pool.append(seq_group)  
                break  
        else:  
            # Allocate token slot and commit sequence to the active batch   
            self.block_manager.allocate_slot(seq_group)  
            running_pool.append(seq_group)  
              
    self.running_queue = running_pool  
    return SchedulerOutputs(running=running_pool, preempted=preempted_pool)
```

💠‍🌐 Here’s a corrected, drop-in version of that subsection. I tightened the mechanics and fixed the misleading “0-prefill” implication: a hit bypasses prefill **only for the matched prefix**, not necessarily for the entire request. I also made explicit that RadixAttention is an **exact token-prefix cache**, not semantic caching with a fancy tree hat. 

### **The Radix Tree Cache**

SGLang organizes prefix-cache metadata on the host CPU using a radix tree, also called a compressed trie. This mechanism, known as **RadixAttention**, stores shared prompt prefixes as paths through a token tree, with each node referencing the physical KV-cache blocks retained in GPU HBM.8 The tree does not cache “meaning.” It caches exact token-prefix structure. If the serialized prompt changes by even a small token-level difference—such as altered whitespace, reordered tool definitions, changed system text, or shifted variable placement—the request may fall onto a different path and lose reuse.

```
+--------------------------------------------------------------------------------------+
|                              RADIXATTENTION PREFIX CACHE                             |
+--------------------------------------------------------------------------------------+
|                                                                                      |
|  Stable prompt root                                                                  |
|  [System Instructions + Tool Schemas + Shared Template Prefix]                       |
|          |                                                                           |
|          |  KV blocks for this prefix are reused across matching requests            |
|          v                                                                           |
|  +------------------------------+                                                    |
|  | Root / Shared Prefix Node    |                                                    |
|  | token span: T0...Tn          |                                                    |
|  | KV refs: block_ids[...]      |                                                    |
|  | ref_count: active users      |                                                    |
|  +--------------+---------------+                                                    |
|                 |                                                                    |
|        +--------+-------------------------------+                                    |
|        |                                        |                                    |
|        v                                        v                                    |
|  +------------------------+              +------------------------+                  |
|  | Branch A: Shared Path  |              | Branch B: Shared Path  |                  |
|  | e.g. workflow/tool A   |              | e.g. workflow/tool B   |                  |
|  +-----------+------------+              +-----------+------------+                  |
|              |                                       |                               |
|       +------+-------+                       +-------+------+                        |
|       |              |                       |              |                        |
|       v              v                       v              v                        |
|  [User A1 suffix] [User A2 suffix]      [User B1 suffix] [User B2 suffix]            |
|  unique leaf     unique leaf            unique leaf     unique leaf                  |
|                                                                                      |
+--------------------------------------------------------------------------------------+
```

When a request is ingested, the scheduler walks the radix tree from the root, comparing the incoming token sequence against stored token spans.8 The result determines how much prefill computation can be avoided:

* **Full Prefix Hit:** The request’s stable prefix exactly matches an existing tree path whose KV blocks are still resident in GPU memory. The runtime locks the matched nodes, increments their reference counts, and bypasses prefill computation for that matched prefix. The dynamic suffix, if any, still requires prefill.

* **Partial Prefix Hit:** The request matches the tree for some initial token span but diverges at a later point, usually where user-specific content, retrieved evidence, or tool arguments begin. The runtime reuses the matched prefix KV blocks and computes prefill only for the unmatched suffix. The newly generated suffix blocks can then be appended as a new leaf.

* **Prefix Miss:** The request fails to match a reusable prefix path. The serving engine performs a normal prefill over the full compiled prompt and may insert the resulting KV blocks into the radix tree if cache policy, tenant scope, and capacity permit.

```
Incoming token sequence:

  [ Stable Prefix Tokens ][ Dynamic Suffix Tokens ]
           |                        |
           v                        v
  matched in radix tree        computed by prefill

Execution effect:

  Reuse cached KV blocks  +  Prefill only unmatched suffix
           |                        |
           +-----------+------------+
                       v
              [ Decode can begin ]
```

This structure is why prompt layout directly affects throughput. Static, high-reuse material—system instructions, tool schemas, output contracts, and long-lived templates—should be placed at the prompt root. Dynamic material—user messages, retrieved documents, transient memory, tool results—should be appended after the stable prefix. This maximizes the length of shared token paths and increases the `radix_prefix_match_length` telemetry signal.

The radix tree is ultimately bounded by the same HBM pool that stores active KV-cache blocks. When the physical block pool saturates, the runtime must evict cached nodes. SGLang-style runtimes use a leaf-first LRU policy: low-reuse leaf nodes, usually unique user-query suffixes, are evicted before high-reuse parent prefixes.14 Active nodes are protected by reference counts while live requests depend on them; once those counts fall to zero, the node becomes eligible for eviction according to policy.

```
+--------------------------------------------------------------------------------------+
|                             RADIX CACHE EVICTION LOGIC                               |
+--------------------------------------------------------------------------------------+
|                                                                                      |
|  Prefer to retain:                                                                    |
|      Root system prefix                                                               |
|      Shared tool schemas                                                              |
|      High-frequency workflow templates                                                |
|      Nodes with active reference counts                                               |
|                                                                                      |
|  Prefer to evict first:                                                               |
|      Unique user-query leaves                                                        |
|      Rare branches                                                                    |
|      Old low-access suffixes                                                          |
|      Unreferenced nodes after completion                                              |
|                                                                                      |
|  Practical rule: keep the trunk, shed the twigs.                                      |
|                                                                                      |
+--------------------------------------------------------------------------------------+
```

RadixAttention therefore functions as a physical throughput multiplier, not an abstract memory feature. It reduces redundant prefill work, lowers TTFT for repeated prompt structures, and improves GPU utilization—but only when upstream prompt serialization is stable. Prompt churn creates cache-miss storms; stable prompt roots create reusable KV highways.

## **Caching Strategy Matrix**

To minimize compute redundancy and physical transport bottlenecks, modern runtimes utilize a diverse array of caching layers. Each layer targets distinct performance gains while introducing specific structural, consistency, and security boundaries:

| Cache Class | Reused Resource | Computation Saved | Primary Invalidation Trigger | Security / Access Risk | Freshness Risk | Key Telemetry Indicator |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **Prompt Caching** 8 | Pre-computed KV state tensors of stable prompt prefixes.8 | Prefill forward pass compute (GEMM-heavy FLOPS).2 | Byte-level modification of the cached instruction string.8 | Low. Instructions are typically static and system-wide.8 | Low. Instantiated during system configuration releases. | prefix_cache_hit_ratio.9 |
| **Prefix Caching** 8 | Sub-sequences of common tokens (e.g., shared templates).8 | Prefill computations for the matched prefix token range.8 | Positional changes or variable insertions within the prefix path.8 | Low. Prefix templates are restricted to core system scopes.8 | Low. Updates are bound to structural release manifests.8 | radix_prefix_match_length.8 |
| **KV Cache Reuse** 15 | Session-specific KV tensors across multi-turn chats.15 | Full conversational history recalculation at each turn.20 | Exceeding session TTL; explicit thread termination events. | High. Cross-contamination risk between user sessions.8 | Low. Tightly coupled to the active execution state. | session_block_retention_rate. |
| **Semantic Caching** 15 | Complete natural language response strings.15 | Full forward passes of both prefill and decode stages.15 | Vector distance drift of incoming queries; explicit purges. | Critical. Risk of serving user A's private data to user B.22 | High. External knowledge base updates bypass the cache.16 | semantic_cache_hit_distance. |
| **Retrieval-Result Caching** 8 | Raw text or embeddings of dynamic fetched contexts.8 | Vector database query execution and index scan times. | Updates to the underlying vector index or document store.8 | High. Requires strict row-level security (RLS) enforcement. | High. Stale documents are served if indices update.8 | retrieval_cache_latency_saved. |
| **Tool-Result Caching** | Outputs of external functional executions (APIs, calculators). | Downstream API round-trip times and external platform costs. | Time-To-Live (TTL) expiration; system state transitions. | Medium. Cached tool outputs must respect authorization bounds. | High. Real-time APIs (e.g., stock charts) rot rapidly. | tool_api_avoidance_rate. |
| **Answer Caching** | Deterministic outputs of specific input hashes. | Full model evaluation; network and pipeline transport times. | Changes to prompt, system config, or temperature variables. | High. Output data must align with active user permission scopes. | High. Directly bypasses dynamic real-time lookups. | answer_cache_hit_ratio. |
| **Embedding Caching** | Dense vector representations of input text strings. | Host-to-device transfers; embedding model forward passes. | Character modification or normalization changes of input text. | Low. Vectors are abstract representations but contain information. | Low. Core embedding models are highly static. | embedding_hit_ratio. |
| **Compiled-Context Caching** | Structured context matrices ready for direct attention mapping.8 | Host-side tokenization and memory layout compilation times.20 | Updates to the underlying context-compiler layout logic.8 | Medium. Memory layouts must strictly segment tenant metadata. | Medium. Changes to reference bundles invalidate layouts.8 | context_compile_hit_ratio. |

### **The Tenure Principle and Cache Key Composition**

To guarantee that cached states do not bypass security controls, data freshness guarantees, or deployment boundaries, every cache entry must adhere to the **Tenure Principle**. Under this framework, cached data is never stored as an anonymous tensor or raw text fragment; it must carry a structured tenure wrapper that defines its execution context.  
The physical cache key is computed as a cryptographic hash of the input tokens concatenated with metadata representing the exact environment that produced the artifact:  
CacheKey = Hash(TokenIDs |  
| TenantID |  
| AccessPermissions |  
| ReleaseManifestID |  
| TemporalTTL)  
If an incoming request matches the exact TokenIDs but fails to satisfy the TenantID match or access controls, the cache hit is rejected. The request is routed to a clean computation path, preventing security boundaries from being bypassed by memory optimizations.

## **GPU Memory Pressure and CPU/GPU Tradeoffs**

The capacity of an accelerator memory system (such as HBM3/HBM3e) is the primary constraint on concurrent serving performance.1 To prevent out-of-memory crashes, VRAM must be segmented into distinct allocation domains.8

### **VRAM Memory Pressure Map**

The total allocated VRAM (V_allocated) of an active inference-serving GPU is defined by:  
V_allocated = V_weights + V_cache + V_activations + V_adapters + V_metadata + V_scratch 1

```
┌────────────────────────────────────────────────────────────────────────┐  
│ Total GPU HBM/VRAM Capacity                                            │  
├─────────────────┬─────────────────┬──────────┬──────────┬──────────────┤  
│ Model Weights   │ KV Cache Pool   │ Activ.   │ Adapters │ Runtime/     │  
│ (V_weights)     │ (V_cache)       │ (V_act)  │ (V_adap) │ CUDA Graphs  │  
│ Fixed footprint │ Dynamic allocation│ Dynamic  │ Dynamic  │ Overhead   │  
└─────────────────┴─────────────────┴──────────┴──────────┴──────────────┘
```

* **Model Weights (V_weights)**: The static memory footprint required to hold parameter matrices in memory.1 For example, a 70B parameter model demands a fixed allocation of 140 GB in FP16 (2 bytes/param) and 70 GB in FP8 (1 byte/param).1  
* **KV Cache Pool (V_cache)**: The dynamic pool pre-allocated by the serving engine during boot to prevent runtime CUDA allocation failures.8 It is managed directly by parameters like -mem-fraction-static.8  
* **Activations (V_activations)**: The temporary memory required to store intermediate layer activations during forward passes.21 It scales with batch size and layer width but is reclaimed immediately following kernel execution.21  
* **Adapters (V_adapters)**: VRAM allocated for parameter-efficient fine-tuning weights (e.g., LoRA checkpoints) loaded dynamically at runtime.26  
* **Runtime Overhead / CUDA Graphs (V_metadata)**: VRAM reserved by the serving engine (e.g., vLLM or SGLang) for metadata tracking, memory lookup tables, and pre-compiled execution graphs (CUDA Graphs).8  
* **Scratchpad Tensors (V_scratch)**: Transitory workspace buffers allocated for intermediate mathematical operations, such as token sorting or memory copying.

Because model weights are static, optimization focus targets the trade-off between weight precision and KV cache budget.1 If a 70B model is compressed from FP16 to FP8 on an A100 80GB GPU cluster (TP = 2, providing 160 GB aggregate VRAM), the weights footprint drops from 140 GB to 70 GB.1 This shift reclaims 70 GB of physical memory, directly expanding the KV cache pool from 15 GB (restricted to low concurrency) to 85 GB (supporting hundreds of concurrent sequences).1

### **CPU/GPU Tradeoff Model**

When serving models that exceed the local aggregate HBM pool, runtimes can offload segments to system DRAM or CXL storage tiers.9 This trade-off balances capacity against throughput across multiple hardware interconnect layers:

| Memory Tier | Physical Capacity | Peak Bandwidth | Interconnect Protocol | Latency Cost (Decode) | Systemic Role |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **GPU SRAM** | 10 - 250 MB | 10 - 50 TB/s | On-chip memory bus | Sub-nanosecond | L1/L2 caches; local registers for active thread blocks. |
| **GPU HBM** 1 | 80 - 192 GB 1 | 3.35 - 8.0 TB/s 1 | Co-packaged silicon interposer 1 | Baseline (1.0) 1 | Primary residency for weights, activations, and active KV cache.21 |
| **NVLink Interconnect** 8 | Multi-GPU unified pool | 900 GB/s 8 | Custom NVLink routing fabric 8 | 2x - 3x latency multiplier | Inter-GPU weight sharding (TP >= 2) and KV cache transfer.8 |
| **System DRAM (Host)** 9 | 512 GB - 4.0 TB | 100 - 300 GB/s | DDR5 memory bus | 20x - 40x latency multiplier | Primary residency for host metadata, tokenizers, and swapped cache.6 |
| **CXL Switched DRAM** 26 | 1.0 - 8.0 TB | 32 - 128 GB/s | Compute Express Link (CXL over PCIe) 26 | 40x - 100x latency multiplier | Ultra-large shared memory pools for cold-state KV caches.26 |
| **PCIe Bus Link** 8 | N/A | 32 - 64 GB/s (x16 link) 9 | PCIe Gen 4 / Gen 5 standard 9 | 50x - 120x latency multiplier | Host-to-Device transfer bridge; bottleneck for swapping operations.9 |

Offloading weight states or KV cache blocks to CPU DRAM or CXL storage expands context capacities but alters performance characteristics.9 Because host system DRAM bandwidth is up to thirty times slower than GPU HBM3, any step requiring data retrieval from host memory halts the GPU's execution pipeline.1 Offloading is highly effective for extending context size limits during slow asynchronous workloads, but it remains impractical for low-latency interactive serving.

## **Batching, Queueing, and Scheduler Behavior**

To amortize the high memory bandwidth cost of weight transfers, runtimes aggregate independent requests into parallel execution batches.1 However, the scheduling of these batches directly impacts system latency profiles and queueing stability.3

### **Batching Mechanics**

Traditional static batching processes a fixed set of requests simultaneously, running until the longest response completes.5 This approach introduces severe tail latency (P99) because short requests are blocked by long-running requests.9 Continuous batching (or iteration-level scheduling) resolves this by evaluating the batch at every token generation step.5 Completed requests are dropped immediately, and new requests are admitted from the queue.5

Static Batching:  
Batch Step 1 (Wait for slowest) ──►  
Batch Step 2                    ──► GPU sits idle waiting for Req B to complete...

Continuous Batching:  
Iteration 10  ──►  
Iteration 11  ──►  
Iteration 12  ──►

Under continuous batching, the scheduler manages sequence length heterogeneity and early finishes dynamically, keeping the GPU highly utilized.8

### **Queueing Theory and Capacity Sizing**

When designing inference fleets, arrival patterns are inherently stochastic, behaving as classical M/M/c queueing systems governed by the Erlang-C formula. The offered system load (rho) for a fleet of c GPUs with arrival rate lambda and mean service time S is modeled by:  
rho = (lambda * S) / c  
The probability that an incoming request is forced to wait in the ingestion queue is defined by:  
P(W_q > 0) = C(c, a) = ((a^c) / (c! * (1 - rho))) / (sum_{k=0}^{c-1} (a^k / k!) + (a^c) / (c! * (1 - rho))) where a = lambda * S  
The expected queue wait time (E) is:  
E = C(c, a) * S / (c * (1 - rho))  
The critical constraint is the (1 - rho) denominator. As system utilization approaches saturation (rho -> 1.0), expected queue latency scales twentyfold, violating latency SLAs without any change in the model's physical execution speed.

```
Expected Wait Time E (seconds)  
  ▲  
30│                                                  /  
25│                                                 /  
20│                                                /  
15│                                               /  
10│──────────────────────────────────────────────/── (10s SLO Threshold)  
 5│                                  ___________/  
 0└─────────────────────────────────┴───────────┴────►  
  0%                               80%         95%   System Utilization (rho)
```

If a fleet operates at 50% utilization, the queue wait time is minor. If a sudden demand spike shifts utilization to 95%, expected queue latency scales twentyfold, violating latency SLAs without any change in the model's physical execution speed.

### **Scheduler Policies as Product Decisions**

Scheduler policies directly shape user experience and system cost profiles. Schedulers must evaluate multiple priorities when dispatching execution batches:

* **First-Come, First-Served (FCFS)**: Admits requests sequentially.5 This approach is highly fair but prone to head-of-line blocking under mixed workloads.5  
* **Virtual Lag Time (VLT) / Largest-VLT-First**: Prioritizes requests at risk of violating latency SLAs. This optimization ensures SLA compliance but can degrade system throughput.  
* **Shortest Remaining Time First (SRTF) / Length Prediction**: Uses proxy models to predict sequence length, prioritizing short requests to minimize queue latency.17 This policy can starve long-context sequences under heavy load.

## **Capacity Planning Framework**

To size physical GPU capacity, architects must evaluate production traces using p50, p95, and p99 distributions rather than average workload metrics.

### **Sizing Methodology**

Let a multi-tenant production environment be characterized by three distinct workload profiles:  
T_mix = { w_conversational -> 70%, w_rag -> 20%, w_agent_loops -> 10% }  
The distribution attributes are:

* **Conversational (w_conversational)**: N_in_p95 = 1024, N_out_p95 = 256, average arrival rate lambda_1 = 30 req/sec.  
* **RAG Contexts (w_rag)**: N_in_p95 = 16384, N_out_p95 = 512, average arrival rate lambda_2 = 8 req/sec.  
* **Agent Loops (w_agent_loops)**: N_in_p95 = 32768, N_out_p95 = 128, average arrival rate lambda_3 = 2 req/sec.

The objective is to size an accelerator cluster to guarantee an ITL of t_step <= 25 ms under peak concurrent execution using the Llama-3-70B model in FP8 precision (b = 1).1

#### **Step 1: Calculate the Worst-Case Weight Footprint**

Choose an NVIDIA H200 SXM5 GPU (141 GB HBM3e, beta = 4.8 TB/s).1 The model weights footprint is:  
V_weights = 70B * 1 byte = 70 GB 1  
With 5 GB reserved for runtime APM metadata, the unallocated HBM capacity available for the dynamic block pool is:  
V_pool = 141 GB - 70 GB - 5 GB = 66 GB 1

#### **Step 2: Compute Dynamic Memory Demands**

Using the p95 sequence lengths, compute the memory requirement for each workload type:  
V_seq_conversational = 2 * 80 * 8 * 128 * (1024 + 256) * 1 = 209,715,200 bytes ~ 0.195 GB 1  
V_seq_rag = 2 * 80 * 8 * 128 * (16384 + 512) * 1 = 2,768,240,640 bytes ~ 2.578 GB 1  
V_seq_agent = 2 * 80 * 8 * 128 * (32768 + 128) * 1 = 5,389,680,640 bytes ~ 5.019 GB 1

#### **Step 3: Size Concurrency and Cluster Replication**

If the average concurrent active streams are B = 40 under peak loads, running all sequences on a single GPU is impossible because the memory requirements exceed physical HBM limits:  
V_total_required = (28 * 0.195) + (8 * 2.578) + (4 * 5.019) = 5.46 + 20.624 + 20.076 = 46.16 GB  
While the total memory requirement (46.16 GB) fits within the 66 GB pool, a sudden burst to p99 context lengths will exhaust HBM, triggering preemption and latency spikes.  
To maintain system stability, architects must size capacity using a 30% system margin budget (M_margin = 0.30):  
V_limit_safe = V_pool * (1 - M_margin) = 66 GB * 0.70 = 46.2 GB  
This indicates the GPU is operating at its exact capacity threshold. To handle arrival spikes and support failover operations, the cluster must be sized to scale horizontally:  
Replicas = ceiling(B_concurrency_peak / B_safe_limit) * (1 + Failover Overhead)  
Using TP = 4 over an NVLink interconnect provides 264 GB of dynamic block pool memory, allowing the fleet to absorb peak concurrent sequences with a secure capacity margin.8

## **Runtime Failure Mode Map**

Under high workload intensity, inference runtimes face unique failure modes driven by memory constraints and queueing dynamics.22 The following map details the symptoms, root causes, detection signals, and remediation strategies for these failures:

| Failure Mode | Structural Symptom | Immediate Root Cause | Detection Telemetry Signal | Smallest Effective Corrective Move | Long-Term Architectural Remedy |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Out-Of-Memory (OOM)** 8 | Engine process crashes; GPU driver throws memory allocation errors.8 | Transient activations or dynamic adapters exceed total VRAM capacity.12 | cudaErrorMemoryAllocation in logs; sharp drop in host processes. | Reduce -mem-fraction-static to free HBM capacity.8 | Enforce strict input context length validation; compile paths with static CUDA Graphs. |
| **KV Cache Exhaustion** 12 | Latency spikes; token generation stalls mid-sequence.5 | Concurrency of long-context requests exhausts the physical block pool.9 | free_block_queue count drops to zero; preemption alerts spike.12 | Dynamically lower -max-running-requests to throttle ingress.8 | Transition to GQA; configure sliding-window attention mechanisms. |
| **Cache Fragmentation** 6 | Batch capacity degrades; GPU utilization drops. | Allocation of non-contiguous variable memory chunks under naive managers.6 | Memory allocation retry flags; high external memory gaps. | Force a garbage collection sweep; restart the serving worker. | Deploy PagedAttention to virtualize cache memory into fixed blocks.6 |
| **Prefix-Cache Miss Storm** 8 | TTFT spikes across all ingress lanes.8 | Minor changes in instructions invalidate radix tree matches, forcing full prefills.8 | radix_cache_hit_rate drops to zero; prefill FLOPS spike.8 | Standardize and freeze prompt templates in the context compiler.8 | Implement exact-match token boundaries; locate static prefixes at prompt root.8 |
| **Semantic-Cache Staleness** | System serves incorrect, outdated, or low-quality responses. | Semantic similarity thresholds fail to capture temporal or contextual state drift. | Increased client-side error flags; user-driven downvotes. | Flush the vector index; increase the similarity distance threshold. | Apply the **Tenure Principle**; bind cache TTLs to source data release manifests. |
| **Queue Buildup** 3 | User-facing TTFT climbs linearly over time.5 | Arrival rate (lambda) consistently exceeds the service capacity (mu) of the active fleet. | queue_depth increases; W_q metrics rise exponentially. | Enable aggressive shedding of low-priority traffic. | Deploy dynamic horizontal autoscaling; scale replica counts based on queue latency. |
| **p95/p99 Latency Spikes** | Generation rates freeze; tail user latency spikes. | Compute-bound prefill phases block memory-bound decode steps in the batch.5 | ITL variance increases; p99_latency metrics spike. | Enable chunked prefill to interleave execution phases.8 | Disaggregate prefill and decode tasks into dedicated hardware nodes.31 |
| **Head-of-Line (HOL) Blocking** 5 | Short requests suffer high latency in ingestion buffers.22 | Long-context requests monopolize scheduler slots under simple FCFS policies.5 | queue_wait_time variance spikes; short-request latencies climb. | Reorder execution queues to prioritize short requests. | Transition to length-predictive scheduling (e.g., SRTF). |
| **Long-Context Starvation** 11 | Long requests remain pending; scheduler continuously postpones execution. | Scheduler prioritizes short requests to maintain low average latencies. | Long-context queue residence times spike; high drop-out rates. | Force-allocate a dedicated batch lane for long-context requests. | Implement weighted fair queueing (WFQ) with starvation prevention thresholds. |
| **Batch Inefficiency** 9 | GPU utilization drops under high traffic loads. | Pad token insertion under static batching wastes computational cycles.10 | Pad token ratio increases; GPU SM utilization falls. | Adjust batch timeout parameters. | Deploy continuous batching to manage variable-length inputs dynamically.5 |
| **Noisy-Neighbor Tenants** 22 | Specific tenant keys suffer latency spikes. | A single tenant exhausts the shared physical block pool with high-volume workloads.22 | VRAM allocation maps skew toward a single tenant identifier. | Throttle the abusive tenant's concurrency limits.8 | Implement tenant-isolated block managers and weighted fair queues.22 |
| **Retry Storms** | Ingress queue depths grow exponentially. | Client-side timeouts trigger rapid retries, compounding scheduler load. | Rate of duplicate request IDs in queue spikes. | Implement adaptive backpressure and return HTTP 503 codes.13 | Configure client-side exponential backoff with jitter. |
| **Streaming Stalls** 5 | Client generation pauses; output streams stutter. | Autoregressive decode steps are interrupted by large, non-chunked prefill processes.5 | inter_token_latency variance spikes mid-stream.2 | Enable chunked prefill.8 | Deploy prefill/decode disaggregation.31 |
| **Tool-Wait Amplification** | Complete agent execution loop times scale. | Synchronous tool execution stalls, holding KV cache blocks in VRAM. | Dynamic block locking times spike; high VRAM occupancy. | Swap inactive agent KV blocks to host DRAM during tool wait phases.9 | Implement asynchronous tool execution; release block tables during wait states. |
| **Adapter Memory Pressure** 26 | High latency during dynamic adapter loads; potential VRAM OOMs.26 | Loading multiple active LoRA adapters simultaneously exhausts available HBM.26 | Adapter load latencies spike; free block counts fall. | Pre-allocate dedicated memory slots for active adapters. | Implement unified multi-tenant adapter execution runtimes.26 |
| **CPU Offload Collapse** 9 | Generation rates drop; GPU utilization falls.9 | High host-to-device transfer overhead over saturated PCIe links.9 | PCIe bus utilization reaches 100%; H2D_transfer_latency spikes. | Throttle offloading triggers; restrict max sequence context limits.8 | Upgrade interconnect infrastructure to NVLink or high-bandwidth CXL networks.8 |
| **GPU Underutilization** 1 | High infrastructure costs with low operational throughput. | Low batch concurrency fails to saturate the GPU's memory bus. | GPU SM utilization is low; HBM bandwidth utilization is poor. | Increase dynamic batch sizes; consolidate workloads on fewer replicas.3 | Pool independent multi-tenant traffic to smooth arrival patterns. |
| **GPU Saturation** 1 | Expected queue latency spikes; TTFT climbs exponentially. | Arrival volume saturates the physical capacity of the active fleet. | SM utilization stays at 100%; queue depths grow. | Route overflow traffic to fallback cloud GPU nodes. | Provision additional replicas; implement strict rate-limiting policies. |
| **Backpressure Failure** 13 | System crashes; cascaded node failures occur. | System fails to throttle traffic when queues and memory saturate.13 | Memory allocation failures; container health checks fail. | Manually restart model service containers. | Configure automated admission controls based on active VRAM utilization.13 |

## **Runtime Observability Guidance**

To maintain high availability and prevent performance regressions, platform engineers must track metrics across three hardware and software execution domains:

```
                  ┌────────────────────────────────────────┐  
                  │      Serving Engine Observability      │  
                  └───────────────────┬────────────────────┘  
                                      │  
            ┌─────────────────────────┼─────────────────────────┐  
            ▼                         ▼                         ▼  
┌───────────────────────┐   ┌───────────────────────┐   ┌───────────────────────┐  
│   Hardware Metrics    │   │  Memory Pool Metrics  │   │  Latency/UX Metrics   │  
├───────────────────────┤   ├───────────────────────┤   ├───────────────────────┤  
│ • Effective MBU %     │   │ • Free Block Count    │   │ • TTFT (p50/p95/p99)  │  
│ • SM Utilization %    │   │ • Active Block Ref    │   │ • ITL/TPOT (p99)      │  
│ • PCIe/NVLink Bandw.  │   │ • Preemption Rate     │   │ • Queue Wait (W_q)    │  
└───────────────────────┘   └───────────────────────┘   └───────────────────────┘
```

The critical observability indicators required to monitor throughput health and diagnose performance bottlenecks are defined below:

| Domain | Telemetry Metric | Recommended Target Boundary | Physical Measurement Meaning | Diagnostic Utility |
| :---- | :---- | :---- | :---- | :---- |
| **Hardware** | **Effective Memory Bandwidth Utilization (MBU%)** 1 | 75% - 88% | The percentage of peak memory bandwidth actively utilized by the GPU memory controllers.1 | Low MBU% during decoding indicates scheduling overhead, small batch sizes, or PCIe bottlenecks.1 |
| **Hardware** | **SM Utilization (SM%)** 1 | > 85% (Prefill), < 15% (Decode) 1 | The percentage of active streaming multiprocessors executing GPU instructions.1 | High SM% combined with low MBU% indicates compute-bound prefill; low SM% with high MBU% confirms memory-bandwidth-bound decoding.1 |
| **Hardware** | **PCIe/NVLink Bandwidth Consumption** 8 | < 25% (PCIe), < 75% (NVLink) | The data transfer rate across physical interconnects.8 | Saturation indicates excessive swapping or inter-GPU synchronization bottlenecks.8 |
| **Memory** | **Free Block Queue Count** 22 | > 15% of total blocks 22 | The count of unallocated physical memory blocks in the PagedAttention allocator.22 | A downward trend indicates rising memory pressure and predicts imminent preemption.22 |
| **Memory** | **Active Block Reference Counts** 14 | > 1.2 average | The number of logical sequences mapped to a single physical memory block.21 | High reference counts confirm successful prefix caching and copy-on-write sharing.14 |
| **Memory** | **Preemption Rate** 5 | 0.0 req/sec | The frequency of sequence evictions (Swap or Recompute) per unit time.5 | Any value above zero indicates the system is operating beyond stable capacity limits.9 |
| **Latency/UX** | **Time-to-First-Token (TTFT)** 2 | < 200 ms (p95) 2 | The total elapsed time between request ingestion and the emission of the first token.2 | Spikes indicate queue delays, tokenization bottlenecks, or cold-cache prefill overhead.5 |
| **Latency/UX** | **Inter-Token Latency (ITL / TPOT)** 2 | < 20 ms (p99) 2 | The latency between successive output tokens during decoding.2 | Spikes indicate memory bus saturation, large concurrent batch sizes, or preemption events.1 |
| **Latency/UX** | **Queue Wait Time (W_q)** | < 50 ms (p95) | The duration a request spends in ingestion buffers before scheduler admission.3 | Spikes confirm system saturation and predict SLA violations. |
| **Throughput** | **Batch Concurrency (Active Sequences)** | 32 - 128 sequences 8 | The number of active sequences processed simultaneously in the execution batch. | Low concurrency indicates traffic starvation; high concurrency risks VRAM exhaustion and preemption.9 |
| **Throughput** | **Tokens Per Second (TPS)** | Target dependent on model size | The aggregate generation rate of the serving engine. | Drops in TPS under constant concurrency indicate hardware throttling or preemption overhead. |
| **Throughput** | **Requests Per Second (RPS)** | Target dependent on fleet size | The rate of incoming requests processed by the system. | Used to compute system utilization (rho) and guide capacity planning. |
| **Cache** | **Prefix Cache Hit Rate** 9 | > 40% | The percentage of prefill tokens served from cached RadixAttention nodes.8 | Low hit rates indicate unstructured prompt templates or excessive cache eviction.8 |
| **Cache** | **Eviction Rate** 14 | < 1.0 blocks/sec | The rate at which cached blocks are reclaimed to free physical memory.14 | High eviction rates indicate the active working context size exceeds physical HBM capacity.14 |
| **Robustness** | **Retry Rate** | < 1% | The frequency of client-initiated retries for failed requests. | Rising retry rates indicate system instability and can trigger queue collapse under load. |

## **Cross-Canon Handoff Map**

This report establishes the physical runtime and memory foundation for downstream architectural and operational systems detailed across *The AI Engineering Systems Canon*:

| Destination Report | Core Dependency | Functional Hand-off Mechanics |
| :---- | :---- | :---- |
| **AI-ENG-K (Weight Dynamics & Decoding Strategy)** | Prefill/Decode Cost Model 1 | Provides the latency and bandwidth cost equations used to evaluate quantization (FP8/FP4), speculative decoding, and constrained generation. |
| **AI-ENG-L (Model Serving Architecture)** | Capacity Planning & Queueing Models 3 | Informs replication, load balancing, autoscaling, and tenant-isolation routing configurations. |
| **AI-ENG-M (Agent Loop Budgets)** | Context Payload Constraints 1 | Sets physical limits on sequence lengths, dynamic iterations, and retries in autonomous agent loops. |
| **AI-ENG-N (Tool Latency & Contract Design)** | Prefix Caching Mechanics 8 | Informs the placement and serialization of tool schemas to maximize RadixAttention cache hits.8 |
| **AI-ENG-O (Action Verification Overhead)** | Constrained Generation Cost 15 | Quantifies the ITL performance penalty of structured FSM constraint parsing on decoding speed.15 |
| **AI-ENG-V (Resource Abuse & Cost Bombs)** | Queue Exhaustion & Failure Modes 22 | Establishes the physical profile of "Fill and Squeeze" resource exhaustion and security threats.22 |
| **AI-ENG-W (Fallback & Degraded Modes)** | Dynamic Preemption Thresholds 5 | Establishes the trigger parameters for fallback routing to compressed models or static templates when VRAM saturates. |
| **AI-ENG-Z (Telemetry & APM)** | Hardware & Memory Metrics 1 | Integrates physical hardware performance metrics into system APM dashboards. |
| **AI-ENG-AA (Runtime Evaluations)** | Trace Replay & Concurrency testing | Provides execution traces and queue metrics to validate performance under simulated production loads. |
| **AI-ENG-AC (Incident Response)** | Runtime Failure Mode Map 22 | Establishes diagnostics and playbook solutions for performance degradation incidents. |
| **AI-ENG-AE (Infrastructure Efficiency)** | Energy-per-token execution 1 | Maps memory access patterns to system power footprints, optimizing energy efficiency under load. |

## **Durable Principles**

### **I. Memory Governs Intelligence**

The throughput of an inference engine is constrained by how efficiently data moves through hardware, not by the model's parameter size.1 System optimization must focus on maximizing memory bandwidth utilization (MBU%) and minimizing cache fragmentation.1

### **II. Optimize Prefill and Decode Independently**

Prefill is compute-bound, while decode is memory-bandwidth-bound.1 Systems must separate these workloads using chunked prefill or dedicated hardware nodes to prevent large prefill steps from stalling concurrent decoding streams.8

### **III. Treat Every Token as a Physical Memory Allocation**

A sequence is not an abstract stream of text; it is a dynamic allocation of physical VRAM.6 Every generated token consumes space, and the scheduler must manage these block tables dynamically to prevent preemption loops and system crashes.10

### **IV. Enforce the Tenure Principle Securely**

Prefix caching and semantic caching are highly effective performance tools.8 However, every cached state must carry a strict tenure wrapper defining its tenant scope, temporal validity, and permissions to prevent security leaks across boundaries.

### **V. Queue Latency is Non-Linear**

System capacity must be sized for peak arrival variance rather than average workload volume. As system utilization approaches saturation, expected queue wait times climb exponentially, making high steady-state utilization a major risk to latency SLAs.

### **VI. Upstream Prompts Dictate Downstream System Stability**

Throughput failures often originate upstream in prompt design.8 Dynamic context payloads, varying templates, and unstructured formatting invalidate prefix caches, creating high prefill overhead and saturating hardware under load.8

#### **Works cited**

1. AI's Memory Wall Problem: Why More GPUs Don't Fix Inference ..., accessed June 8, 2026, [https://www.spheron.network/blog/ai-memory-wall-inference-latency-guide-2026/](https://www.spheron.network/blog/ai-memory-wall-inference-latency-guide-2026/)  
2. What Does “LLMs Are Memory Bandwidth Bound” Really Mean? | by ..., accessed June 8, 2026, [https://medium.com/@jiminlee-ai/what-does-llms-are-memory-bandwidth-bound-really-mean-4a1a57161b53](https://medium.com/@jiminlee-ai/what-does-llms-are-memory-bandwidth-bound-really-mean-4a1a57161b53)  
3. The Hidden Economics of LLM Inference | Hebbia, accessed June 8, 2026, [https://www.hebbia.com/blog/the-hidden-economics-of-llm-inference](https://www.hebbia.com/blog/the-hidden-economics-of-llm-inference)  
4. CMU CSD PhD Blog - Operator-Level Disaggregated Serving for Efficient LLM Inference, accessed June 8, 2026, [https://www.cs.cmu.edu/~csd-phd-blog/2026/operator-level-disaggregated-serving/](https://www.cs.cmu.edu/~csd-phd-blog/2026/operator-level-disaggregated-serving/)  
5. How vLLM Serves Thousands of Requests with Low Latency | by Arul Mathur - Medium, accessed June 8, 2026, [https://medium.com/understanding-llm-serving/how-vllm-serves-thousands-of-requests-with-low-latency-5ab2c513284d](https://medium.com/understanding-llm-serving/how-vllm-serves-thousands-of-requests-with-low-latency-5ab2c513284d)  
6. vLLM: How PagedAttention Solved the Memory Crisis in LLM Serving - Medium, accessed June 8, 2026, [https://medium.com/@serahabensour/vllm-how-pagedattention-solved-the-memory-crisis-in-llm-serving-61aece9fdf83](https://medium.com/@serahabensour/vllm-how-pagedattention-solved-the-memory-crisis-in-llm-serving-61aece9fdf83)  
7. Paged Attention: Turning the Page on Transformer Memory | by Sai Saketh - Towards AI, accessed June 8, 2026, [https://pub.towardsai.net/paged-attention-turning-the-page-on-transformer-memory-f394ca74e230](https://pub.towardsai.net/paged-attention-turning-the-page-on-transformer-memory-f394ca74e230)  
8. SGLang in Production: A Developer's Guide to Structured ... - Runpod, accessed June 8, 2026, [https://www.runpod.io/articles/guides/blog-sglang-production-llm-pipelines](https://www.runpod.io/articles/guides/blog-sglang-production-llm-pipelines)  
9. vLLM Explained: PagedAttention and Continuous Batching - Runpod, accessed June 8, 2026, [https://www.runpod.io/articles/guides/vllm-pagedattention-continuous-batching](https://www.runpod.io/articles/guides/vllm-pagedattention-continuous-batching)  
10. LLM Inference: Continuous Batching and PagedAttention · Better ..., accessed June 8, 2026, [https://insujang.github.io/2024-01-07/llm-inference-continuous-batching-and-pagedattention/](https://insujang.github.io/2024-01-07/llm-inference-continuous-batching-and-pagedattention/)  
11. Simulation Model Reference | vLLM Semantic Router, accessed June 8, 2026, [https://vllm-semantic-router.com/docs/v0.3/fleet-sim/sim-algorithms](https://vllm-semantic-router.com/docs/v0.3/fleet-sim/sim-algorithms)  
12. Optimization and Tuning - vLLM Documentation, accessed June 8, 2026, [https://docs.vllm.ai/en/v0.8.2/performance/optimization.html](https://docs.vllm.ai/en/v0.8.2/performance/optimization.html)  
13. Optimization and Tuning - vLLM - vLLM Documentation, accessed June 8, 2026, [https://docs.vllm.ai/en/stable/configuration/optimization/](https://docs.vllm.ai/en/stable/configuration/optimization/)  
14. RadixAttention - SGLang, accessed June 8, 2026, [https://sgl-project-sglang-93.mintlify.app/concepts/radix-attention](https://sgl-project-sglang-93.mintlify.app/concepts/radix-attention)  
15. SGLang: Efficient Execution of Structured Language Model Programs - NIPS, accessed June 8, 2026, [https://proceedings.neurips.cc/paper_files/paper/2024/file/724be4472168f31ba1c9ac630f15dec8-Paper-Conference.pdf](https://proceedings.neurips.cc/paper_files/paper/2024/file/724be4472168f31ba1c9ac630f15dec8-Paper-Conference.pdf)  
16. SGLang: Efficient Execution of Structured Language Model Programs - OpenReview, accessed June 8, 2026, [https://openreview.net/forum?id=VqkAKQibpq](https://openreview.net/forum?id=VqkAKQibpq)  
17. Orca: A Distributed Serving System for Transformer-Based Generative Models, accessed June 8, 2026, [https://www.semanticscholar.org/paper/Orca%3A-A-Distributed-Serving-System-for-Generative-Yu-Jeong/9d7a75601e0e50dd68d40cfb8ef0e891dad797a6](https://www.semanticscholar.org/paper/Orca%3A-A-Distributed-Serving-System-for-Generative-Yu-Jeong/9d7a75601e0e50dd68d40cfb8ef0e891dad797a6)  
18. Towards Multi-Model LLM Schedulers: Empirical Insights into Offloading and Preemption, accessed June 8, 2026, [https://arxiv.org/html/2605.19593v1](https://arxiv.org/html/2605.19593v1)  
19. SuperInfer: SLO-Aware Rotary Scheduling and Memory Management for LLM Inference on Superchips - arXiv, accessed June 8, 2026, [https://arxiv.org/html/2601.20309v2](https://arxiv.org/html/2601.20309v2)  
20. SGLang: The Complete Guide to High-Performance LLM Inference, accessed June 8, 2026, [https://inference.net/content/sglang-complete-guide/](https://inference.net/content/sglang-complete-guide/)  
21. Efficient Memory Management for Large Language Model Serving with PagedAttention - Zilliz Learn, accessed June 8, 2026, [https://zilliz.com/learn/efficient-memory-management-for-llm-serving-pagedattention](https://zilliz.com/learn/efficient-memory-management-for-llm-serving-pagedattention)  
22. Fill-Squeeze Scheduler DoS | LLM Security Database - Promptfoo, accessed June 8, 2026, [https://www.promptfoo.dev/lm-security-db/vuln/fill-squeeze-scheduler-dos-a3f62057](https://www.promptfoo.dev/lm-security-db/vuln/fill-squeeze-scheduler-dos-a3f62057)  
23. [2309.06180] Efficient Memory Management for Large Language Model Serving with PagedAttention - arXiv, accessed June 8, 2026, [https://arxiv.org/abs/2309.06180](https://arxiv.org/abs/2309.06180)  
24. RadixAttention Explained: How SGLang Beats PagedAttention at Scale - Rajat Pandit, accessed June 8, 2026, [https://rajatpandit.com/ai-engineering/radixattention-vs-pagedattention/](https://rajatpandit.com/ai-engineering/radixattention-vs-pagedattention/)  
25. My Understanding of vLLM and PagedAttention | by praveenreddy_c | May, 2026 | Medium, accessed June 8, 2026, [https://medium.com/@mailpraveenreddy.c/my-understanding-of-vllm-and-pagedattention-58cfafa30f3d](https://medium.com/@mailpraveenreddy.c/my-understanding-of-vllm-and-pagedattention-58cfafa30f3d)  
26. [PDF] Efficient Memory Management for Large Language Model Serving with PagedAttention | Semantic Scholar, accessed June 8, 2026, [https://www.semanticscholar.org/paper/Efficient-Memory-Management-for-Large-Language-with-Kwon-Li/83b90f4a0ae4cc214eb3cc140ccfef9cd99fac05](https://www.semanticscholar.org/paper/Efficient-Memory-Management-for-Large-Language-with-Kwon-Li/83b90f4a0ae4cc214eb3cc140ccfef9cd99fac05)  
27. Deep Dive into Efficient LLM Inference with nano-vLLM | Moncef Abboud, accessed June 8, 2026, [https://cefboud.com/posts/inside-llm-inference-engine-nano-vllm-explanation/](https://cefboud.com/posts/inside-llm-inference-engine-nano-vllm-explanation/)  
28. Inside SGLang: LMSys New Framework for Super Fast LLM Inference | by Jesus Rodriguez, accessed June 8, 2026, [https://jrodthoughts.medium.com/inside-sglang-lmsys-new-framework-for-super-fast-llm-inference-77e67b8933ce](https://jrodthoughts.medium.com/inside-sglang-lmsys-new-framework-for-super-fast-llm-inference-77e67b8933ce)  
29. AI-research-SKILLs/12-inference-serving/sglang/references/radix-attention.md at main, accessed June 8, 2026, [https://github.com/firecrawl/ai-research-skills/blob/main/12-inference-serving/sglang/references/radix-attention.md](https://github.com/firecrawl/ai-research-skills/blob/main/12-inference-serving/sglang/references/radix-attention.md)  
30. Paged Attention from First Principles: A View Inside vLLM | Hamza's Blog, accessed June 8, 2026, [https://hamzaelshafie.bearblog.dev/paged-attention-from-first-principles-a-view-inside-vllm/](https://hamzaelshafie.bearblog.dev/paged-attention-from-first-principles-a-view-inside-vllm/)  
31. GitHub - sgl-project/sglang: SGLang is a high-performance serving framework for large language models and multimodal models., accessed June 8, 2026, [https://github.com/sgl-project/sglang](https://github.com/sgl-project/sglang)

---

[← Back to Canon Map](../canon-map.md)