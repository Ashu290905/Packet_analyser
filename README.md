# DPI Engine  
## Multi-Threaded Deep Packet Inspection Engine (C++17)

A performance-oriented Deep Packet Inspection engine built in modern C++ (C++17). The system parses PCAP files, extracts TLS Server Name Indication (SNI) metadata from ClientHello handshakes, and reconstructs packet streams using a multi-threaded processing architecture.

The project focuses on low-level protocol parsing, concurrency design, and predictable performance under load.

---

## Overview

Given an input PCAP file, the engine:

1. Streams packets from disk  
2. Parses Ethernet, IPv4, and TCP headers manually  
3. Identifies TLS ClientHello records  
4. Extracts SNI from TLS extensions  
5. Writes processed packets to a new PCAP  

The architecture mirrors the core mechanics used in domain-level inspection systems powering firewalls, traffic filtering platforms, and monitoring infrastructure.

---

## Engineering Approach

This project was intentionally built without high-level networking abstractions.

### Design Priorities

- Manual protocol decoding (Ethernet, IPv4, TCP, TLS)  
- Explicit memory and buffer management  
- Multi-threaded workload distribution  
- Reduced synchronization overhead  
- Modular separation of parsing, concurrency, and I/O  

The concurrency model distributes packet processing across worker threads to maximize CPU utilization while minimizing contention. The implementation avoids unnecessary copying and keeps parsing logic close to the underlying byte layout.

The objective was correctness, modularity, and performance — not just functional output.

---

## Architecture

```text
PCAP Reader → Packet Parser → TLS SNI Extractor → Output Writer

Modules

File	Responsibility
dpi_mt.cpp	Thread orchestration and task scheduling
pcap_reader.cpp	PCAP ingestion and streaming
packet_parser.cpp	Ethernet/IP/TCP decoding
sni_extractor.cpp	TLS ClientHello parsing and SNI extraction
types.cpp	Shared protocol structures

Each component is isolated to maintain clean boundaries between concurrency control and protocol parsing.
```

---

## Build
 ### macOS / Linux:
```bash
clang++ -std=c++17 -O2 -I include \
    src/dpi_mt.cpp \
    src/pcap_reader.cpp \
    src/packet_parser.cpp \
    src/sni_extractor.cpp \
    src/types.cpp \
    -lpcap \
    -o dpi_engine
```
Run:
```bash
./dpi_engine input.pcap output.pcap
```


### Windows (MSVC)
```cmd
cl /EHsc /std:c++17 /O2 /I include /Fe:dpi_engine.exe ^
    src\dpi_mt.cpp ^
    src\pcap_reader.cpp ^
    src\packet_parser.cpp ^
    src\sni_extractor.cpp ^
    src\types.cpp
```
Run:
```cmd
dpi_engine.exe input.pcap output.pcap
```
---

### What This Project Demonstrates
	•	Systems-level protocol reasoning
	•	Thread coordination and workload partitioning
	•	Performance-aware C++ design
	•	Clean modular architecture in low-level systems code
	•	Practical understanding of traffic inspection mechanics

