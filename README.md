# Mitigating Cache Side-Channel Attacks via Dynamic Cache Partitioning

This project implements dynamic cache partitioning techniques for the Last-Level Cache (LLC) to defend against various cache side-channel attacks including conflict-based, occupancy-based, and timing-based attacks. The implementation uses the ChampSim trace-based simulator to evaluate the effectiveness of different partitioning strategies.

## Overview

Cache side-channel attacks exploit shared cache resources to leak sensitive information between processes. This project addresses these vulnerabilities through:

- **Dynamic Cache Partitioning**: Runtime allocation of cache resources based on process requirements
- **Hash Function**: Dynamic remapping scheme for LLC sets to processes
- **Multi-Core Support**: Evaluation across multi-core environments with SPEC CPU benchmarks
- **Performance Analysis**: Assessment of isolation effectiveness vs. performance overhead

## Features

### Implemented Mitigation Techniques

1. **Way Partitioning**: Divides cache ways among different processes
2. **Static Set Partitioning**: Fixed allocation of cache sets to processes
3. **Dynamic Set Partitioning**: Runtime-adaptive set allocation based on workload characteristics

### Attack Defense Coverage

- **Conflict-based attacks**: Prevents attackers from evicting victim's cache lines
- **Occupancy-based attacks**: Limits cache occupancy information leakage
- **Timing-based attacks**: Reduces timing side-channel vulnerabilities

## Prerequisites

### System Requirements

- **GCC 7**: Required for ChampSim compilation
- **Ubuntu/Linux**: Recommended development environment
- **Multi-core system**: For realistic evaluation scenarios

### Installing GCC 7

```bash
sudo apt update 
sudo add-apt-repository ppa:ubuntu-toolchain-r/test 
sudo nano /etc/apt/sources.list
```

Update the last line with:
```
deb [arch=amd64] http://archive.ubuntu.com/ubuntu focal main universe
```

Install GCC 7:
```bash
sudo add-apt-repository ppa:ubuntu-toolchain-r/test 
sudo apt-get install gcc-7 
sudo apt-get install g++-7 

sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 0 
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 0
```

Configure alternatives (if needed):
```bash
sudo update-alternatives --config g++ 
sudo update-alternatives --config gcc
```

## Building the Project

The project uses the ChampSim build system with custom LLC replacement policies for cache partitioning.

### Basic Build

For standard LRU with way partitioning:
```bash
./build_champsim.sh lru_wp 2
```

For standard LRU (baseline):
```bash
./build_champsim.sh lru 2
```

### Build Parameters

```bash
./build_champsim.sh ${LLC_REPLACEMENT} ${NUM_CORES}
```

- `LLC_REPLACEMENT`: Replacement policy (`lru`, `lru_wp` for way partitioning)
- `NUM_CORES`: Number of cores (use `2` for dual-core evaluation)

## Configuration Options

### Partitioning Strategies

The implementation supports three partitioning approaches controlled by compile-time flags in `cache.cc`:

#### 1. Way Partitioning
```bash
./build_champsim.sh lru_wp 2
```

#### 2. Static Set Partitioning
Uncomment in `cache.cc`:
```cpp
#define STATIC_SET_PARTIONING_ACTIVE 1
```

#### 3. Dynamic Set Partitioning
Uncomment in `cache.cc`:
```cpp
#define DYNAMIC_SET_PARTIONING_ACTIVE 1
```

**Note**: Only enable one partitioning strategy at a time.

## Running Simulations

### Two-Core Evaluation

Use the provided script for dual-core workload evaluation:

```bash
./run_2core.sh <binary_path> <workload1_id> <workload2_id>
```

**Example**:
```bash
./run_2core.sh bin/lru_wp-2core 1 2
```

### Script Parameters

- `binary_path`: Path to the compiled ChampSim binary
- `workload1_id`: Workload number for CPU 1 (from `workloads.txt`)
- `workload2_id`: Workload number for CPU 2 (from `workloads.txt`)

### Simulation Configuration

- **Warmup Instructions**: 50M instructions
- **Simulation Instructions**: 50M instructions
- **Output**: Results stored in `results/mix-{workload1}-{workload2}.txt`


## References

- ChampSim Simulator: Trace-based microarchitecture simulation
- SPEC CPU Benchmarks: Standard performance evaluation suite
- Cache Side-Channel Attack Literature: Security vulnerability research

## License

This project extends the ChampSim simulator for academic research purposes. Please refer to the original ChampSim license for usage terms.