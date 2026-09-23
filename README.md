# [Tập viết](tapviet.500kv.co)

**A single HTML page that turns Vietnamese words and sentences into a printable handwriting
workbook for children.** Type the lesson, pick the font and the tracing style, preview every
page, then print or save as PDF. No server, no build step, no account.

File: `index.html`. Open it in Chrome or Edge, from disk or from any static server.

---

## What it does

1. **Takes the lesson as text.** One item per line: a letter, a syllable, a word, or a full
   sentence. A title and a keyword line go above the ruled rows.
2. **Lays each item on ruled rows** sized to the Ministry of Education handwriting model
   (Decision 31/2002/QĐ-BGDĐT): lowercase letters fill two cells, tall letters reach the fifth
   cell, descenders drop three cells below the baseline.
3. **Renders every copy in a tracing style** you choose: dim, dotted, dashed, outlined, or a
   sequence that mixes them across the row.
4. **Paginates and prints** at exact millimetre size on A4, A5, or Letter.

## Quick start

1. Open `tap-viet.html`.
2. Type the lesson in the text box under **Nội dung**. The sample lesson loads on first visit.
3. Choose a tracing sequence under **Nét tô**. The default is dim, then dotted, then dashed.
4. Press **In / Lưu PDF**. In the print dialog set margins to **None** and scale to **100%**.
   Choose "Save as PDF" as the printer to get a file.

Settings and content persist in the browser between visits.

## Writing the lesson

Each non-empty line of the text box is one item. Three prefixes change how a line renders.

| Line | Result |
|---|---|
| `bé` | Short item. Repeats across the row, spread evenly. |
| `# Âm b` | Heading. Printed dark in the handwriting font, not traced. |
| `-` | One empty ruled row for free writing. |
| `> bé bi bô` | Sentence mode forced: written once per row, wrapped, repeated N rows. |
| `bé bi bô, bà bế bé.` | A line ending in `.`, `!`, `?` or `…` uses sentence mode automatically. |

Short items fill one row by default. Raise **Từ ngắn: số dòng** to give each word more rows.
Sentences repeat for **Câu dài: số lần viết** rows.

## Tracing styles

Every copy gets a one-letter style code. The sequence cycles across copies on a row, or across
rows for a sentence. With **Chữ mẫu đậm ở lần đầu** on, the first copy is always the dark
sample and the sequence starts from the second copy.

| Code | Name in the page | Rendering |
|---|---|---|
| `D` | Đậm | Solid dark ink, the model to copy. |
| `M` | Mờ | Solid fill at the lightness you set. |
| `C` | Chấm bi | Glyph filled with a dot pattern. |
| `G` | Nét đứt | Glyph filled with diagonal cuts, reads as a dashed stroke. |
| `V` | Viền chấm | Dotted outline, no fill. |
| `N` | Viền gạch | Dashed outline, no fill. |
| `T` | Trống | Empty slot for the child to write alone. |

Pick a preset from **Dãy kiểu nét** or choose **Tùy chỉnh** and type any string of codes,
for example `MCGT`. Sliders set the ink lightness, tint colour, dot size, dash size, and
outline width. All of them apply to the print, not only the preview.

## Fonts

The default font is **Playwrite VN**, a cursive typeface by TypeTogether built on the
Vietnamese primary school handwriting model, with complete diacritics and tone marks. It is
embedded in the HTML under the SIL Open Font License, so the page prints the same offline and on
any machine. Weight 100 to 400 is adjustable; lighter weights suit outline styles, heavier
weights suit dotted fills.

Fifteen more Google Fonts with a verified Vietnamese subset are listed, split into cursive
(Dancing Script, Great Vibes, Allura, Pattaya, Charm, Pacifico) and print (Playpen Sans, Andika,
Patrick Hand, Mali, Itim, Sriracha, Be Vietnam Pro, Nunito). These load from Google Fonts and
need a network connection the first time.

**Your own font.** Choose **Phông tự chọn** and pick a `.ttf`, `.otf`, `.woff` or `.woff2` file.
This is how to use a school font you already own, such as the HP001 family. The page keeps the
file in browser storage when it fits, otherwise you pick it again next visit.

**Sizing.** By default the page measures the chosen font and scales it so the letter `x` is
exactly two cells tall. Switch **Căn cỡ theo** to `chữ h` to scale tall letters to the row
height instead, which helps fonts whose ascenders run long.

## Ruling

| Setting | Default | Meaning |
|---|---|---|
| Kiểu kẻ | Ô li | Full square grid. `Kẻ ngang` draws only horizontals. `4 dòng kẻ` draws top, x-height, baseline and bottom lines. |
| Ô li (mm) | 2.5 | Cell size. Matches a grade 1 exercise book. |
| Ô li trên dòng | 5 | Cells above the baseline (2.5 units). |
| Ô li dưới dòng | 3 | Cells below the baseline (1.5 units). |
| Cách giữa các dòng | 0 | Extra cells between rows. 0 gives a continuous notebook grid. |
| Màu kẻ | Xanh | Line tint. The baseline is always drawn darker. |

## Page

A4, A5 or Letter, portrait or landscape, margin in millimetres. Toggles for the title block, the
name / class / date line, and page numbers. The page count and the computed font size show above
the preview.

## How it works

- Each page is one inline `<svg>` whose viewBox is in millimetres, so `1 unit = 1 mm` on paper.
  A `@page { size }` rule matches the sheet, and the print stylesheet hides everything but the
  pages.
- Font metrics come from a canvas `measureText` pass on `x`, `n`, `u`, `v`, `h`, `g` after
  `document.fonts.load` resolves for the lesson text. Widths for row packing and word wrapping
  use the same pass.
- Dotted and dashed fills are SVG `<pattern>` elements defined per page and referenced from the
  text `fill`. Outline styles use `stroke-dasharray` on the glyph path.
- State lives in `localStorage` under `tapviet.v1`; a custom font lives under `tapviet.font.v1`
  as base64.

## Known limits

- Print scale must be 100% with no browser margins, or the cells shrink. The page says so under
  the print button.
- Tone marks over tall capital letters can rise above the top line. That matches paper books.
- Dotted and dashed fills follow the glyph outline, not the pen path. A single-stroke school
  font loaded through **Phông tự chọn** gives the truest dashed strokes.
- Tested in Chromium browsers. Firefox prints SVG patterns but may ignore `@page size`.
