# FolioBind Zinemaker Studio

### _URL: https://ras0k.github.io/foliobind-zinemaker/_

---

**Zero-Seam PDF & Adobe Illustrator (`.ai`) Booklet Imposition & Multi-Artboard Zinemaker Engine**

FolioBind Zinemaker Studio is a self-contained, zero-backend browser application that converts single-page `.pdf` and PDF-compatible Adobe Illustrator (`.ai`) documents into print-ready **Half-Page (2-Up Saddle-Stitch)** or **Quarter-Page (4-Up Pocket/Zine)** booklets on standard paper stocks (`8.5" × 11"` US Letter, `11" × 17"` Tabloid, `A4`, and `A3`).

Every imposed sheet side is generated with a **Zero-Gutter Seamless Spine Lock** (`0.000"` inner seam) so two-page panoramic spread illustrations join continuously across the center fold without hairline gaps, and can be exported as either a duplex-ready `.pdf` or a multi-artboard `.ai` vector file where **every imposed press side is an individual Adobe Illustrator Artboard**.

---

## Core Capabilities

| Feature | Description |
| :--- | :--- |
| **Native `.PDF` & `.AI` Artboard Ingestion** | Reads standard `.pdf` files and PDF-compatible Adobe Illustrator `.ai` files completely client-side using `pdf-lib` and `PDF.js`. |
| **Half-Page Booklet (`2-Up Saddle-Stitch`)** | Imposes `5.5" × 8.5"` artboards side-by-side onto `11" × 8.5"` (`8.5" × 11"` Landscape) sheets. A **32-page** input produces **16 imposed artboards** (**8 physical sheets** printed two-sided). |
| **Quarter-Page Booklet (`4-Up Quad / Zine`)** | Imposes `4.25" × 5.5"` artboards in a `2 × 2` grid onto `8.5" × 11"` Portrait sheets (8 booklet pages per physical duplex sheet; **32 pages → 8 imposed artboards / 4 physical sheets**). Supports both **2-Tier Cut & Stack** and **Cross-Fold (French-Fold) Signature** ordering. |
| **Zero-Seam Spine Lock (`0.000"` Gutter)** | Anchors the inner spine edges of paired left and right pages flush to the exact center fold line (`x = W / 2`), ensuring two-page panoramic spreads render and print as a single continuous image even when outer safe margins are added. |
| **Inner Spine Seam Trim (Overlap Shave)** | Optional `0.000"`–`0.375"` inner-edge trim shaves unwanted inner white borders from source PDFs so panoramic spread artwork meets cleanly at the fold. |
| **Saddle-Stitch Creep (Shingling) Compensation** | Configurable per-sheet inward shift (`0.000"`–`0.020"`/sheet) compensates for paper bulk on thicker signatures (`20lb Bond`, `80lb Gloss`, `Cardstock`) so fore-edge trims stay aligned. |
| **Interactive Preview & 4× Spine Seam Loupe** | Switch between **Imposed Press Sheets (Artboards)**, **Reader Spread Preview (Seamless Check)**, and the **All Artboards & Duplex Map** schedule, complete with a real-time **4× Spine Seam Loupe** crosshair magnifier across the center fold. |
| **Vector `.PDF` & Multi-Artboard `.AI` Export** | Preserves vector paths, typography, and embedded artwork via `pdfDoc.embedPdf()`, setting explicit `MediaBox`, `TrimBox`, and `ArtBox` boundaries per imposed sheet so Adobe Illustrator opens every imposed side as a standalone artboard. |

---

## Imposition Mathematics (32-Page Example on `8.5" × 11"`)

For a document with $N$ source pages (automatically padded to a multiple of 4 for 2-Up saddle-stitch, or 8 for 4-Up quarter-size):

### 1. Half-Page Booklet (`2-Up Saddle-Stitch`)
- **Finished Booklet Size**: `5.5" × 8.5"` (Half US Letter)
- **Paper Stock**: `8.5" × 11"` Landscape (`11.0" × 8.5"`)
- **Physical Duplex Sheets**: $S = \frac{N}{4}$ (For $N = 32$, $S = 8$ two-sided sheets = **16 imposed artboards**)
- **Page Pairing Formula** for sheet index $s \in \{0, 1, \dots, S - 1\}$:
  - **Front Side (Artboard $2s + 1$)**: $\text{Left} = N - 2s$, $\text{Right} = 2s + 1$
  - **Back Side (Artboard $2s + 2$)**: $\text{Left} = 2s + 2$, $\text{Right} = N - 2s - 1$

| Physical Sheet | Front Artboard (`[Left \| Right]`) | Back Artboard (`[Left \| Right]`) | Role |
| :--- | :--- | :--- | :--- |
| **Sheet 1** | Artboard 1: `[ 32 \| 1 ]` | Artboard 2: `[ 2 \| 31 ]` | Outer Cover Sheet |
| **Sheet 2** | Artboard 3: `[ 30 \| 3 ]` | Artboard 4: `[ 4 \| 29 ]` | Signature Sheet 2 |
| **Sheet 3** | Artboard 5: `[ 28 \| 5 ]` | Artboard 6: `[ 6 \| 27 ]` | Signature Sheet 3 |
| **Sheet 4** | Artboard 7: `[ 26 \| 7 ]` | Artboard 8: `[ 8 \| 25 ]` | Signature Sheet 4 |
| **Sheet 5** | Artboard 9: `[ 24 \| 9 ]` | Artboard 10: `[ 10 \| 23 ]` | Signature Sheet 5 |
| **Sheet 6** | Artboard 11: `[ 22 \| 11 ]` | Artboard 12: `[ 12 \| 21 ]` | Signature Sheet 6 |
| **Sheet 7** | Artboard 13: `[ 20 \| 13 ]` | Artboard 14: `[ 14 \| 19 ]` | Signature Sheet 7 |
| **Sheet 8** | Artboard 15: `[ 18 \| 15 ]` | Artboard 16: `[ 16 \| 17 ]` ★ | Inner Centerfold Sheet |

### 2. Quarter-Page Booklet (`4-Up Cut & Stack` or `Cross-Fold`)
- **Finished Booklet Size**: `4.25" × 5.5"` (Quarter US Letter)
- **Paper Stock**: `8.5" × 11"` Portrait (`2 × 2` quad layout)
- **Physical Duplex Sheets**: $S = \frac{N}{8}$ (For $N = 32$, $S = 4$ two-sided sheets = **8 imposed artboards**)

---

## Quick Start

### Run Locally
No build step, server, or package installation is required:

1. Clone or download this repository.
2. Open `booklet_imposition_studio.html` (or `index.html`) directly in any modern browser (Chrome, Edge, Firefox, Safari).
3. Drag and drop any `.pdf` or PDF-compatible `.ai` file onto the upload zone—or test immediately using the built-in **32p (`5.5×8.5"`)**, **16p Lookbook**, or **32p Quarter Zine** panoramic spread generators.
4. Click **Export Booklet .PDF** for duplex printing or **Export .AI Artboards** to open all imposed sheets as individual artboards in Adobe Illustrator.

### Deploy on GitHub Pages
1. Push this repository to GitHub (`index.html` and `booklet_imposition_studio.html` are included at the root).
2. In your GitHub repository, navigate to **Settings → Pages**.
3. Under **Build and deployment**, select **Deploy from a branch**, choose `main` and `/ (root)`, and click **Save**.
4. Your live studio will be published at `https://<your-username>.github.io/<repository-name>/`.

---

## Printing & Adobe Illustrator Workflow Tips

- **Two-Sided (Duplex) Printing**:
  - For **Half-Page (`2-Up` Landscape)** booklets, set your printer driver to **Print at 100% / Actual Size** and **Two-Sided: Flip on Short Edge** (or select **Flip on Long Edge (Auto-Rotate Back 180°)** inside FolioBind if your printer defaults to long-edge binding).
- **Eliminating Existing Inner Borders on Panoramic Spreads**:
  - If your source PDF was exported with white inner margins that interrupt a two-page photo spread, keep **Eliminate Center Spine Seam** checked and gradually increase **Inner Edge Seam Trim / Overlap** (`0.005"` increments) while watching the **4× Spine Seam Loupe** until both halves kiss cleanly at the center fold.
- **Opening `.AI` Exports in Adobe Illustrator**:
  - Open the exported `.ai` file directly in Adobe Illustrator (`File → Open`). Every imposed press side (`Artboard 1` through `Artboard 16`) opens as a separate, accurately sized `11" × 8.5"` (or `8.5" × 11"`) vector artboard with intact `TrimBox` and `ArtBox` coordinates.

---

## Tech Stack

- **HTML5 Canvas & SVG** — High-DPI interactive press sheet renderer & 4× spine seam inspection loupe
- **[pdf-lib](https://pdf-lib.js.org/) (`v1.17.1`)** — Client-side vector PDF/AI page embedding, artboard box geometry (`MediaBox`, `TrimBox`, `ArtBox`), and document generation
- **[PDF.js](https://mozilla.github.io/pdf.js/) (`v3.11.174`)** — Client-side source artboard rasterization for live preview
- **[Tailwind CSS](https://tailwindcss.com/) (`v3.4.17`)** — Responsive dark-room zinemaker studio interface

## License

Released under the MIT License.
