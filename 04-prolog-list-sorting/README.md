# Prolog — Sorting Lists of Lists

**Course:** Logic Programming — UNIFAL-MG
**Professor:** Luiz Eduardo da Silva
**Language:** Prolog

## Overview

This activity implements two classic Prolog list-sorting predicates from
scratch (no built-ins), using recursive helper predicates and insertion sort.

### Part A — `lsort/2`

Sorts a list of lists by the **length of each sublist** (shortest first,
duplicates preserved).

Strategy: *insert → sort → strip*
1. Compute the length of each sublist
2. Build `(Length, Sublist)` pairs
3. Sort the pairs by length using insertion sort
4. Strip the lengths, returning only the sorted sublists

### Part B — `lfsort/2`

Sorts a list of lists by **the frequency of their lengths** (sublists whose
length occurs less often come first; ties are broken by length).

Strategy: *insert → sort → strip*
1. Compute the length of each sublist
2. Count how many sublists share each length (frequency)
3. Build `(Frequency, Length, Sublist)` triples
4. Sort by frequency, breaking ties by length
5. Strip frequency/length, returning only the sorted sublists

## Files

| File | Description |
|------|-------------|
| `atividade.pl` | Prolog source with both predicates and their helpers |
| `atividade1.pdf` | Original assignment statement |

## Usage

Load the file in a Prolog interpreter (e.g. SWI-Prolog):

```prolog
?- consult('atividade.pl').
?- lsort([[1,2,3],[1],[1,2]], X).
?- lfsort([[1,2,3],[1],[1,2],[4]], X).
```

## Author

Pedro Brassi Luccas — Computer Science, UNIFAL-MG
