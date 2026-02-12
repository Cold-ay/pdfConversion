# Universal Doc Converter

A browser-based document conversion tool that runs entirely in the client. No server uploads—all processing happens locally.

## Features

| Mode | Input | Output |
|------|-------|--------|
| **PDF to Image** | PDF file | PNG, JPEG, or TIFF per page |
| **Image to PDF** | Multiple images | Single PDF |
| **Word to PDF** | .docx file | PDF |

## How It Works

### Architecture

```
User uploads file → Mode-specific processor → Output (download or gallery)
```

### 1. PDF to Image

- **Library**: PDF.js (renders PDF pages to canvas)
- **Flow**:
  1. Load PDF using `pdfjsLib.getDocument()`
  2. For each page: render to a canvas with the selected scale (1x, 1.5x, 2x)
  3. Export canvas as PNG, JPEG, or TIFF (TIFF via UTIF.js)
  4. Show a gallery with per-page preview and download buttons

### 2. Image to PDF

- **Library**: jsPDF
- **Flow**:
  1. Read each image as a data URL
  2. Add images to a jsPDF document (one per page)
  3. Page size options:
     - **A4**: Fit image to A4 (width-based or aspect-fitted)
     - **Fit**: Scale image to fit A4 while keeping aspect ratio

### 3. Word to PDF

- **Libraries**: Mammoth.js → html2canvas → jsPDF
- **Flow**:
  1. Mammoth converts .docx to HTML
  2. HTML is rendered into a hidden off-screen div
  3. html2canvas captures the div as an image
  4. jsPDF creates a multi-page PDF from the image (split by A4 height)

## Key Functions

| Function | Purpose |
|----------|---------|
| `switchMode(mode)` | Switches between the three tools and updates UI/hints |
| `handleFiles(e)` | Validates files and calls the correct processor |
| `processPdfToImg(file)` | PDF → images |
| `processImgToPdf(files)` | Images → single PDF |
| `processWordToPdf(file)` | Word → PDF |
| `saveBlobWithPicker()` | Saves file via File System Access API or fallback download |

## Dependencies (CDN)

- **Tailwind CSS** – styling
- **PDF.js** – read PDFs
- **UTIF.js** – TIFF encoding
- **jsPDF** – create PDFs
- **Mammoth.js** – Word (.docx) to HTML
- **html2canvas** – HTML to canvas
- **Lucide** – icons

## Usage

1. Open `index.html` in a modern browser.
2. Choose a mode (PDF to Image, Image to PDF, or Word to PDF).
3. Drag and drop files or click the upload button.
4. Download the converted files.

## Notes

- All processing is client-side; files never leave your machine.
- `showSaveFilePicker` (File System Access API) is used when available for better save dialogs.
- Word conversion preserves layout via HTML rendering; complex formatting may not match exactly.
