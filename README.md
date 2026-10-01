# StegResize

JPEG header steganography and dimension manipulation tool for CTF forensics challenges.

Extracts and modifies the SOI (Start of Image) marker metadata in JPEG files, specifically the height and width fields in the JPEG frame header (FF C0 marker). Allows interactive resizing of embedded image dimensions while preserving or altering hidden data encoded in the header structure. Useful for analyzing steganographic payloads hidden in JPEG headers, recovering obfuscated dimensions, or reversing dimension-based steganography techniques.

![sof0](/example/jpegsof0.png "JPEG SOF0 Explanation")

Common CTF use: Extract flag data from JPEG headers where metadata has been tampered with or hidden, or decode challenges where image dimensions encode information via header manipulation.

Usage:

`stegresize <image_file>`

## Original Image

![og](/example/office.jpg "Orginal Image")

## Utilising StegResize

![usage](/example/usage.png "Usage")

![success](/example/success.png "Script Completion Successful")

## Final Image

![final](/example/final.jpg "Final Image")
Flag : `HQ8{dc3f4d7cd6d33f8a903a71adeabeda5a}`

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
