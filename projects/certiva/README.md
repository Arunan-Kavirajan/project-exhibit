# Certiva

> **Certificates, without the chaos.**

A browser-based certificate automation platform that turns a repetitive certificate-generation workflow into a visual, automated process.

**[🚀 Live Demo](https://certiva-seven.vercel.app)** · **🔒 Source Code Private**

**Status:** Completed
**Platform:** Web
**Built with:** React · TypeScript · Vite · PDF.js · pdf-lib

---

## Overview

Generating hundreds of certificates manually can become a surprisingly tedious process.

A typical workflow involves editing a certificate template, copying participant names, positioning text, exporting individual PDFs, and repeating the process for every participant.

**Certiva** automates that entire workflow inside the browser.

Users upload a certificate template and participant spreadsheet, visually define where dynamic information should appear, map those fields to spreadsheet columns, preview the result, and generate the complete batch of personalized certificates.

The entire process happens locally in the browser.

---

## The Problem

Certificate generation often involves a simple but painful loop:

```text
Spreadsheet
    ↓
Copy participant name
    ↓
Open certificate
    ↓
Position text
    ↓
Export PDF
    ↓
Repeat × hundreds
```

Certiva turns that into:

```text
PDF + Spreadsheet
       ↓
Visual Field Setup
       ↓
Preview
       ↓
Bulk Generation
       ↓
ZIP Download
```

The goal was not simply to automate PDF generation, but to make the process **visual, flexible, and accessible without requiring users to write code**.

---

## What I Built

### 📄 PDF Template Upload

Users can upload an existing certificate design as a PDF through a drag-and-drop interface.

The original design becomes the base template for every generated certificate.

### 📊 Spreadsheet Import

Participant information can be imported from:

* XLSX files
* CSV files
* Raw Google Forms exports

The spreadsheet columns become available for mapping to certificate fields.

### ✏️ Visual Field Editor

Instead of entering coordinates manually, users can draw, drag, and resize fields directly over the certificate.

For example:

```text
┌─────────────────────────────────────┐
│                                     │
│            CERTIFICATE              │
│                                     │
│       ┌───────────────────┐         │
│       │     ARUNAN        │         │
│       └───────────────────┘         │
│                                     │
│       ┌───────────────────┐         │
│       │  Web Development  │         │
│       └───────────────────┘         │
│                                     │
│              20/08/2026             │
│                                     │
└─────────────────────────────────────┘
```

Each field can independently control:

* Font family
* Font size
* Minimum font size
* Text alignment
* Spreadsheet mapping
* Fixed text

### 👀 Live Preview

Before generating the entire batch, users can preview how the first certificate will look.

This allows field placement, formatting, and text fitting to be verified before committing to a large generation job.

### ⚙️ Bulk Generation

Certiva processes every spreadsheet row and creates a personalized certificate for each participant.

The system also handles individual failures without stopping the entire batch.

If one certificate fails, the remaining certificates continue generating and the user receives a report explaining what went wrong.

### 📦 ZIP Export

All generated certificates can be downloaded together as a single ZIP archive.

---

## How It Works

```text
┌──────────────────────┐
│   Upload Template    │
│       + Data         │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│   Visual Editor      │
│                      │
│ Draw fields          │
│ Map columns          │
│ Configure typography │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│    Live Preview      │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│   Bulk Generation    │
│                      │
│ Spreadsheet rows     │
│        ↓             │
│ Personalized PDFs    │
└──────────┬───────────┘
           ↓
┌──────────────────────┐
│   Error Report       │
│         +            │
│    ZIP Export        │
└──────────────────────┘
```

---

## Architecture

Certiva is designed as a fully client-side application.

```text
                     Browser
                        │
        ┌───────────────┼────────────────┐
        │               │                │
        ▼               ▼                ▼
     PDF.js           XLSX           React UI
        │               │                │
        │               │                ▼
        │               │        Certificate Context
        │               │                │
        └───────────────┼────────────────┘
                        │
                        ▼
                     pdf-lib
                        │
                        ▼
                Generated PDFs
                        │
                        ▼
                     JSZip
                        │
                        ▼
                  ZIP Download
```

No certificate files or participant spreadsheets need to be uploaded to a backend.

---

## Fully Client-Side

One of the most important architectural decisions was keeping the entire workflow inside the browser.

```text
User Files
    │
    ├── Certificate PDF
    └── Participant Spreadsheet
             │
             ▼
          Browser
             │
     ┌───────┴────────┐
     ▼                ▼
   Parse            Render
     │                │
     └───────┬────────┘
             ▼
       Generate PDFs
             │
             ▼
         ZIP Archive
             │
             ▼
          Download
```

This means participant information and certificate files do not need to leave the user's device.

It also removes the need for a dedicated backend for the core generation workflow.

---

## Tech Stack

| Technology                 | Purpose                                     |
| -------------------------- | ------------------------------------------- |
| **React**                  | User interface                              |
| **TypeScript**             | Type-safe application development           |
| **Vite**                   | Development and production build tooling    |
| **Tailwind CSS**           | UI styling                                  |
| **React Router**           | Application navigation                      |
| **react-pdf / pdfjs-dist** | PDF rendering                               |
| **pdf-lib**                | PDF manipulation and certificate generation |
| **xlsx**                   | XLSX and CSV parsing                        |
| **JSZip**                  | ZIP archive generation                      |
| **react-dropzone**         | File upload interactions                    |

---

## Application Structure

```text
src/
├── components/
│   ├── pdf/
│   │   ├── PdfViewer.tsx
│   │   └── FieldBox.tsx
│   │
│   ├── shared/
│   │   ├── Navbar.tsx
│   │   └── MobileBlocker.tsx
│   │
│   └── upload/
│       ├── PdfUploader.tsx
│       └── SpreadsheetUploader.tsx
│
├── context/
│   └── CertificateContext.tsx
│
├── pages/
│   ├── LandingPage.tsx
│   ├── UploadPage.tsx
│   ├── TemplateEditorPage.tsx
│   ├── GeneratingPage.tsx
│   └── DownloadPage.tsx
│
├── types/
│   └── certificate.ts
│
├── utils/
│   ├── generateCertificate.ts
│   └── generateAllCertificates.ts
│
└── App.tsx
```

---

## Engineering Highlights

### Coordinate-based PDF rendering

The visual editor allows users to interact with fields in screen coordinates while the generated PDF requires accurate document coordinates.

The application therefore has to translate visual field positions into the coordinate system used by the PDF.

### Automatic text shrinking

Certificate names can vary dramatically in length.

Instead of allowing long names to overflow their designated field, Certiva supports a minimum font size and automatically reduces the font size when necessary.

### Per-field configuration

Each certificate field can have its own formatting and data source.

For example:

```text
Name
├── Spreadsheet column: Participant Name
├── Font: ...
├── Size: 32
├── Minimum: 18
└── Alignment: Center

Event
├── Spreadsheet column: Event
├── Font: ...
├── Size: 20
└── Alignment: Center

Date
├── Fixed text: 20 August 2026
└── Alignment: Center
```

This makes the editor useful for templates that contain both dynamic and static information.

### Failure-tolerant batch processing

A single malformed row should not destroy an entire certificate-generation job.

Certiva therefore tracks individual failures while allowing the rest of the batch to continue.

The final result reports both successful and failed generations.

---

## Design Direction

Certiva uses a warm editorial visual identity inspired by physical stationery and certificate design.

The interface combines:

* Warm cream backgrounds
* Editorial serif typography
* Clean sans-serif UI text
* Olive-green primary accents
* Spacious layouts
* Minimal interface chrome

The design intentionally avoids the appearance of a generic enterprise dashboard.

---

## Desktop-First Experience

The certificate editor depends heavily on precise mouse interactions for positioning and resizing fields.

Rather than forcing a cramped mobile version of the editor, Certiva explicitly detects narrow screens and displays a dedicated notice asking the user to switch to a larger display.

This keeps the editing experience focused on the environment it was designed for.

---

## What This Project Demonstrates

Certiva provided hands-on experience with:

* React application architecture
* TypeScript
* Client-side file processing
* PDF rendering and manipulation
* Coordinate systems
* Spreadsheet parsing
* Bulk document generation
* ZIP archive creation
* Error-tolerant batch processing
* Drag-and-drop interfaces
* Interactive resizing and positioning
* Responsive design decisions
* Building privacy-conscious browser applications

---

## Project Status

**Completed**

The core certificate upload, spreadsheet mapping, visual editing, preview, bulk generation, error handling, and ZIP export workflows are implemented.

---

## Source Code

🔒 **Private repository**

The source code is intentionally kept private. This page documents the product, architecture, engineering decisions, and capabilities without exposing the implementation.

---

## Author

**Arunan Kavirajan**

IT undergraduate at SRM Institute of Science and Technology, Chennai.

Building software, experimenting with AI, and turning ideas into working products.

[GitHub](https://github.com/Arunan-Kavirajan) · [LinkedIn](https://www.linkedin.com/in/arunan-kavirajan)
