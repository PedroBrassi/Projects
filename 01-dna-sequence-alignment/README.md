# DNA Sequence Alignment

**Course:** Algorithms and Data Structures III (AEDS III) — UNIFAL-MG
**Professor:** Iago A. Carvalho
**Language:** C

## Overview

This project implements and compares two algorithms for **DNA sequence
alignment** (pairwise sequence alignment), a classic bioinformatics problem:
given two DNA strings (over the alphabet `A, T, C, G`), find the best way to
align them — optionally inserting gaps — to maximize a pairing score, where
`A` pairs with `T` and `C` pairs with `G`.

Two approaches were required:

- A **dynamic programming** algorithm (Needleman-Wunsch style), which
  guarantees the optimal alignment score.
- A **greedy** algorithm, which trades optimality for speed.

Both algorithms compute the total pairing score and are benchmarked for
execution time over a set of provided test instances.

## Project Structure

```
02-dna-sequence-alignment/
├── src/
│   ├── base.c            # Driver program (provided by the professor, not modified)
│   ├── algoritmos.c       # Dynamic programming + greedy implementations
│   ├── algoritmos.h       # Function prototypes
│   └── DNA.txt            # Sample input sequences
├── docs/
│   ├── descricao.pdf              # Original assignment statement
│   ├── Pareamento_de_DNA___TP2.pdf # Assignment slides/spec
│   ├── Relatório tp2.versaofinal.pdf # Final experimental report
│   ├── Tabela.txt                 # Results table
│   └── comparativo_bottomup_traceback.jpg
```

## Algorithms

- **Dynamic Programming**: builds a score matrix bottom-up over the two
  sequences and reconstructs the optimal alignment via traceback —
  `O(n·m)` time and space.
- **Greedy**: pairs bases using a local, incremental decision rule without
  backtracking — much faster, but not guaranteed optimal.

## Build & Run

```bash
make
./base <arquivo_de_entrada>
```

Only `algoritmos.c` / `algoritmos.h` were written for this assignment; the
driver (`base.c`) was provided as-is by the professor.

## Author

Pedro Brassi Luccas — Computer Science, UNIFAL-MG
