# Image Processing Engine

Create a glow effect on PPM images using a multithreaded, box-blur-based pipeline.

> **Note:** This program operates on PPM images. Convert PNG/JPEG sources to `.ppm` first.

## Features

- Adds a glow effect to images by blurring bright pixels and compositing the blur back onto the original.
- Supports multithreaded processing by splitting the image into horizontal strips.
- Works with common image formats (PNG, JPEG) after conversion to PPM using online tools such as
  [CloudConvert](https://cloudconvert.com/ppm-converter) or [FreeConvert](https://www.freeconvert.com/ppm-converter),
  or desktop tools like Pixillion or Netpbm.

## Quick start

```bash
gcc -g -o glow_multithreaded read_ppm.c write_ppm.c glow_multithreaded.c -lpthreads -lm
./glow_multithreaded -N 8 -t 200 -b 17 -f earth.ppm
```

If you need a quick conversion step, use an online converter such as
[CloudConvert](https://cloudconvert.com/ppm-converter) or [FreeConvert](https://www.freeconvert.com/ppm-converter),
or a desktop tool like Pixillion or Netpbm.

## Requirements

- C compiler (gcc/clang)
- Make (optional)

## Build

Option A — Use GNU compiler:

```bash
gcc -g -o glow_multithreaded read_ppm.c write_ppm.c glow_multithreaded.c -lpthreads -lm
```

Option B — Using Makefile:

```bash
make
```

## Usage

```bash
./glow_multithreaded -N <NumThreads> -t <brightness threshold> -b <box blur size> -f <ppmfile>
```

Parameter guidance:

- `-N`: number of worker threads to use (e.g., 4, 8, 16).
- `-t`: brightness threshold (0–255); higher values target only very bright pixels.
- `-b`: box blur size (positive integer, >= 1). Larger values create a softer, wider glow.

Examples:
- To create a glow effect on an image saved in a file named `earth.ppm` using 8 threads, a threshold of 200, and a box size of 17:

```bash
./glow_multithreaded -N 8 -t 200 -b 17 -f earth.ppm
```

## Implementation details

The glow pipeline:

1. Identify pixels whose brightness exceeds the threshold.
2. Build a blur filter by averaging each color channel (red, green, blue) across a surrounding box. The
   box size controls how many neighboring pixels contribute to the average (e.g., a box size of 25 uses
   25 pixels total, including the target pixel).
3. Superpose the blur filter on the original image by adding the blurred color values to the original.
4. Split the image into horizontal strips based on the number of threads, and process each strip in parallel.
