<div align="center">

</div>

## Contents

- [Installation](#installation)
- [Usage](#usage)
- [Advanced Usage](#advanced-usage)
- [Datasets](#datasets)
- [Citation](#citation)
- [License](#license)


## Installation

SynthTIGER requires `python>=3.6` and `libraqm`.

To install SynthTIGER from PyPI:

```bash
$ pip install synthtiger
```

If you see a dependency error when you install or run SynthTIGER, install [dependencies](depends).

## Usage

```bash
# Set environment variable (for macOS)
$ export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
```

```
usage: synthtiger [-h] [-o DIR] [-c NUM] [-w NUM] [-s NUM] [-v] SCRIPT NAME [CONFIG]

positional arguments:
  SCRIPT                Script file path.
  NAME                  Template class name.
  CONFIG                Config file path.

optional arguments:
  -h, --help            show this help message and exit
  -o DIR, --output DIR  Directory path to save data.
  -c NUM, --count NUM   Number of output data. [default: 100]
  -w NUM, --worker NUM  Number of workers. If 0, It generates data in the main process. [default: 0]
  -s NUM, --seed NUM    Random seed. [default: None]
  -v, --verbose         Print error messages while generating data.
```

### Examples

#### SynthTIGER text images

```bash
# horizontal
synthtiger -o results -w 4 -v examples/synthtiger/template.py SynthTiger examples/synthtiger/config_horizontal.yaml

# vertical
synthtiger -o results -w 4 -v examples/synthtiger/template.py SynthTiger examples/synthtiger/config_vertical.yaml
```

<p>
    <img src="https://user-images.githubusercontent.com/12423224/153699084-1d5fbb15-0ca0-4a85-9639-6f2c4c1bf9ec.png" width="50%"/>
    <img src="https://user-images.githubusercontent.com/12423224/199258481-5706db59-127a-4453-a8ab-4a0bb9f266d5.png" width="45%"/>
</p>

- `images`: a directory containing images.
- `gt.txt`: a file containing text labels.
- `coords.txt`: a file containing bounding boxes of characters with text effect.
- `glyph_coords.txt`: a file containing bounding boxes of characters without text effect.
- `masks`: a directory containing mask images with text effect.
- `glyph_masks`: a directory containing mask images without text effect.

#### Multiline text images

```bash
synthtiger -o results -w 4 -v examples/multiline/template.py Multiline examples/multiline/config.yaml
```

<img src="https://user-images.githubusercontent.com/12423224/153699088-cdeb3eb3-e117-4959-abf4-8454ad95d886.png" width="75%"/>

- `images`: a directory containing images.
- `gt.txt`: a file containing text labels.

## For any other language


1. Prepare corpus

   `txt` format, line by line ([example](resources/corpus/mjsynth.txt)).

2. Prepare fonts

   See [font customization](#font-customization) for more details.

3. Edit corpus path and font path in config file ([example](examples/synthtiger/config_horizontal.yaml))

4. Run synthtiger

### Font customization

1. Prepare fonts

   `ttf`/`otf` format ([example](resources/font)).

2. Extract renderable charsets

   ```bash
   python tools/extract_font_charset.py -w 4 fonts/
   ```

   This script extracts renderable charsets for all font files ([example](resources/font/Ubuntu-Regular.txt)).

   Text files are generated in the input path with the same names as the fonts.

3. Edit font path in config file ([example](examples/synthtiger/config_horizontal.yaml))

4. Run synthtiger

### Colormap customization

1. Prepare images

   `jpg`/`jpeg`/`png`/`bmp` format.

2. Create colormaps

   ```bash
   python tools/create_colormap.py --max_k 3 -w 4 images/ colormap.txt
   ```

   This script creates colormaps for all image files ([example](resources/colormap/iiit5k_gray.txt)).

3. Edit colormap path in config file ([example](examples/synthtiger/config_horizontal.yaml))

4. Run synthtiger

