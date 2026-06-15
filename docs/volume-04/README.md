# Volume 4 — Runtime Architecture and Inference Mechanics

*How AI systems actually execute under physical, computational, and operational constraints.*

## Reports

### [AI-ENG-J — Throughput Mechanics: Memory Pressure, KV Cache & Hardware Realities](./ai-eng-j-throughput-mechanics.md) 
    Covers the brutal physics of scaling inference. Includes prefill vs. decode latency, KV cache management, eviction policies, prompt caching, semantic caching, continuous batching, paged attention, GPU memory pressure, CPU/GPU tradeoffs, and capacity planning under real workloads.

### [AI-ENG-K — Weight Dynamics: Quantization, Compression & Decoding Strategy](./ai-eng-k-weight-dynamics.md)
    Covers INT8, INT4, FP8, AWQ, GPTQ, pruning, speculative decoding, draft models, constrained decoding, sampling behavior, and quality degradation boundaries. Teaches how to make models faster and cheaper without quietly destroying their reasoning, formatting, or instruction-following reliability.

### [AI-ENG-L — Model Serving Architecture: Routing, Load Balancing & Deployment Topologies](./ai-eng-l-model-serving-architecture.md)
    Covers inference servers, API gateways, local deployment, edge deployment, cloud deployment, multi-model routing, tenant-aware capacity, queueing, backpressure, autoscaling, cold starts, rate limits, and high-availability serving patterns.

[← Back to Canon Map](../canon-map.md)