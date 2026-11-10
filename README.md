# Table to CSV & Excel Web Extractor - RowCraft

[![Chrome Web Store](https://img.shields.io/chrome-web-store/v/ofhajfbdajpjidhcpbofkoeaegjjafeg?label=Chrome%20Web%20Store&logo=googlechrome&logoColor=white&color=10b981)](https://chromewebstore.google.com/detail/ofhajfbdajpjidhcpbofkoeaegjjafeg)
[![Manifest V3](https://img.shields.io/badge/Chrome-Manifest%20V3-10b981?logo=googlechrome&logoColor=white)](https://chromewebstore.google.com/detail/ofhajfbdajpjidhcpbofkoeaegjjafeg)
[![Privacy: 100% Local](https://img.shields.io/badge/Privacy-100%25%20Client--Side%20In--Memory-0ea5e9)](https://talofer.github.io/rowcraft/privacy.html)
[![License: MIT](https://img.shields.io/badge/License-MIT-f59e0b)](./LICENSE)

**RowCraft Extractor** turns any messy HTML `<table>`, open Shadow DOM table, or modern ARIA data grid (`[role="grid"]`, `[role="table"]`) into a clean **UTF-8 BOM CSV (`.csv`)**, native **Excel workbook (`.xlsx` — single or multi-sheet)**, or **Clipboard TSV/Markdown** in 1 click.

👉 **[Install RowCraft on the Chrome Web Store (100% Free)](https://chromewebstore.google.com/detail/ofhajfbdajpjidhcpbofkoeaegjjafeg)**  
🌐 **[Use the Free Online HTML Table to CSV Converter Utility](https://talofer.github.io/rowcraft/)**

---

## Why Standard Browser Copy-Paste Breaks Spreadsheets (And How RowCraft Solves It)

When you highlight an HTML table on a webpage and paste it into Microsoft Excel or Google Sheets:
1. **Multi-Line Cell Fracturing:** Nested `<br>`, `<div>`, and `<p>` tags inside a single `<td>` cell are interpreted as row breaks, splitting one logical record across 3–5 broken spreadsheet rows.
2. **UTF-8 Mojibake (`×©×œ×•× `):** Standard `.csv` downloads omit the UTF-8 Byte Order Mark (`\uFEFF`), causing Microsoft Excel on Windows to misread Hebrew, Arabic, CJK, and accented European characters.
3. **OWASP CSV Formula Injection (DDE):** Web tables containing cells starting with `=`, `+`, `-`, or `@` (such as `=CMD|' /C calc'!A0`) can execute arbitrary formulas when opened in spreadsheet software.
4. **32,767-Character Overflow:** Cells containing large JSON payloads or logs exceeding Microsoft Excel's hard `32,767`-character cell limit corrupt `.xlsx` and `.csv` imports.

### What RowCraft Does Automatically in Local Browser Memory
- **Flattens Internal Cell Line Breaks:** Collapses `<br>`, `<div>`, `<p>`, and `<li>` blocks inside each cell into clean single-line text so rows never fracture.
- **Prepends `\uFEFF` UTF-8 BOM:** Ensures Microsoft Excel, Google Sheets, and LibreOffice Calc render international characters (`שלום עולם`, `Café`, `日本語`) cleanly on first open.
- **OWASP DDE / Formula Injection Guard:** Prefixes dangerous formula prefixes (`=`, `+`, `-`, `@`, `\t`, `\r`) with a literal apostrophe (`'`) while preserving signed numbers/currency (`-$1,250.00`, `-42.50`) and dash placeholders (`--`).
- **1-Click Multi-Sheet `.XLSX` Workbooks:** Detects all tables on a webpage and exports them either individually or combined into a single multi-worksheet `.xlsx` file.
- **Zero-Backend `activeTab` Privacy:** No `<all_urls>` background host permissions, no account required, and zero table data ever leaves your browser.

---

## Technical Deep-Dive Articles

- 📄 [**Why Pasting Web Tables Destroys Excel Rows (And How RFC 4180 & DDE Sanitization Solve It)**](./docs/articles/multiline-table-breaks.md)
- 🔒 [**The Hidden Security Risks of Cloud Web-Scraping Extensions (`<all_urls>` vs `activeTab`)**](./docs/articles/client-side-vs-cloud-scrapers.md)

---

## Global Localization (`_locales/`)

RowCraft ships native Chrome Web Store localization bundles in:
- 🇺🇸 **English (`en`):** *Table to CSV & Excel: RowCraft Extractor*
- 🇩🇪 **German (`de`):** *Tabelle zu CSV & Excel: RowCraft Extraktor*
- 🇪🇸 **Spanish (`es`):** *Tabla a Excel y CSV: RowCraft Extractor*
- 🇫🇷 **French (`fr`):** *Tableau vers CSV & Excel : RowCraft Extracteur*
- 🇯🇵 **Japanese (`ja`):** *Web表をCSV・Excelに変換: RowCraft エクストラクター*

---

## Legal & Privacy

- **[Privacy Policy](https://talofer.github.io/rowcraft/privacy.html)** — 100% client-side in-memory table extraction.
- **[Terms of Service](https://talofer.github.io/rowcraft/terms.html)** — Provided "AS IS" with a strict $10.00 USD limitation of liability.
- **[License](./LICENSE)** — MIT License.
