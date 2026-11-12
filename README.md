# GCI-CONVERTER-REGION
Convert GameCube save files (.gci) between PAL, NTSC-U and NTSC-J in your browser. Single HTML file, runs 100% locally, changes only one header byte.
https://alanpiguaveruiz.github.io/GCI-CONVERTER-REGION/

# GCI Region Converter

Change the region of a GameCube save file (`.gci`) between **PAL**, **NTSC-U** and **NTSC-J**, right in your browser. One HTML file, no dependencies, nothing to install.

**Live demo:** `https://<your-user>.github.io/<your-repo>/` *(replace after enabling GitHub Pages)*

## Features

- Runs 100% locally: your save is never uploaded anywhere.
- Works on any OS with a modern browser (Windows, macOS, Linux, Android, iOS).
- Writes a **single byte** (offset `0x03`) and leaves everything else untouched, including the maker code.
- Renames the downloaded file so the region letter in the game code matches the new region (e.g. `01-GC6E-Colosseum.gci` becomes `01-GC6P-Colosseum.gci`).
- Reads and shows the game code, maker code, current region and file size, and warns if the header or size looks unusual.
- Drag and drop support, keyboard accessible, responsive layout.

## Usage

1. Open `index.html` in your browser (double-click it, or use the hosted page).
2. Drop your `.gci` file onto the page, or click to choose it.
3. Pick the target region. The current region is locked.
4. Click **Download** and replace the original save in your emulator or memory card.

The original file is never modified. The browser can't overwrite it, so you always get a converted copy.

## How it works

A `.gci` file starts with the directory entry of the save:

| Offset | Size | Meaning |
|--------|------|---------|
| `0x00` | 4    | Game code. The 4th byte is the region: `P` = PAL, `E` = NTSC-U, `J` = NTSC-J |
| `0x04` | 2    | Maker (publisher) code |
| `0x06` | 1    | Unused, normally `0xFF` |

A valid file is `0x40 + n * 0x2000` bytes long. The converter changes only byte `0x03` and renames the file accordingly.

## Limitations

- Only the region byte in the header is changed. The save data itself is **not** converted, so the target game must use the same save format as the source game.
- Not every game accepts a save from another region, even with a matching header.
- Always keep a backup of your original saves.

Tested with a *Pokémon Colosseum* save, which loaded correctly as a USA save.

## Code structure

Everything lives in one `index.html`, split into clearly separated sections:

1. **Styles:** design tokens first, then base, layout and components.
2. **Markup:** structure only.
3. **`GCI` module:** pure byte-level logic (`leer`, `convertir`, `codigoConRegion`, `renombrar`) with no DOM access.
4. **UI:** connects the module to the page.

## Background

This is a cross-platform rewrite of *GCConverter 1.0*, a Windows-only (.NET Framework) tool that changes the region of GameCube saves.

## Disclaimer

Not affiliated with or endorsed by Nintendo. *GameCube* and all game titles are trademarks of their respective owners.

## License

MIT. Add a `LICENSE` file to match.
