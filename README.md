# Facturi QR

A shared invoice book for Swiss QR-bills, with the interface in Romanian.

- Scan a photo, screenshot or PDF of a Swiss QR-bill. The creditor, IBAN, amount, reference and message are read from the QR code.
- Check IBANs (including QR-IBANs), 27-digit QR references and RF creditor references.
- Show each bill's payment slip with the Swiss QR code, so it can be paid by scanning it with a banking app.
- Track what is open, due soon, overdue and paid, and who paid it.
- Keep the original photo or PDF with each bill.

## How it runs

The app is one file, `index.html`, with no build step. It loads three libraries from public CDNs:

- [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) 1.4.4 draws the QR code
- [jsQR](https://github.com/cozmo/jsQR) 1.4.0 reads QR codes from images
- [PDF.js](https://mozilla.github.io/pdf.js/) 3.11.174 reads PDFs

### Inside Claude

The live version is a Claude artifact. There, the shared invoice list, file attachments, signed-in members and downloads come from Claude's artifact runtime (`window.claude.use("db" | "user" | "assets" | "downloads")`).

### Outside Claude

Opened directly in a browser or from GitHub Pages, `window.claude` doesn't exist. The app then starts in **preview mode** with three example bills. Scanning, the QR slip and validation all work, but nothing is saved and nothing is shared.

To use it with real data outside Claude, replace the `dbStore` and the asset upload calls in `index.html` with a backend such as Supabase or Firebase.

## QR-bill notes

- Generated codes follow the Swiss Payment Standards QR-bill format version 2.0 (`SPC` / `0200`), with structured addresses (type `S`) and error correction level M.
- The labels on the payment slip stay in English. QR-bills may only use German, French, Italian or English there.
- Swico S1 bill information is read to fill in the invoice number, invoice date and due date when present.
