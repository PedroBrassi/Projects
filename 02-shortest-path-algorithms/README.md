# Shortest Path Algorithms — Dijkstra vs. Bellman-Ford vs. Duan (2025)

**Course:** Algorithms and Data Structures III (AEDS III) — UNIFAL-MG
**Professor:** Iago A. Carvalho
**Language:** C

This project implements and experimentally compares algorithms for the
**Single Source Shortest Path (SSSP)** problem — computing the shortest path
from a source vertex to every other vertex in a weighted graph.

Algorithms implemented:

- **Dijkstra**
- **Bellman-Ford**
- **Duan (2025)** — a version inspired by a deterministic algorithm based on
  frontier reduction and local relaxation

In addition to computing shortest paths, the project measures **execution
time**, enabling an experimental comparison between algorithms.

---

## Project Structure

```txt
.
├── base.c               # Main program
├── algoritmos.c          # Algorithm implementations
├── algoritmos.h          # Function prototypes
├── Makefile              # Build automation
├── run_all.sh            # Benchmark runner script
├── data/                 # Test graph instances (.txt)
├── results.txt            # Benchmark results
└── README.md
```

---

## Algorithms

### 1. Dijkstra

Computes shortest paths on graphs with non-negative weights.

- graph represented as an **adjacency matrix**
- uses linear selection of the minimum-distance vertex (**no heap**)
- approximate complexity:

```math
O(n^2)
```

### 2. Bellman-Ford

Performs successive relaxations over all edges of the graph.

- supports negative weights (no negative cycles)
- multiple global iterations
- early termination when no updates occur

Complexity (adjacency matrix representation):

```math
O(n^3)
```

### 3. Duan (2025)

A version inspired by the deterministic algorithm proposed by **Duan et al.
(2025)**, using:

- controlled local relaxation (Bellman-Ford-like)
- exploration frontier reduction
- pivot selection
- local refinement similar to **BMSSP**
- termination based on local vertex ordering

Core parameters:

```math
k = \lfloor \log^{1/3}(n) \rfloor
```

```math
t = \lfloor \log^{2/3}(n) \rfloor
```

This approach aims to reduce the dependency on global ordering present in
Dijkstra's algorithm.

---

## Input File Format

Graphs are stored in `.txt` files:

```txt
N M
u1 v1 weight1
u2 v2 weight2
...
uM vM weightM
```

Where:

- `N` → number of vertices
- `M` → number of edges
- `u v weight` → edge between vertices `u` and `v`

### Example

```txt
5 6
0 1 2
0 2 4
1 2 1
1 3 7
2 4 3
3 4 1
```

Notes:

- vertices are 0-indexed
- the graph is treated as **undirected**
- weights must be positive

---

## Building the Project

```bash
make
```

This compiles the `.c` files, generates the `.o` object files, and creates the
`base` executable, using:

```bash
gcc -Wall -Wextra -O3 -lm
```

| Flag | Purpose |
|------|---------|
| `-Wall` | Enables important warnings |
| `-Wextra` | Extra warnings |
| `-O3` | Aggressive optimization |
| `-lm` | Math library (`log`, `pow`, etc.) |

---

## Manual Execution

```bash
./base file.txt
```

Or via the Makefile:

```bash
make run FILE=instance_example.dat
```

`make run` invokes:

```make
run:
	./$(TARGET) $(FILE)
```

allowing a file to be passed dynamically, e.g.:

```bash
make run FILE=data/graph100.txt
```

---

## Automated Tests

The project includes an automatic benchmarking system via `run_all.sh`:

```bash
make test
```

This target:

1. iterates over every `.txt` file in `data/`
2. runs all three algorithms on each instance
3. saves results to `results.txt`

Each test run reports, for Dijkstra, Duan and Bellman-Ford:

- total cost of the shortest paths
- execution time

---

## Test Instances (`data/`)

Files in `data/` represent different graph instances used for the
experimental benchmarks. File names encode:

- the **graph model**
- the **number of vertices**
- generation parameters
- an instance identifier

Example: `data/barabasi-10000-m3-1.txt`

| Part | Meaning |
|------|---------|
| `barabasi` | Graph model |
| `10000` | Number of vertices |
| `m3` | Model parameter (e.g., edges added per new node) |
| `1` | Instance identifier |

### Graph models used

| Name in file | Model |
|--------------|-------|
| `barabasi` | Barabási–Albert |
| `erdos` | Erdős–Rényi |
| `watts` | Watts–Strogatz |

### Instance count

The test set contains 10 instances of each model (Barabási-Albert,
Erdős-Rényi, Watts-Strogatz), with 2 graphs at each of the following sizes:
500, 1,000, 5,000, 10,000 and 20,000 vertices — allowing performance and
scalability comparisons across graph scales.

---

## Output Format

```txt
Dijkstra: <cost> <time>
Duan: <cost> <time>
bellmanFord: <cost> <time>
```

Example:

```txt
Dijkstra: 658195.000000 0.175240
Duan: 658195.000000 0.102341
bellmanFord: 658195.000000 1.482311
```

---

## Results File

After running `make test`, results are stored in `results.txt`:

```txt
===== Test 1 =====
File: data/graph100.txt
Dijkstra: ...
Duan: ...
bellmanFord: ...

===== Test 2 =====
File: data/graph500.txt
...
```

---

## Cleaning the Project

```bash
make clean
```

Removes the executable, `.o` files, and `results.txt`.

---

## Theoretical Comparison

| Algorithm | Complexity |
|-----------|------------|
| Dijkstra | `O(n²)` |
| Bellman-Ford | `O(n³)` |
| Duan (simplified) | `~O(n² log^(1/3)n)` |

- **Dijkstra** tends to be faster on small/medium graphs
- **Bellman-Ford** is slower due to multiple global relaxations
- **Duan** aims to reduce unnecessary relaxations through controlled local
  exploration

## Author

Pedro Brassi Luccas — Computer Science, UNIFAL-MG
Course: AEDS III, Professor Iago A. Carvalho
