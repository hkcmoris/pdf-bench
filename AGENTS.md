# PDF Bench agent instructions

These instructions apply throughout the repository.

## Working agreements

- Always run the format script with `npm run format` after modifying files.
- Always run the lint script with `npm run lint` after modifying files.
- Always run `npm test` after modifying JavaScript files.
- Prefer `npm` when installing dependencies.
- Ask for confirmation before adding new production dependencies.

Install the development tools from the lockfile with `npm ci` before running the checks.

## PDF jobs

- Work from a copy or write to a fresh output path. Keep source PDFs, images, and fonts intact. Commit only fixtures that can be shared.
- Choose tools for the operation at hand. Check the [package guide](PACKAGES.md) before proposing a dependency. If the user requests an editable form, preserve its fields.
- For image placement, read the page dimensions and rotation first. Specify the target size in PDF points (72 per inch), preserve the aspect ratio, and reuse the same embedded image object for a repeated logo.
- Reopen the saved PDF and check page count, order, and dimensions against the request. Render changed pages and inspect margins, image crops, and text at the intended reading size.
- Check features in the saved file. For a form, read the field values back and confirm they display in a viewer. For OCR, search for a phrase visible on the scan. For a link, check its target URL or page. Report the output path and any check you could not perform.
