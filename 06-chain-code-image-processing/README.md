# Chain Code (Freeman Chain Code) — C

**Course:** Image Processing — UNIFAL-MG
**Professor:** Luiz Eduardo da Silva
**Language:** C

A C implementation of the "Chain Code" practical activity: a pair of
programs that **encode** the contour of an object in a PBM image into a
compact `.ctn` file, and **decode** a `.ctn` file back into a reconstructed
PBM image.

## Project Structure

```
07-chain-code-image-processing/
├── Makefile
├── README.md
├── include/
│   ├── pbm.h         # PBM image reading/writing
│   ├── chaincode.h   # Contour tracing and hexadecimal conversion
│   └── ctn.h         # .ctn format reading/writing
├── src/
│   ├── pbm.c
│   ├── chaincode.c
│   ├── ctn.c
│   ├── encode.c      # `encoder` program's main
│   └── decode.c      # `decoder` program's main
├── examples/
│   ├── exemplo.ctn         # Example from the assignment statement (7x7, start (2,1), 13 directions)
│   ├── exemplo_forma.pbm   # 5x5 square ring (closed contour) for testing the encoder
│   └── estrela.pbm         # Test example - 5-pointed star, 41x41
├── bin/        # created by `make` - contains the executables (encoder, decoder)
├── build/      # created by `make` - contains intermediate .o files
└── results/    # created by `make` - contains .ctn/.pbm files generated at runtime
```

`bin/`, `build/` and `results/` are not part of the repository: they are
created automatically by `make` and fully removed by `make clean`.

## Build

From the project root:

```bash
make
```

This produces two executables in `bin/`: `bin/codificador` (encoder) and
`bin/decodificador` (decoder), and creates an empty `results/` folder to
receive generated `.ctn`/`.pbm` files.

To clean everything (`build/`, `bin/` and `results/`):

```bash
make clean
```

To build and run a quick test with the bundled examples (output goes to
`results/`):

```bash
make test
```

To run the encoder **and** decoder on any `.pbm` image in `examples/`
without typing the full commands, call `make` followed by the file name:

```bash
make <image_name>.pbm
```

Example, using `examples/estrela.pbm`:

```bash
make estrela.pbm
```

This automatically:
1. builds the project if it hasn't been built yet
2. runs the encoder on `examples/estrela.pbm`, producing `results/estrela.ctn`
3. runs the decoder on `results/estrela.ctn`, producing
   `results/estrela_reconstruido.pbm`

In general, for an image `examples/<name>.pbm`, `make <name>.pbm` generates
`results/<name>.ctn` and `results/<name>_reconstruido.pbm`. If the requested
file doesn't exist under `examples/`, `make` reports
`No rule to make target...` instead of running the programs.

## Usage

### Encoder (image → `.ctn`)

```bash
./bin/codificador <input_image.pbm> <output_file.ctn>
```

Example:
```bash
./bin/codificador examples/exemplo_forma.pbm output.ctn
```

The program prints the dimensions, starting point, number of directions and
the generated hexadecimal chain to the terminal, in addition to writing the
`.ctn` file.

### Decoder (`.ctn` → image)

```bash
./bin/decodificador <input_file.ctn> <output_image.pbm>
```

Example:
```bash
./bin/decodificador examples/exemplo.ctn output.pbm
```

The generated PBM image is always in **P1 (ASCII)** format.

> The PBM reader (`ler_pbm`) accepts both **P1 (ASCII)** and **P4 (binary)**
> as input. Writing is always in P1.

## `.ctn` File Format

```
<nl> <nc>
<start_row> <start_col>
<n_directions>
<hex_chain>
```

Example (`examples/exemplo.ctn`):
```
7 7
2 1
13
F700124648
```

## Direction System (8-connected)

Convention used:

```
3 2 1
4 x 0
5 6 7
```

| Code | Offset (Δrow, Δcol) | Direction |
|:----:|:---------------------:|-----------|
| 0 | (0, +1) | East |
| 1 | (-1, +1) | Northeast |
| 2 | (-1, 0) | North |
| 3 | (-1, -1) | Northwest |
| 4 | (0, -1) | West |
| 5 | (+1, -1) | Southwest |
| 6 | (+1, 0) | South |
| 7 | (+1, +1) | Southeast |

## How Encoding Works

1. **Starting point search**: scans the image top-to-bottom, left-to-right
   until it finds the first pixel with value 1.
2. **Contour tracing**: uses **Moore-Neighbor Tracing** (8-connected). At
   each step, the search for the next contour pixel starts from the
   direction right after the one opposite to the arrival direction, scanning
   the 8 neighbors in increasing order (counter-clockwise, per the
   assignment's numbering) until the next contour pixel is found. Tracing
   stops when it returns to the starting point (closed contour) or when
   there are no more contour neighbors (a single-pixel object).
3. **Chain generation**: each tracing step yields a direction code (0 to 7).
4. **Binary conversion**: each direction becomes 3 bits (000 to 111).
5. **Hexadecimal packing**: all direction bits are concatenated and grouped
   into 4-bit blocks. If the total bit count isn't a multiple of 4, the end
   is zero-padded up to the next multiple of 4 — exactly as in the
   assignment's example (13 directions × 3 bits = 39 bits → 1 padding bit →
   40 bits → 10 hex digits → `F700124648`).

## How Decoding Works

1. Reads `nl`, `nc`, the starting point, the number of directions and the
   hex chain.
2. Converts each hex digit into 4 bits.
3. Groups the bits into 3-bit blocks, recovering the direction codes
   (trailing padding bits are discarded).
4. Starting from the initial point, applies each offset sequentially,
   marking each visited pixel as 1 in an `nl x nc` image initially set to 0.
5. Writes the result as a PBM (P1) image.

## Validation

- `examples/exemplo.ctn` is exactly the example from the assignment
  statement (`7 7 / 2 1 / 13 / F700124648`). Decoding it reproduces, pixel
  by pixel, the point sequence manually computed in the assignment PDF:
  `(2,1) → (3,2) → (4,1) → (5,1) → (5,2) → (5,3) → (5,4) → (4,4) → (3,4) →
  (2,4) → (1,5) → (1,4) → (1,3) → (1,2)`.
- `examples/exemplo_forma.pbm` is a 5×5 square ring (closed contour).
  Encoding it and then decoding the resulting `.ctn` produces an image
  **pixel-for-pixel identical** to the original, validating the full
  encode → decode cycle.

## Author

Pedro Brassi Luccas — Computer Science, UNIFAL-MG
