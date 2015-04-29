# png2pos
[![Build Status](https://travis-ci.org/petrkutalek/png2pos.svg?branch=master)](https://travis-ci.org/petrkutalek/png2pos)
[![Paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=4TNCBPJT2R4MC)

png2pos is a utility to convert PNG images to ESC/POS format (printer control codes and escape sequences) used by POS thermal printers. Output file can be just sent to printer.

**png2pos does not contain any Epson drivers, it is not a driver/filter replacement… png2pos is just a utility for embedded projects, for whose printing PNG files is sufficient and ideal lightweight solution.**

png2pos is:

* **free** and open-source
* rock **stable**
* highly **optimized**, fast, tiny and lightweight (few KiBs binary, no lib dependencies)
* **secure** (does not require any escalated privileges)
* using **high-quality** algorithms
* easy to use
* multiplatform (tested on Linux — x86 and ARM/Raspberry Pi, OS X and Windows)
* well **tested**
* 100% handcrafted in Prague, CZ :-)

## Compatibility

Check [Supported Printers](./compatibility.md) page.

## How does it work?

It accepts any PNG file (B/W, greyscale, RGB, RGBA), applies Histogram Equalization Algorithm and via Atkinson Dithering Algorithm converts it to B/W bitmap wrapped by ESC/POS commands.

ESC/POS is a printer language. The “POS” stands for “Point of Sale”, the “ESC” stands for “escape” because command instructions are escaped with a special characters. png2pos utilizes ```ESC@```, ```GSV```, ```GSL```, ```GS8L``` and ```GS(L``` ESC/POS commands. It also prepends needed printer initialization binary sequences and adds paper cutoff command, if requested.

png2pos requires 5 × WIDTH (rounded up to multiple of 8) × HEIGHT bytes of RAM. (e.g. to process full-width image of receipt 768 pixels tall you need about 2 MiB of RAM.)

png2pos converts RGBA images into greyscale version via algorithm compliant with ITU-R, BT.709. (RGBA → RGB → R'G'B' (gamma 2.2) → luma Y' → lightness L*). For performance reasons png2pos uses pre-calculated lookup tables and integer based math.

![gamma](docs/gamma.png)

## Pricing and Support

png2pos is free MIT-licensed software provided as is. **Unfortunately I am unable to provide you with free support**.
If you like png2pos and use it, please let me know, it motivates me in further development.

Important: png2pos will *never ever* be like Emacs; it is simple and specialized utility.

There is also an alternative project called [png2escpos](https://github.com/twg/png2escpos) created by The Working Group Inc. It's younger, simpler and uses libpng. png2pos is in all aspects mature, nevertheless you may like alternative even more.

## Precompiled binary packages

There are available prepared builds for some common platforms in [Releases](https://github.com/petrkutalek/png2pos/releases) section.

Please, if you use GPG, verify also attached GPG signature:

Import public key into temporary keyring (you can import it into your permanent keyring of course) and verify ZIP file's ASC signature:

    $ curl https://forers.com/494CD31C.asc | gpg --no-default-keyring --keyring ./gpg-png2pos.tmp --import ↵
    gpg: key 494CD31C: public key "Petr Kutalek <petr.kutalek@forers.com>" imported
    gpg: Total number processed: 1
    gpg: imported: 1 (RSA: 1)

    $ gpg --no-default-keyring --keyring ./gpg-png2pos.tmp --verify png2pos-v1.5.4-rpi.zip.asc ↵
    gpg: Signature made Thu Jan 1 20:42:13 2015 CET using RSA key ID 494CD31C
    gpg: Good signature from "Petr Kutalek <petr.kutalek@forers.com>"
    gpg: WARNING: This key is not certified with a trusted signature!
    gpg: There is no indication that the signature belongs to the owner.
    Primary key fingerprint: 06EB BE07 4850 3817 8533 B45D C1D9 23EB 494C D31C

    $ rm ./gpg-png2pos.*

## Build

If you prefer to build binary file yourself, clone the source code:

    $ git clone https://github.com/petrkutalek/png2pos.git ↵
    $ cd png2pos ↵
    $ git submodule init ↵
    $ git submodule update ↵

To build and install binary just type:

    $ make install ↵

On Mac typically you can use clang preprocessor:

    $ make CC=clang install ↵

On Linux you can also build static binary (e.g. also based on [musl](http://www.musl-libc.org/intro.html)):

    $ make CC=/usr/local/musl/bin/musl-gcc static ↵
    $ make install ↵

Windows binary is build in MinGW by (MinGW must be included in PATH):

    C:\devel\png2pos> mingw32-make -f Makefile.win strip

### Available make targets

target | make will build…
:----- | :------
(empty)  | png2pos
clean | (removes intermediate products)
strip | stripped version (suggested)
profiled | profiled version (up to 3 % performance gain on repeat tasks)
install | install png2pos into PREFIX (default /usr/local)

png2pos has no lib dependencies and is easy to build and run on Linux, Mac and Windows.

## Usage examples

    $ png2pos -c -r /tmp/*.png > /dev/usb/lp0 ↵

## Examples

### Lena
Original (3 bytes per pixel)

![original](docs/lena_png2pos_0_original.png)

Greyscale version (1 byte per pixel)

![grey](docs/lena_png2pos_1_grey.png)

Post-processed version (Histogram Equalization Algorithm, 1 byte per pixel)

![post-processed](docs/lena_png2pos_2_pp.png)

Produced B/W dithered version (Atkinson Dithering Algorithm, 1 bit per pixel)

![B/W](docs/lena_png2pos_3_bw.png)

Histograms (raw and post-processed image)

![histograms](docs/lena_png2pos_histogram.png)
