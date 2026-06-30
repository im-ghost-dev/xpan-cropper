# XPAN Cropper

**A browser-based Hasselblad XPan panoramic crop tool with film emulation and AI upscaling.**  
Built by [G.H.O.S.T](https://github.com/im-ghost-dev) · Bangalore, India

---

## Screenshots

<table>
  <tr>
    <td align="center" width="60%">
      <img src="docs/screenshots/desktop-ui.png" alt="Desktop UI" width="100%"/>
      <sub>Desktop</sub>
    </td>
    <td align="center" width="40%">
      <img src="docs/screenshots/mobile-ui.jpg" alt="Mobile UI" width="100%"/>
      <sub>Mobile</sub>
    </td>
  </tr>
</table>

---

## What is this?

The [Hasselblad XPan](https://en.wikipedia.org/wiki/Hasselblad_XPan) was a cult panoramic film camera that exposed a **65×24mm frame** across two standard 35mm frames — producing an extreme **2.71:1 aspect ratio** that no digital camera natively shoots.

This tool lets you simulate that framing on any digital photo. Load an image, compose your panoramic frame by panning and zooming inside the crop box, apply one of 27 hand-tuned film stock emulations, and export a full-resolution JPEG — with optional AI upscaling so the tight crop never looks soft.

Everything runs **entirely in the browser**. No server. No upload. No installation. Single HTML file.

---

## Live Demo

> **[im-ghost-dev.github.io/xpan-cropper](https://im-ghost-dev.github.io/xpan-cropper)**

---

## Features

### Core
- **XPan crop** — 65:24 (2.71:1) ratio, locked to the Hasselblad XPan standard
- **Pan & zoom** — drag to pan, scroll/pinch to zoom, double-tap/click to reset
- **Rotation** — ±45° slider with pan constraint recalculated on the rotated bounding box
- **Custom ratios** — presets for XPan / 3:1 / 2:1 / 16:9 / 7:3, plus free custom W:H input
- **Drag-and-drop** — drop a photo directly onto the canvas

### Film Emulation — 27 Stocks

All looks are per-channel tone curves (lift, gamma, contrast, colour cast) derived from published densitometry and spectral sensitivity data. Grain is **seeded** — same seed = identical noise every export.

| Group | Stocks |
|---|---|
| **Hasselblad** | Natural Color, XPan B&W |
| **Kodak Color** | Portra 400, Portra 160, Gold 200, UltraMax 400, Ektar 100, Ektachrome E100, Kodachrome 64 |
| **Kodak B&W** | Tri-X 400, T-MAX 100, T-MAX P3200 |
| **Fujifilm Color** | Provia 100F, Velvia 50, Velvia 100, Astia 100F, Superia 400, Pro 400H |
| **Fujifilm B&W** | Acros 100 II, Neopan 400 |
| **Ilford B&W** | HP5 Plus 400, Delta 3200, FP4 Plus 125, XP2 Super 400 |
| **Cinematic** | CineStill 800T (with halation), CineStill 50D, Vision3 500T |

### Adjustments
- Brightness / Contrast / Saturation sliders on top of film look
- Grain seed slider (0–999) for deterministic, repeatable exports

### Undo / Redo
- 50-step history — pan, zoom, rotation, all sliders, look changes, ratio changes, resets

### AI Upscaling
- **Swin2SR 4×** (Microsoft, realworld variant) — runs entirely in-browser via [Transformers.js](https://github.com/huggingface/transformers.js) + ONNX Runtime
- **WebGPU** on Chrome 113+ / Edge, falls back to WASM CPU automatically
- Model (~25 MB) downloads once, cached in IndexedDB — instant on next use
- Prevents resolution loss when cropping a tight panoramic section from a large photo

### Export
- Full native-resolution crop as JPEG (0.95 quality)
- Filename: `xpan_[stock]_seed[n]_[timestamp].jpg`

---

## Usage

1. Open `index.html` in any modern browser — or visit the [live demo](https://im-ghost-dev.github.io/xpan-cropper)
2. Drop a photo onto the canvas or use **Load Photo**
3. Pan and zoom to compose your XPan frame
4. Pick a film stock
5. Adjust sliders as needed
6. Toggle **AI Upscaling** if you want 4× resolution on export
7. Hit **Export Image**

---

## Browser Support

| Browser | Film Looks | AI (WASM) | AI (WebGPU) |
|---|---|---|---|
| Chrome 113+ | ✅ | ✅ | ✅ |
| Edge 113+ | ✅ | ✅ | ✅ |
| Firefox | ✅ | ✅ | ⚠️ Flag |
| Safari 17+ | ✅ | ✅ | ⚠️ Partial |
| Mobile Chrome | ✅ | ✅ | Device-dependent |

HEIC loads natively on **Safari / iOS** only.

---

## File Structure

```
xpan-cropper/
├── index.html                        ← entire tool, self-contained
├── README.md
├── LICENSE
└── docs/
    └── screenshots/
        ├── desktop-ui.png
        └── mobile-ui.jpg
```

---

## Notes

**Single file** — no build step, no dependencies, works from a USB drive or local double-click.

**Film stock disclaimer** — Kodak, Fuji, Hasselblad, and Ilford do not publicly release their proprietary LUT files. Emulations here are built from scratch using published spectral sensitivity data and community measurement projects (darktable, Filmulator, RawTherapee).

**Why Swin2SR not Real-ESRGAN** — Real-ESRGAN's ONNX weights aren't on a CORS-friendly CDN. Swin2SR's `realworld-sr-x4-64-bsrgan-psnr` variant is properly hosted on Hugging Face Hub via Xenova's Transformers.js repo, downloads with progress tracking, and caches to IndexedDB.

---

## Credits

Film emulation parameters from community-measured data.  
[Swin2SR](https://github.com/JingyunLiang/SwinIR) by Microsoft Research · ONNX conversion by [Xenova](https://huggingface.co/Xenova) · [Transformers.js](https://github.com/huggingface/transformers.js) by Hugging Face

---

## License

[MIT](LICENSE) — do whatever you want with it, attribution appreciated.
