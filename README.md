# 🧩 Laplacian and Gaussian Pyramid: Image Compositing

Turns two ordinary photos into one seamless composite by decomposing them into Gaussian and Laplacian pyramids and blending each frequency band separately, demonstrated across five example pairs.

---

## 📑 Table of Contents

- [About the Project](#-about-the-project)
- [Highlights](#-highlights)
- [The Technique](#-the-technique)
- [Gallery of Blends](#-gallery-of-blends)
- [Repository Layout](#-repository-layout)
- [Quickstart](#-quickstart)
- [Core Functions](#-core-functions)
- [Prerequisites](#-prerequisites)
- [Takeaways](#-takeaways)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

---

## ✨ Main Components

- Gaussian pyramid built via repeated blur-and-downsample
- Laplacian pyramid built by subtracting each Gaussian level from the next
- Supports both hard binary masks and soft alpha masks
- Mask blending done level-by-level across the full pyramid stack
- Reconstruction verified numerically against the original image
- Handles arbitrary image dimensions, not just powers of two

---

## 🔬 Algorithm Overview

1. **Downsample** each source image into a Gaussian pyramid (blur, then halve the resolution, repeat).
2. **Extract detail** at each level to build a Laplacian pyramid the difference between a Gaussian level and the next level expanded back up.
3. **Build a mask pyramid** the same way, from the composite mask.
4. **Blend** the two Laplacian pyramids level-by-level, weighted by the mask pyramid at that resolution.
5. **Collapse** the blended pyramid back into a full-resolution image by repeatedly upsampling and adding detail back in.

---

## 🎨 Gallery of Blends

| # | Pair | Mask Style |
|---|------|-----------|
| 1 | Tiger & Bear | Vertical split (binary) |
| 2 | Coffee & Tea | Vertical split (binary) |
| 3 | Black Panther & Tiger | Vertical split (binary) |
| 4 | Fox & Cat | Horizontal split (binary) |
| 5 | Airplane over Ocean | Custom object cutout (soft alpha) |

---

## 🚀 Steps to run

**1. Install the dependencies**
```bash
pip install numpy pillow matplotlib jupyter
```

**2. Launch the notebook**
```bash
jupyter notebook Laplacian_and_Gaussian_Pyramid.ipynb
```
(JupyterLab, VS Code, and Google Colab work too.)

**3. Run it top to bottom**
Each section builds on the last Gaussian pyramid, then Laplacian pyramid, then mask, then blend, then reconstruction check across all five examples.

---

## 🧠 Core Functions

| Function | What it does |
|---|---|
| `gaussian_pyramid(img, levels)` | Builds the Gaussian pyramid |
| `laplacian_pyramid(gpyramid)` | Derives the Laplacian pyramid from a Gaussian pyramid |
| `reconstruct(lpyramid)` | Collapses a Laplacian pyramid back into a full image |
| `vertical_split_mask` / `horizontal_split_mask` | Build a simple binary composite mask |
| `blend_pyramids(la, lb, gmask)` | Combines two Laplacian pyramids using a mask's Gaussian pyramid |
| `pyramid_blend(img_a, img_b, mask, levels)` | Runs the full pipeline end to end |

---

## 🛠️ Prerequisites

- Python 3
- NumPy
- Pillow (PIL)
- Matplotlib
- Jupyter Notebook / JupyterLab (or VS Code with the Jupyter extension)

---

## 🎓 Takeaways

- **The upsampling math has to compensate for zero-interleaving** : multiplying the blur kernel by 4 total energy is what keeps reconstruction lossless; skipping this scaling silently corrupts every level above the base.
- **A single hard blend always shows its seam; a pyramid blend doesn't** : because low frequencies (color, lighting) get blended broadly while high frequencies (edges, texture) stay local to the mask boundary.
- **Masks don't have to be binary** : the airplane composite uses a continuous alpha mask, and the exact same blending pipeline handles it without modification.
- **Non-power-of-two images aren't a special case** : as long as exact shapes are tracked at every pyramid level, the math holds regardless of image dimensions.

---

## 🤝 Contributing

If you have any suggestions or want to improve the project, feel free to fork it, make your changes and submit a pull request.

---

## 🔒 License

This project is licensed under the [MIT License](./LICENSE).

---

## 📧 Contact

If you have any questions or concerns, please don't hesitate to contact me via email at imam220826@gmail.com
