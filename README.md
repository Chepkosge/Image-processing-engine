# Image Processing Engine

Image Processing Engine is a small C program for applying simple image edits — currently supports:
- Adjusting image brightness
- Applying a blur filter

# Features

- Brightness adjustment (brighten or darken)
- Gaussian/box blur (configurable radius)
- Works with common image formats (PNG, JPEG) when converted to .ppm using online tools e.g. [CloudConvert](https://cloudconvert.com/ppm-converter) or [FreeConvert](https://www.freeconvert.com/ppm-converter) or desktop tools e.g. Pixillion or Netpbm

## Requirements

- C compiler (gcc/clang)
- Make (optional)
- One of the following image I/O options (choose whichever your project uses; install if needed):
  - stb_image.h + stb_image_write.h (single-header public-domain/ISC-style libs)
  - libpng / libjpeg (development headers)
- math library (libm) if your code uses math functions

---

## Build

Below are example build instructions. Update the commands to match your repository layout and chosen image I/O implementation.

Option A — single-header (stb):
1. Place `stb_image.h` and `stb_image_write.h` in the project (or vendor them).
2. Build:
   gcc -O2 -Wall -o imageproc src/*.c -lm

Option B — libpng/libjpeg (pkg-config):
1. Install development packages (example on Debian/Ubuntu):
   sudo apt-get install libpng-dev libjpeg-dev
2. Build:
   gcc -O2 -Wall -o imageproc src/*.c `pkg-config --cflags --libs libpng libjpeg` -lm

If you use a Makefile, run:
   make

Adjust source paths and flags to match the repository.

---

## Usage

Replace `imageproc` with your executable name if different.

Basic syntax:
   ./imageproc [options] <input-file> <output-file>

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
