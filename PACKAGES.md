# Free tools for PDF Bench

## First download

Start with **`pdf-lib`** (MIT). It can create a PDF, embed PNG and JPEG images, copy or reorder pages, draw text and graphics, and fill standard PDF form fields (AcroForms). One local npm install covers both the image proof and the append-pages version in the [README](README.md). Its text APIs can draw new text and read form values. To extract paragraphs already printed on a page, use Poppler below. [pdf-lib documentation](https://pdf-lib.js.org/) · [Limitations](https://github.com/Hopding/pdf-lib#limitations) · [License](https://github.com/Hopding/pdf-lib/blob/master/LICENSE.md)

```sh
npm install pdf-lib
```

Use an empty job folder for experiments. In the checkout itself, `npm ci` installs Prettier and markdownlint for maintaining the docs. PDF tools are selected per job. To add one as a repository production dependency, follow the [working agreements](AGENTS.md).

## Add a tool when the job calls for it

| Job                                               | Free tool                | When to download it                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Lay out a long report                             | PDFKit (MIT)             | Use `npm install pdfkit` for reports with tables and paragraphs that flow across pages. It also embeds fonts and images. [PDFKit guide](https://pdfkit.org/) · [License](https://github.com/foliojs/pdfkit/blob/master/LICENSE)                                                                                                                                                       |
| Use a custom font with `pdf-lib`                  | `@pdf-lib/fontkit` (MIT) | Use `npm install @pdf-lib/fontkit` when you need a particular typeface or characters missing from a built-in font. Register it with `pdf.registerFontkit(...)` and embed a font file that covers the characters and permits embedding. [Font example](https://github.com/Hopding/pdf-lib#embed-font-and-measure-text) · [Fontkit license](https://github.com/Hopding/fontkit#license) |
| Inspect, render, or extract text                  | Poppler utilities (GPL)  | One download gives you `pdfinfo` for page dimensions, `pdfimages -list` for embedded image details, `pdftoppm` for page previews, and `pdftotext` for an existing text layer. [Ubuntu package and file list](https://packages.ubuntu.com/noble/poppler-utils) · [License](https://formulae.brew.sh/formula/poppler)                                                                   |
| Assemble or check existing PDFs from the terminal | qpdf (Apache-2.0)        | Useful for a one-off page selection, merge, or structural check. On Windows, download the matching binary from the official releases. [Windows downloads](https://github.com/qpdf/qpdf/releases) · [Command reference](https://qpdf.readthedocs.io/en/stable/cli.html) · [License](https://qpdf.readthedocs.io/en/stable/license.html)                                                |
| Make an image-only scan searchable                | OCRmyPDF (MPL-2.0)       | Adds a searchable text layer using Tesseract. Install the recognition language for the scan. The Ubuntu package brings in the required system tools. [Installation by platform](https://ocrmypdf.readthedocs.io/en/stable/installation.html) · [Project and license](https://github.com/ocrmypdf/OCRmyPDF#license)                                                                    |

On Windows with Ubuntu in WSL, run `sudo apt update` in the Ubuntu shell, then install only the tool you picked with `sudo apt install poppler-utils`, `sudo apt install qpdf`, or `sudo apt install ocrmypdf`. Run that tool from the same shell, in the folder containing the PDFs. The npm examples also work directly in PowerShell.

## Try the tool on a real job

- **Check the image proof.** Run `pdfinfo proof.pdf` to confirm the page count and A4 size. Run `pdfimages -list proof.pdf` to inspect the embedded image's pixel dimensions and effective resolution. Then run `pdftoppm -r 144 -png proof.pdf preview` and open the resulting PNG to check the placement.
- **Assemble two documents.** For ordinary PDFs, `qpdf --empty --pages cover.pdf report.pdf -- combined.pdf` writes the cover pages followed by the report pages. This assembles pages into a new document and discards the source document-level metadata. Run `qpdf --check combined.pdf` for syntax and stream errors, then open it and verify the page order. [qpdf page selection](https://qpdf.readthedocs.io/en/stable/cli.html#page-selection)
- **Recover searchable text.** Try `pdftotext scan.pdf text.txt` first. If some pages are image-only, run `ocrmypdf --skip-text -l eng scan.pdf searchable.pdf` for an English scan. This skips pages that already have text. Search the result for a distinctive phrase from a scanned page and check the spelling. [OCRmyPDF cookbook](https://ocrmypdf.readthedocs.io/en/stable/cookbook.html)
