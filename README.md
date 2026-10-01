# StegResize

**Fix or change the height of a JPEG to reveal hidden or cropped parts of the image.** Built for CTF steganography and forensics challenges.

Reads and modifies the height and width fields in a JPEG's SOF0 (Start of Frame, `FF C0`) segment. A common CTF trick is to shrink the height in this header so part of the image is cut off when viewed, even though the pixel data is still in the file. StegResize lets you change the height interactively to reveal the hidden or cropped part of the image.

Useful for recovering images with tampered dimensions, finding flags hidden below the visible area, and solving dimension-based steganography challenges.

Use it when:
- A CTF image looks cut off, cropped, or oddly short
- The file size seems too large for the visible image
- A challenge hints at "dimensions", "height", or "something below"

![sof0](/example/jpegsof0.png "JPEG SOF0 Explanation")

## Usage

```bash
stegresize <image_file>
```

## Original Image

![og](/example/office.jpg "Original Image")

## Utilising StegResize

![usage](/example/usage.png "Usage")

![success](/example/success.png "Script Completion Successful")

## Final Image

![final](/example/final.jpg "Final Image")
Flag : `HQ8{dc3f4d7cd6d33f8a903a71adeabeda5a}`

## How it works

Every baseline JPEG has a SOF0 (Start of Frame) segment starting with the bytes `FF C0`. The height and width are stored right after it as 2-byte big-endian numbers:

| Bytes | Meaning |
|---|---|
| `FF C0` | SOF0 marker |
| `00 11` | Segment length (17 bytes) |
| `08` | Bits per sample |
| `05 F5` | **Height** (0x05F5 = 1525 px) |
| `05 39` | **Width** (0x0539 = 1337 px) |
| `03` | Number of color components |

### Before and after (the example in this repo)

`example/office.jpg`, with the SOF0 segment at offset `0x9E`:

```
ff c0 00 11 08 05 f5 05 39 03
               ^^^^^ height = 1525
```

`example/final.jpg`, after StegResize raised the height:

```
ff c0 00 11 08 07 08 05 39 03
               ^^^^^ height = 1800
```

Only two bytes changed: `05 F5` became `07 08`. Both files are exactly the same size, because the hidden rows were in the file all along. The viewer just wasn't told to draw them.

### Doing it by hand

If you want to check a file without the tool:

```bash
# Find the SOF0 marker offset (in decimal)
grep -obUaP "\xff\xc0" image.jpg | head -1

# Show the 10 bytes from that offset (example: 158)
xxd -s 158 -l 10 image.jpg

# Overwrite the height (offset + 5) with 0x0708 = 1800
printf '\x07\x08' | dd of=image.jpg bs=1 seek=163 conv=notrunc
```

StegResize automates this and lets you try different values quickly.

### Why height, not width

JPEG pixel data is stored row by row, and the decoder uses the width in the header to know where each row ends. Raising the height just tells it to draw more rows, so the hidden part appears cleanly. Changing the width makes every row wrap in the wrong place, which shifts the blocks and produces a slanted, scrambled image.

StegResize also asks for a width, but only change it if a challenge has tampered with the width itself and you know the correct value. Otherwise, type the original width again when prompted.

---

# Installation

Here are the steps to install the `stegresize` command:

1. Open a terminal.

2. Clone the repository by running the following command:

```bash
git clone https://github.com/erhszo/stegresize.git
```

3. Navigate to the directory where the repository was cloned:

```bash
cd stegresize
```

4. Run the `install.sh` script:

```bash
sudo bash install.sh
```

After running these commands, the `stegresize` command should be installed and can be used from anywhere in the terminal.
