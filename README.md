# VSC Core (Virtual Storage Connection)

A lightweight, ultra-high-performance, asynchronous **C++23 Data Access Layer (DAL) core**. Strictly decoupled from platform-specific thread models and database drivers. Built as a reusable component for **Qt 6 / QML**, **Unreal Engine 5**, and high-load backend architectures.

---

## 🚀 Architectural Pillars

*   **Zero-Allocation Data Pipeline (`std::pmr`)**
    All rows and data cells are read sequentially into a localized monotonic memory arena (`std::pmr::monotonic_buffer_resource`). Eliminates runtime OS heap fragmentation and `malloc` overhead during massive table fetching.
*   **Thread-Agnostic Scheduling (`ITaskScheduler`)**
    Contains zero raw threads or framework-specific dependencies. Dispatches background database tasks directly onto the hosting application’s threads (e.g., `QThreadPool` for Qt or `UE::Tasks` for Unreal Engine).
*   **Exception-Free Pipeline (`std::expected`)**
    Propagates monadic results and errors downstream without throwing heavy runtime exceptions. Fully compatible with non-RTTI / non-exceptions engine build configurations.
*   **Deterministic Thread-Affinity Guarding**
    Binds active transaction scopes to explicit connection workers via `workerId` tokens to protect thread-sensitive client database handles (e.g., SQLite).
*   **Hardened RAII Rollbacks**
    Tracked via an atomic lock-free state machine. Abandoned or uncommitted transactions automatically trigger a safe, non-blocking asynchronous `ROLLBACK` sequence upon scope destruction.

---

## 📂 Layout Overview

### Data Containers
*   `types.h` — Defines PMR typings (`std::pmr::string`, `std::pmr::vector`).
*   `value.h` — PMR cell variant (`std::variant`) supporting integers, real numbers, strings, blobs, and `Decimal` types.
*   `query.h` / `CmdQuery` — Encapsulates raw SQL strings and bound parameters.
*   `result_set.h` / `ResultSet` — Move-only, cache-friendly data matrix with a fixed arena allocator lifespan.

### Abstraction Interfaces
*   `i_connection.h` / `i_connection_factory.h` — Base driver and driver-factory contracts.
*   `i_transaction.h` — Basic transactional context boundary.

### Engine Mechanics
*   `i_task_scheduler.h` — Abstract frame-safe background worker offloader.
*   `transaction_impl.h` — Strong RAII concrete transaction coordinator.
*   `vsc_engine.h` / `VscEngine` — High-level async orchestrator dispatching query promises into standard `std::future` handles.

---

## 🛠️ Requirements
*   **Compiler:** C++23 toolchain (GCC 13+, Clang 16+, MSVC 19.36+).
*   **Build System:** CMake 3.22+.
