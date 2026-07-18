# Partition Problem — Comparative Analysis of Heuristics

**Course:** Algorithms and Data Structures III (AEDS III) — UNIFAL-MG
**Department:** Computer Science, Universidade Federal de Alfenas (UNIFAL-MG)
**Language:** C

This repository contains the implementation, instance generators, heuristic
algorithms, and statistical reports developed for the resolution and
computational analysis of the **Partition Problem**.

The goal is to compare different heuristic and meta-heuristic approaches
applied to a classic **NP-complete** combinatorial optimization problem,
evaluating each method's performance in terms of **solution quality** and
**computational time**.

---

## 1. The Partition Problem

The **Partition Problem** consists of dividing a set of positive integers
into two disjoint subsets such that the absolute difference between the sums
of their elements is minimized.

Formally, given a set:

$$
S = \{x_1, x_2, x_3, ..., x_n\}
$$

the goal is to find two subsets $A \cup B = S$, $A \cap B = \emptyset$, that
minimize the objective function (*fitness*):

$$
\text{Fitness} = \left| \sum_{i \in A} x_i - \sum_{j \in B} x_j \right|
$$

The lower the fitness, the better balanced the subsets are. The ideal value
is **0**, representing a perfectly balanced partition.

---

## 2. Implemented Algorithms

Four different approaches were implemented and compared:

### 2.1 Pure Partition (Baseline)

A deterministic baseline: all elements are placed in group **A**, leaving
group **B** empty. Doesn't produce good solutions, but serves as a
reference point to measure the gains achieved by the other heuristics.

**Complexity:** `O(N)`

### 2.2 Greedy Algorithm (Adapted LPT)

Based on the **Longest Processing Time (LPT)** strategy. Elements are first
sorted in descending order (QuickSort), then each element is inserted into
the subset with the smallest accumulated sum at that point.

**Steps:**
1. Sort elements in descending order
2. Compare the partial sums of both groups
3. Greedily insert into the lower-load group

**Complexity:** `O(N log N)`

### 2.3 Simulated Annealing (Local Search)

A meta-heuristic inspired by the physical process of thermal annealing. The
solution is represented as a binary vector indicating which subset each
element belongs to. At each iteration, the algorithm explores neighboring
solutions via stochastic bit-flip perturbations, occasionally accepting
worse solutions to escape local minima.

Key implementation details:
- Neighbor generation via **bit flip**
- Shuffled indices using **Fisher–Yates shuffle**
- Probabilistic acceptance of worse solutions
- Gradual temperature cooling
- **Incremental** fitness update in `O(1)`, avoiding full recomputation of
  the sums on every move

### 2.4 Genetic Algorithm

A meta-heuristic based on natural selection and evolution. Each solution is
represented as a binary chromosome, where each gene indicates the subset
associated with the corresponding element.

Operators used:
- **Selection:** Tournament (`k = 3`)
- **Crossover:** Single-point crossover
- **Mutation:** Bit-flip mutation
- **Elitism:** pure elitism
- **Adaptive mutation:** mutation rate increases in later generations to
  reduce premature convergence

---

## 3. Project Structure

```text
├── include/                  # Header files (.h)
│   ├── leitor.h
│   ├── partition.h
│   ├── simulatedAnnealing.h
│   ├── genetico.h
│   └── guloso.h
│
├── src/                      # Source implementations (.c)
│   ├── leitor.c
│   ├── partition.c
│   ├── simulatedAnnealing.c
│   ├── genetico.c
│   ├── guloso.c
│   └── main.c
│
├── tools/                    # Auxiliary tools
│   └── instancias.c          # Instance generator
│
├── resultados/                # Generated statistical reports
│   ├── resultadosTabela.txt
│   └── resultadoMedia.txt
│
├── Makefile
└── README.md
```

---

## 4. Experimental Setup

To ensure statistical diversity and evaluate the algorithms' asymptotic
behavior, an automatic instance generator was developed
(`tools/instancias.c`).

**35 test instances** were created, across **7 different sizes**:

$$
N \in \{100, 200, 500, 1000, 2500, 5000, 10000\}
$$

Each size has **5 independent variations**, generated under different
maximum value bounds (`max_val`): 100, 1000, 5000, 25000, 100000. This
variation significantly affects number dispersion, density of near-optimal
solutions, and search-space difficulty.

Each algorithm was run **5 times per instance** (independent runs), for a
total of `35 instances × 5 runs = 175 runs per algorithm`, reducing the
impact of the randomness inherent to the meta-heuristics.

---

## 5. Build & Run

### Requirements
- `gcc` or `cc` compiler
- `make`
- Linux, macOS, or MinGW/WSL on Windows

### Clean previous builds (optional)
```bash
make clean
```

### Build
```bash
make
```

### Run the experiments
```bash
make run
```

While running, the terminal shows live progress of the test batch:

```text
========================================================================
   Starting General Experiment Batch
========================================================================

[PartitionPuro]      - Finished for 100   - Total time: 0.000854 s
[SimulatedAnnealing] - Finished for 100   - Total time: 0.054120 s
[AlgoritmoGenetico]  - Finished for 100   - Total time: 0.124512 s
[Guloso]             - Finished for 100   - Total time: 0.003011 s
----------------------------------------------------------------------------
```

Final reports are generated automatically in `resultados/`:
- `resultadosTabela.txt` → detailed results
- `resultadoMedia.txt` → consolidated averages

---

## 6. Experimental Results

The experiments totaled approximately **388 seconds** of computational time.

### Solution quality
- The **Genetic Algorithm** performed best on small/medium instances
  (`N ≤ 500`), thanks to population diversity.
- **Simulated Annealing** was more robust and stable on large instances
  (`1000 ≤ N ≤ 5000`), reaching near-optimal fitness at low computational
  cost.
- The **Greedy (LPT) Algorithm** was surprisingly competitive on large
  instances, achieving the best average fitness at `N = 10000`.

### Computational time
- **Pure Partition** had the lowest cost but the worst solution quality.
- **Simulated Annealing** scaled excellently, ranging from ~0.003s to
  ~0.064s.
- The **Genetic Algorithm**'s execution time grew significantly with `N`,
  reaching ~11.18s at `N = 10000`.
- The **Greedy Algorithm** ran nearly instantly at every scale tested.

---

## 7. Key Conclusions

**Genetic Algorithm — best on small instances:** on smaller problems
(`N ≤ 500`), it produced high-quality solutions thanks to population
diversity and the crossover/mutation operators, but its computational cost
grew significantly on larger instances.

**Simulated Annealing — best overall balance:** it offered the best
trade-off between solution quality, scalability, and computational cost.
The `O(1)` incremental fitness update allowed a high number of iterations
while keeping execution time low.

**Greedy — excellent at large scale:** at `N = 10000`, it achieved the best
average fitness with essentially instant execution, suggesting that for
large sets, the density of near-optimal solutions favors simple
incremental-balancing approaches.

---

## 8. Technologies

- **Language:** C
- **Compiler:** GCC
- **Build system:** Makefile
- **Paradigms:** Heuristics, meta-heuristics, evolutionary computation

## 9. Author

Pedro Brassi Luccas
Universidade Federal de Alfenas (UNIFAL-MG) — Computer Science Department, AEDS III
