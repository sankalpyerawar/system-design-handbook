# Latency Numbers Every Programmer Should Know

**Source:** [Jeff Dean / Peter Norvig (via jboner Gist)](https://gist.github.com/jboner/2841832)

## The Numbers (~2012)
These numbers provide a sense of scale for various system operations. While absolute hardware speeds change, the **relative ratios** usually remain similar.

| Action | Time (ns) | Time (us/ms) | Relative Scale |
| :--- | :--- | :--- | :--- |
| **L1 cache reference** | 0.5 ns | | **1x** (Baseline) |
| **Branch mispredict** | 5 ns | | 10x |
| **L2 cache reference** | 7 ns | | 14x |
| **Mutex lock/unlock** | 25 ns | | 50x |
| **Main memory reference** | 100 ns | | 200x |
| **Compress 1K bytes with Zippy** | 3,000 ns | 3 us | |
| **Send 1K bytes over 1 Gbps network** | 10,000 ns | 10 us | |
| **Read 4K randomly from SSD** | 150,000 ns | 150 us | ~300x Memory |
| **Read 1 MB sequentially from memory** | 250,000 ns | 250 us | |
| **Round trip within same datacenter** | 500,000 ns | 500 us | |
| **Read 1 MB sequentially from SSD** | 1,000,000 ns | 1 ms | 4x Memory Read |
| **Disk seek** | 10,000,000 ns | 10 ms | 20x Datacenter RTT |
| **Read 1 MB sequentially from disk** | 20,000,000 ns | 20 ms | 80x Memory Read |
| **Send packet CA -> Netherlands -> CA** | 150,000,000 ns | 150 ms | |

*Note: 1 ns = 10^-9 s | 1 us = 10^-6 s | 1 ms = 10^-3 s*

## What This Means (Analysis)

Understanding these orders of magnitude is critical for system design:

### 1. The "Memory Wall"
*   Accessing **Main Memory (RAM)** is ~200x slower than L1 cache.
*   **Implication**: Algorithms should be "cache-friendly". Sequential access (predictable pre-fetching) is significantly faster than random pointer chasing.

### 2. The Cost of I/O (Input/Output)
*   **Disk (HDD) is glacial**: A disk seek (10 ms) is **20,000,000x** slower than an L1 cache reference.
*   **SSD vs HDD**: SSDs are vastly faster than HDDs, but still much slower than RAM.
*   **Implication**: Avoid disk I/O in the hot path of your application. Use **Caching** (Redis/Memcached) to keep hot data in RAM.

### 3. Network Latency
*   **Datacenter Round Trip**: ~0.5 ms. This is expensive compared to local operations.
*   **Cross-Region**: ~150 ms. Speed of light limits signal transmission.
*   **Implication**: Minimize network calls. Batch requests together. In distributed systems, prefer local processing or availability zones closer to the user to reduce latency.

### 4. Sequential vs. Random Access
*   Reading memory sequentially (scanning an array) is faster than random access (following a linked list) due to CPU prefetching.
*   Reading disk sequentially is **significantly** faster than random seeks.
*   **Implication**: Append-only logs (like in Kafka or Database WALs) are fast because they use sequential write patterns.

## Human Scale Visualization
If **1 CPU cycle (0.3ns)** was **1 second**:

*   **L1 Cache Access**: ~1 second
*   **Main Memory Access**: ~4-6 minutes
*   **Solid State Disk (SSD) IO**: ~2-6 days
*   **Rotational Disk (HDD) IO**: ~1-12 months
*   **Internet (SF to NYC)**: ~4 years
*   **Internet (SF to Australia)**: ~8 years

---

## Q1: What should developers consider while writing algorithms?
Efficiency is often determined by how an algorithm interacts with memory, processors, and networks. Key considerations include:

### 1. The Latency Hierarchy (Data Locality)
*   **Cache Efficiency**: Accessing **L1 cache (0.5 ns)** is 200x faster than **main memory (100 ns)**. Algorithms that maximize "data locality" (keeping data in CPU cache) outperform those that frequently access RAM.
*   **Memory vs. Disk**: Disk-bound algorithms are orders of magnitude slower than memory-bound ones.
    *   *Memory Read (1MB)*: ~250 µs
    *   *SSD Read (1MB)*: ~1,000 µs (1 ms)
    *   *Disk Read (1MB)*: ~20,000 µs (20 ms)

### 2. Branch Predictability
*   Modern processors predict execution paths. A **branch mispredict (~5 ns)** is 10x costlier than an L1 cache reference.
*   **Tip**: Write predictable logic or "branchless" code to avoid wasting CPU cycles on incorrect paths.

### 3. Concurrency and Synchronization
*   **Locking**: A simple **mutex lock/unlock** takes ~25 ns.
*   **Context Switching**: Moving between threads/processes takes **100 ns - 6 µs**.
*   **Tip**: Excessive locking or switching can negate parallelization benefits.

### 4. Network and I/O Bottlenecks
*   The network is often the primary bottleneck.
*   *Datacenter RTT*: 0.5 ms
*   *Cross-Continent RTT*: 150 ms
*   **Tip**: Distinguish between **throughput** (pipe size) and **latency** (travel time). High bandwidth doesn't solve high latency.

### 5. Contention and Queuing
*   Ideal numbers assume isolation. in reality, processes compete for CPU/RAM/Threads.
*   **Tip**: Latency spikes under load due to queuing effects (waiting for resources).

### 6. Compiler Optimizations
*   Compilers might optimize out "useless" code during benchmarks.
*   **Tip**: Ensure benchmarks actually use the computed results to prevent code deletion.

> **Analogy**:
> *   **L1 Cache**: Picking up a pen on your desk.
> *   **RAM**: Walking down the hall to get a pen.
> *   **Datacenter RTT**: A 94-mile hike.
> *   **Global RTT**: Walking around the Earth.
>
> *Goal: Keep work on your desk (Cache) rather than hiking (Network/Disk).*

---

## Q2: How can developers maximize L1 cache hits?
To maximize L1 cache hits (and performance), focus on **Data Locality** and **Predictability**:

*   **Sequential Access**: Access memory linearly (e.g., arrays) rather than randomly. CPU prefetchers can predict and load optimal data chunks.
    *   *Good*: `for(i=0; i<N; i++) array[i]`
    *   *Bad*: `for(i=0; i<N; i++) array[random_index[i]]`
*   **Compact Data Structures**: Pack data tightly. Smaller objects mean more fit in the cache.
    *   *Technique*: Use **Struct of Arrays (SoA)** instead of Array of Structures (AoS) if you iterate over single fields frequently.
*   **Avoid Pointer Chasing**: Linked Lists and Trees often point to scattered memory locations, causing frequent cache misses. Prefer contiguous memory containers like `std::vector` or arrays.
*   **Data Alignment**: Align data structures to cache line boundaries (typically 64 bytes) to prevent a single object from straddling two cache lines.
*   **Loop Tiling (Blocking)**: For matrix operations, process small sub-blocks (tiles) that fit entirely in the cache before moving to the next.

---
**References:**
*   [Original Gist by jboner](https://gist.github.com/jboner/2841832)
*   [Jeff Dean's Numbers](http://research.google.com/people/jeff/)
