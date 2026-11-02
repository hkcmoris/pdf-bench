# PDF Bench

A local workbench for making PDFs and working with documents you already have. Start here to put an image on a page or append it to an existing PDF. For jobs such as filling forms, laying out a long report, or making a scan searchable, see the [free package guide](PACKAGES.md).

## Make a one-page image proof

You need Node.js with npm and a PNG of your choice. Open a terminal in an empty job folder outside this checkout, then run the commands below. They install the one package the example uses. [Download Node.js LTS](https://nodejs.org/en/download)

```sh
npm init -y
npm install pdf-lib
```

Place your image in the job folder as `figure.png`. Save the following as `image-proof.cjs` in the same folder.

```js
const { readFileSync, writeFileSync } = require("node:fs");
const { PDFDocument, PageSizes } = require("pdf-lib");

async function makeProof() {
    const pdf = await PDFDocument.create();
    const page = pdf.addPage(PageSizes.A4);
    const image = await pdf.embedPng(readFileSync("figure.png"));

    const { width, height } = page.getSize();
    const margin = 36; // PDF points, half an inch
    const imageTop = height - margin - 32;
    const size = image.scaleToFit(width - 2 * margin, imageTop - margin);

    page.drawText("Image proof", {
        x: margin,
        y: height - margin - 18,
        size: 18,
    });
    page.drawImage(image, {
        x: (width - size.width) / 2,
        y: imageTop - size.height,
        ...size,
    });
    writeFileSync("proof.pdf", await pdf.save(), { flag: "wx" });
}

makeProof().catch((error) => {
    console.error(error.message);
    process.exitCode = 1;
});
```

Run `node image-proof.cjs` and open `proof.pdf`. You should see one A4 page with a selectable title and the whole image centered horizontally beneath it. The script preserves the image proportions and refuses to overwrite an existing proof. Choose a fresh output name for another run.

**For a JPEG**, use `pdf.embedJpg(readFileSync("figure.jpg"))` on the image-loading line.

**To append the image sheet to an existing document**, replace `PDFDocument.create()` with `PDFDocument.load(readFileSync("source.pdf"))` on the first line inside `makeProof`. Keep the `await` and the `addPage` call. The new file should contain the original pages in order, followed by the image sheet. [pdf-lib examples](https://pdf-lib.js.org/)

## Working here

Keep reusable scripts and redistributable example assets in the repo. Keep personal PDFs, scans, and temporary page previews in the job folder. The [agent instructions](AGENTS.md) cover dependency changes and how to check a finished PDF. The repository uses the [MIT license](LICENSE).

For repository maintenance, use Node.js 22 or newer and run `npm ci` in the checkout. Run `npm run format` after edits and `npm run lint` to check formatting and Markdown. For JavaScript changes, also run `npm test`.
