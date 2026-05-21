# PDF Converter — Specification

## 1. Project Overview

**Project Name:** PDF Converter
**Type:** Single-page web application
**Core Functionality:** Upload a PDF file and convert it to either Word (.docx) or PowerPoint (.pptx) format, with downloadable output.
**Target Users:** General users who need to convert PDF documents to editable formats.

## 2. Technical Architecture

Since this deploys to GitHub Pages (static hosting), all processing happens client-side using browser-based libraries:

| Purpose | Library |
|---------|---------|
| PDF Text Extraction | pdfjs-dist |
| Word Document Generation | docx |
| PowerPoint Generation | officegen |

### File Structure
```
pdf-converter/
├── index.html          # Single-page application
├── SPEC.md             # This specification
└── README.md          # Project documentation
```

## 3. Visual & UI Specification

### Layout
- Full-viewport single page, centered content
- Dark charcoal background with warm gold accent (#d4a853)
- Max content width: 640px

### Color Palette
- Background: #1a1a2e
- Surface/Cards: #16213e
- Primary accent: #d4a853 (gold)
- Secondary accent: #e94560 (coral red)
- Text primary: #eaeaea
- Text muted: #a0a0a0

### Typography
- Headings: "Noto Serif SC" (Google Fonts) — elegant serif for document theme
- Body: "IBM Plex Sans" — clean, technical sans-serif
- Fallback: Georgia, system-ui

### Visual Effects
- Subtle gradient on hero section
- Smooth hover transitions on buttons (0.3s ease)
- Card elevation with box-shadow
- File drop zone with dashed animated border
- Progress bar with gradient fill during conversion

### Components

#### Header
- App title "PDF Converter" with gold accent
- Subtitle explaining functionality

#### Upload Zone
- Large dashed border drop area
- Icon indicating drag-and-drop or click to upload
- States: idle, hover (highlighted border), file-loaded (shows filename)
- Accepts .pdf files only

#### Format Selector
- Two toggle cards: "Word" and "PowerPoint"
- Radio-style selection (one must be selected)
- Visual indication of selected format

#### Action Button
- "Convert & Download" primary button
- Disabled until file is uploaded
- Loading state with spinner during conversion

#### Progress Indicator
- Horizontal progress bar
- Shows conversion percentage
- Gradient fill animation

#### Footer
- Simple credits line
- Disclaimer about file processing

## 4. Functional Specification

### Core Features

1. **PDF Upload**
   - Drag-and-drop onto drop zone
   - Click to open file browser
   - Accept only .pdf files
   - Display selected filename after upload
   - Show file size

2. **Format Selection**
   - Toggle between "Word Document (.docx)" and "PowerPoint (.pptx)"
   - Default: Word Document
   - Visual feedback on selection change

3. **Conversion Process**
   - Extract text content from PDF using pdf.js
   - Generate target format document
   - For Word: Create structured document with headings, paragraphs
   - For PowerPoint: Create slides with text content (approximately 1 slide per page/section)

4. **Download**
   - Automatic download prompt after conversion completes
   - Filename: original name with new extension
   - Browser native download behavior

### User Flow
```
1. User lands on page
2. User drags PDF file onto drop zone (or clicks to browse)
3. PDF file is validated (must be .pdf)
4. User selects output format (Word or PowerPoint)
5. User clicks "Convert & Download"
6. Progress bar shows conversion progress
7. On completion, file downloads automatically
8. User can convert another file
```

### Conversion Logic

#### PDF to Word
- Extract all text content preserving reading order
- Group text into paragraphs
- Apply basic formatting (bold headers based on font size)
- Create proper Word document structure using docx library

#### PDF to PowerPoint
- Extract text content
- Chunk content into logical slides (by page or content sections)
- Create title slide with "Converted from [filename]"
- Add content slides with extracted text
- Use officegen library for .pptx generation

### Edge Cases
- Empty PDF (no text): Show warning message
- Password-protected PDF: Show error, suggest unprotecting first
- Very large PDF (>50MB): Show warning about browser memory
- Extraction failure: Show error with suggestion to try again

## 5. Acceptance Criteria

- [ ] Page loads without errors on modern browsers
- [ ] PDF files can be uploaded via drag-drop and file browser
- [ ] Only .pdf files are accepted (others rejected with message)
- [ ] Format selector allows choosing between Word and PowerPoint
- [ ] Conversion produces valid .docx files (openable in Microsoft Word)
- [ ] Conversion produces valid .pptx files (openable in PowerPoint)
- [ ] Download starts automatically after successful conversion
- [ ] Progress indicator shows during conversion
- [ ] Error messages display for invalid PDFs or conversion failures
- [ ] Application works when served from GitHub Pages URL
- [ ] No console errors in production build