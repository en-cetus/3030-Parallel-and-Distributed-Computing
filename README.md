# Parallel N-Body Gravitational Simulator (MPI)

**Course:** COMP_3030 Parallel and Distributed Computing
**Author:** TianYang Wang
**Student ID:** a1947653
**Private Repository Link:** https://github.com/en-cetus/3030-Parallel-and-Distributed-Computing

---

## Overview

This repository contains the complete implementation for Assignment Part 1 of the parallel 2D N-body gravitational simulator using MPI.

- `part1a.c`: Independent, complete implementation of Part 1A replacing `MPI_Allgather` with a point-to-point ring communication topology (`MPI_Sendrecv`).
- `part1b.c`: Independent, complete implementation of Part 1B eliminating global arrays to achieve a true $O(N/P)$ memory allocation per rank.

---

## Compilation Instructions

Both source files are fully self-contained and can be compiled independently using `mpicc`.

### 1. Standard Production Build

```bash
# Compile Part 1A
mpicc -g -Wall -o part1a part1a.c -lm

# Compile Part 1B
mpicc -g -Wall -o part1b part1b.c -lm

```

### 2. Unit Testing Build (-DDEBUG_TEST)

Both implementations include self-contained unit tests. Compile with `-DDEBUG_TEST` to run automated verification:

```bash
# Compile and run Part 1A Unit Tests
mpicc -g -Wall -DDEBUG_TEST -o test_part1a part1a.c -lm
mpiexec -n 4 ./test_part1a 12 1 0.01 1 g

# Compile and run Part 1B Unit Tests
mpicc -g -Wall -DDEBUG_TEST -o test_part1b part1b.c -lm
mpiexec -n 4 ./test_part1b 12 1 0.01 1 g

```

---

## Execution Instructions

Run the compiled binaries using `mpiexec` with the required command-line arguments:

```bash
mpiexec -n <number_of_processes> ./<binary_name> <n_particles> <n_steps> <delta_t> <output_freq> <g|i>

```

### Example Execution Commands:

```bash
# Run Part 1A with 4 processes, 12 particles, 100 timesteps
mpiexec -n 4 ./part1a 12 100 0.01 1 g

# Run Part 1B with 4 processes, 12 particles, 100 timesteps
mpiexec -n 4 ./part1b 12 100 0.01 1 g

```

---

## Correctness & Validation Evidence

To verify correctness, output states from the baseline solver (`mpi_nbody_basic`), `part1a`, and `part1b` were redirected and compared:

```bash
# Generate output logs
mpiexec -n 4 ./mpi_nbody_basic 12 100 0.01 1 g > baseline_out.txt
mpiexec -n 4 ./part1a 12 100 0.01 1 g > part1a_out.txt
mpiexec -n 4 ./part1b 12 100 0.01 1 g > part1b_out.txt

# Verify numerical alignment
diff -u baseline_out.txt part1a_out.txt
diff -u baseline_out.txt part1b_out.txt

```
