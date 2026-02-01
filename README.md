# Image Processing Engine

Image Processing Engine that creates a glow effect on an image.

# Implementation details
The program creates a glow effect on an image by identifying pixels whose brightness exceeds a given threshold, then creating a blur filter by computing new values for each color component (red, green, blue) of those pixels by finding the average value of the color component from the value of the color component of the pixels around the given pixel. The number of pixels used to compute this average(new value) depends on the given box size. For example, given a box size of 25, 25 pixels(including the pixel in question and the 24 around it) are used to compute the new/average value of the color component. The blur filter is then superposed on the original picture by adding the new value of each color component to the original.
The number of threads to be used as taken as input through stdin and depending on the given number, the image is divided horizontally into N strips, where N is the number of threads to be used.

# Features

- Works with common image formats (PNG, JPEG) when converted to .ppm using online tools e.g. [CloudConvert](https://cloudconvert.com/ppm-converter) or [FreeConvert](https://www.freeconvert.com/ppm-converter) or desktop tools e.g. Pixillion or Netpbm

## Requirements

- C compiler (gcc/clang)
- Make (optional)

## Build

Option A — Use GNU compiler:
run:
gcc -g -o glow_multithreaded read_ppm.c write_ppm.c glow_multithreaded.c -lpthreads -lm

Option B — Using Makefile:
Use the project's Makefile by running:
make

## Usage
To run, run the following command:
./glow_multithreaded -N &ltNumThreads&gt -t &ltbrightness threshold&gt -b &ltbox blur size&gt -f &ltppmfile&gt

Examples:
- To create a glow effect on an image saved in a file named 'earth.ppm" using 8 threads, a threshold of 200, and a box size of 17:
  ./glow_multithreaded -N 8 -t 200 -b 17 -f earth.ppm
  
