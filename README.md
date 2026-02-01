# Image Processing Engine

Image Processing Engine that creates a glow effect on an image.

# Implementation details
The program creates a glow effect on an image by identifying pixels whose brightness exceeds a given threshold, then creating a blur filter by computing new values for each color component (red, blue, green) of those pixels by finding the average value of the color component from the value of the color component of the pixels around the given pixel. The number of pixels used to compute this average(new value) depends on the given box size. For example, given a box size of 25, 25 pixels(including the pixel in question and the 24 around it) are used to compute the new/average value of the color component. The blur filter is then superposed on the original picture by adding the new value of each color component to the original.
The number of threads to be used as taken as input through stdin and depending on the given number, the image is divided horizontally into N strips, where N is the number of threads to be used.

# Features

- Brightness adjustment (brighten or darken)
- Gaussian/box blur (configurable radius)
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
./glow_multithreaded -N <NumThreads> -t <brightness threshold> -b <box blur size> -f <ppmfile>

Common options (update to match your program's CLI):
- --brightness <value>    Adjust brightness by percentage or absolute value. Example: `--brightness 20` (brighten by 20%), `--brightness -10` (darken by 10)
- --blur <radius>         Apply blur with given radius/intensity. Example: `--blur 5`
- --help                  Show usage information

Examples:
- Increase brightness by 15%:
  ./imageproc --brightness 15 in.jpg out.jpg
- Apply blur radius 3:
  ./imageproc --blur 3 in.png out.png
- Combine operations (brightness then blur):
  ./imageproc --brightness 10 --blur 2 in.jpg out.jpg

Notes:
- Parameter ranges depend on your implementation. Typical ranges:
  - brightness: -100 .. +100 (percentage)
  - blur radius: 0 (no blur) .. 10+ (increasing blur)
- Order of operations can affect the final result (e.g., blur then brighten vs brighten then blur).

---

## Examples

Before:
- in.jpg

Commands:
- Brighten:
  ./imageproc --brightness 25 in.jpg brightened.jpg
- Blur:
  ./imageproc --blur 6 in.jpg blurred.jpg
- Both:
  ./imageproc --brightness 10 --blur 3 in.jpg out.jpg

After:
- out.jpg (resulting image)

Include sample input/output images in a `samples/` folder in the repo for demonstration.

---

## Implementation Notes (for maintainers)

- If you're using convolution to implement blur, consider separable Gaussian blur (horizontal + vertical passes) for performance.
- For brightness, operate in linear RGB or convert to a color space appropriate for the intended effect (simple implementations operate per-channel).
- Handle overflow/clamping: ensure pixel values are clamped to [0, 255] for 8-bit images.
- Consider supporting alpha channel and preserving metadata if desired.

---

## Testing

- Add unit tests or example scripts that run the binary on sample images and compare outputs (visual inspections or checksums).
- Validate behavior across formats (PNG, JPEG) and different color types (grayscale, RGB, RGBA).

---

## Contributing

Contributions are welcome. To contribute:
1. Fork the repository.
2. Create a branch: `git checkout -b feature/my-change`
3. Make changes and run tests/build.
4. Open a pull request describing your changes.

Please include:
- A description of the change
- Any new dependencies
- Before/after sample images if relevant

---

## License

Add a LICENSE file to the repository. Suggested: MIT License (or choose whichever license you prefer).

---

## Acknowledgements

- If using stb single-header libraries: https://github.com/nothings/stb
- Any other libraries or resources you used

---

If you want, I can:
- Generate this README.md file and commit it to the repo, or
- Inspect the repository to tailor the README to the actual source files, flags, and dependencies (I can read the repo and update the usage/build sections to match). Which would you prefer?

— GitHub Copilot Chat Assistant
