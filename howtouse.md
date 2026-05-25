# MetaDataChanger — How to Use

Strip AI-generated metadata from images and replace it with professional EXIF data — either a real-looking DSLR camera profile or a Canva export profile.

---

## Setup

```bash
# Install dependencies (one-time)
pip install Pillow piexif

# Or if using the virtual environment already in this folder
source .venv/bin/activate
```

---

## Basic Syntax

```
python metadatachanger.py <input_image> [options]
```

---

## Modes

| Mode | Flag | What it does |
|------|------|--------------|
| Camera (default) | *(none)* | Injects real DSLR/mirrorless EXIF (Canon, Nikon, Sony) |
| Canva | `--canva` | Injects Canva-style export metadata + XMP |

---

## All Commands & Examples

### 1. Basic — Camera mode (default)

Strips AI metadata and applies Canon EOS R5 EXIF by default.

```bash
python metadatachanger.py photo.jpg
```

Output: `photo_clean.jpg`

---

### 2. Choose a different camera preset (`-c` / `--camera`)

```bash
python metadatachanger.py photo.jpg -c 2
```

Picks camera preset #2 (Nikon Z 9). See all presets with `--list-cameras`.

---

### 3. List all camera presets (`--list-cameras`)

```bash
python metadatachanger.py --list-cameras
```

Output:
```
  0  Canon EOS R5           Lens: RF 24-70mm F2.8 L IS USM  |  50mm  f/2.8  ISO 400
  1  Canon EOS R6 Mark II   Lens: RF 50mm F1.2 L USM         |  50mm  f/1.2  ISO 800
  2  NIKON Z 9              Lens: NIKKOR Z 24-70mm f/2.8 S   |  35mm  f/2.8  ISO 200
  3  NIKON Z 8              Lens: NIKKOR Z 85mm f/1.8 S       |  85mm  f/1.8  ISO 640
  4  Sony ILCE-7RM5         Lens: FE 24-70mm F2.8 GM II       |  35mm  f/2.8  ISO 320
  5  Sony ILCE-1            Lens: FE 85mm F1.4 GM             |  85mm  f/1.4  ISO 500
```

---

### 4. Set artist name (`-a` / `--artist`)

```bash
python metadatachanger.py photo.jpg -a "John Doe"
```

```bash
python metadatachanger.py photo.jpg -c 3 -a "Jane Smith"
```

---

### 5. Set custom output path (`-o` / `--output`)

```bash
python metadatachanger.py photo.jpg -o /path/to/output.jpg
```

```bash
python metadatachanger.py photo.jpg -o result.png
```

---

### 6. Set custom copyright (`--copyright`)

```bash
python metadatachanger.py photo.jpg --copyright "© 2026 John Doe Photography"
```

If omitted, copyright is auto-generated as `© <year> <artist>`.

---

### 7. Canva mode (`--canva`)

Makes the image look like it was exported from Canva. Injects Canva XMP metadata.

```bash
python metadatachanger.py design.png --canva
```

---

### 8. Canva mode with title and artist

```bash
python metadatachanger.py design.png --canva --title "Brand Kit 2026" -a "Jane"
```

---

### 9. Combine multiple options

```bash
python metadatachanger.py photo.jpg -c 2 -a "John Doe" --copyright "© 2026 John Doe" -o output.jpg
```

```bash
python metadatachanger.py design.png --canva -t "Summer Campaign" -a "Jane Smith" -o final.png
```

---

## Supported Formats

| Format | Camera mode | Canva mode |
|--------|-------------|------------|
| `.jpg` / `.jpeg` | Full support | Full support + XMP |
| `.png` | Full support | Full support + XMP |
| `.webp` | Full support | Full support + XMP |
| `.tiff` | Full support | Full support |
| Other | Saved as `.jpg` | Saved as `.jpg` |

---

## All Flags Reference

| Flag | Short | Default | Description |
|------|-------|---------|-------------|
| `input` | — | required | Path to input image |
| `--output` | `-o` | `<name>_clean.<ext>` | Output file path |
| `--canva` | — | off | Use Canva mode instead of camera |
| `--camera` | `-c` | `0` | Camera preset number (0–5) |
| `--list-cameras` | — | — | Print all camera presets and exit |
| `--artist` | `-a` | `""` | Artist / photographer / creator name |
| `--title` | `-t` | `""` | Design title (Canva mode only) |
| `--copyright` | — | `"© year artist"` | Custom copyright string |

---

## What the Tool Does Internally

1. **Detects** — scans for AI traces in PNG chunks, EXIF fields, and XMP data (Stable Diffusion, Midjourney, DALL-E, ComfyUI, etc.)
2. **Strips** — removes all existing metadata (info chunks, EXIF, XMP)
3. **Rebuilds** — injects new clean EXIF matching a real camera or Canva export
4. **Saves** — writes the cleaned image to the output path

---

## Quick Reference

```bash
# Quickest use — camera mode, default output name
python metadatachanger.py image.jpg

# Pick a Sony camera, set artist
python metadatachanger.py image.jpg -c 4 -a "My Name"

# Canva mode with title
python metadatachanger.py design.png --canva -t "My Design" -a "My Name"

# See all cameras
python metadatachanger.py --list-cameras
```
