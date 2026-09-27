# Improving Parallel Scalability in Web Server Architectures

A research proposal and architectural blueprint focused on resolving memory contention and thread management bottlenecks in high-concurrency web servers (such as Apache).

---

## 📌 Overview

Traditional thread-per-request and process-per-request web server architectures experience significant latency spikes and performance degradation as concurrent connections scale into the thousands. This degradation is primarily driven by:
- **Lock Contention:** Threads competing for shared global request queues.
- **Shared Memory Bottlenecks:** Multiple worker threads concurrently accessing shared cache/memory spaces.
- **Context-Switching Overhead:** Excessive kernel context switches under heavy I/O.

This project outlines an optimized architectural framework designed to handle **1,000+ concurrent connections** while maintaining average response times **under 100 ms**.

---

## ⚙️ Key Architectural Solutions

1. **Lock-Free Request Queues**
   - Eliminates standard mutual-exclusion locking primitives for handling incoming requests.
   - Allows worker threads to dequeue tasks independently, reducing synchronization bottlenecks.

2. **Per-Thread Memory Queues**
   - Assigns dedicated, thread-local memory caches and request buffers.
   - Eliminates shared-memory contention across parallel execution threads.

3. **Asynchronous Non-Blocking I/O & Batching**
   - Leverages modern multiplexing engines (`epoll` for Linux, `kqueue` for BSD/macOS).
   - Utilizes I/O batching to process socket events collectively, slashing kernel context-switching costs.

---

## 📊 Projected Performance Metrics

| Metric | Baseline | Target with Proposed Architecture | Basis / Rationale |
| :--- | :--- | :--- | :--- |
| **Concurrent Users** | Degrades at scale | **1,000+ concurrent connections** | Benchmarked against high-concurrency event loops |
| **Response Latency** | ~250 ms | **< 100 ms** | Diminished memory contention and lock wait times |
| **Thread Contention** | High | **~40% reduction** | Lock-free data structures |
| **Energy Consumption** | Baseline | **~30% reduction** | Reduced idle spinning, fewer locking cycles, and memory access optimization |

---

## ⚖️ Trade-offs & Considerations

- **Memory Management Complexity:** Individual per-thread memory allocation requires custom memory tracking (e.g., leveraging allocators like `jemalloc`) to prevent memory fragmentation and leakage.
- **Latency vs. Batching Balance:** While I/O batching maximizes throughput, overly aggressive batch sizes can introduce slight micro-delays in real-time/interactive requests.

---

## 📚 References

1. **Hennessy, J., & Patterson, D. (2017).** *Computer Organization and Design: The RISC-V Edition (6th ed.)*. Morgan Kaufmann.
2. **Anderson, D., & Velásquez, S. (2016).** *Improving web server performance using concurrency models: A study of how lock-free data structures and memory queues affect performance*. Journal of Web Engineering.
3. **Zhou, Y., & Guo, J. (2018).** *Improving the scalability of multi-threaded servers requires memory management that considers parallelism*. International Journal of Cloud Computing and Services Science.
4. **Linux Documentation.** *`epoll(7)` — Linux manual page*.
5. **Apple Developer Documentation.** *`kqueue(2)` — Apple Developer Documentation*.
6. **NGINX, Inc. (2020).** *Fine-tuning NGINX for peak performance*.

---

## 👤 Author & Acknowledgments

- **Developer:** [Sammy6899](https://github.com/Sammy6899)
- **Course:** CSE340 - Computer Architecture
