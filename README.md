# Table to CSV & Excel Web Extractor - RowCraft

[![Chrome Web Store](https://img.shields.io/badge/Chrome_Web_Store-Available_Now-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white)](https://chromewebstore.google.com/detail/ofhajfbdajpjidhcpbofkoeaegjjafeg)
[![Manifest V3](https://img.shields.io/badge/Manifest-V3-blue?style=for-the-badge)](https://developer.chrome.com/docs/extensions/develop/migrate/what-is-mv3)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Zero Telemetry](https://img.shields.io/badge/Privacy-100%25_Client--Side-brightgreen?style=for-the-badge)](#privacy--security-first)

> **RowCraft** is a lightweight, zero-configuration Chrome extension designed to extract HTML tables, ARIA grids, and dynamically loaded web tables directly to clean CSV and Excel-compatible files. Built strictly on client-side Manifest V3 architecture with zero user accounts and zero external processing.

[**👉 Add to Chrome (Official Store Listing)**](https://chromewebstore.google.com/detail/ofhajfbdajpjidhcpbofkoeaegjjafeg)

---

## ⚡ Key Capabilities

* **1-Click Table Scraping:** Instantly detects all `<table>` elements and ARIA data grids on the active browser tab.
* **Flawless Excel Compatibility (UTF-8 BOM):** Pre-encodes CSV outputs with `\uFEFF` so international character sets, Hebrew/Arabic glyphs, Asian scripts, and accents open correctly in Microsoft Excel without character corruption.
* **Cell Sanitization:** Normalizes internal line breaks (`<br>`, `<div>`, `<p>`) and unescaped quotes into clean, uniform cells to prevent fractured spreadsheet rows.
* **Zero-Account Architecture:** No email sign-up, no OAuth login, and no backend servers.
* **Large Table Handling:** Free export support for datasets up to 500 rows with immediate unblocked downloads.

---

## 🔒 Privacy & Security First

Unlike cloud-based scrapers that route page data through remote proxies or cloud databases, RowCraft operates entirely in local browser memory:

* **Minimal Permissions:** Requests only `activeTab` (to access the inspected table on click) and `storage` (for local preferences).
* **Zero Remote Processing:** Table parsing and file generation happen in-memory inside your local Chrome sandbox.
* **Safe for Enterprise:** Complies with corporate IT standards—no tracking cookies, no DOM logging, and zero PII collection.

---

## 🚀 Installation & Usage

1. Install RowCraft from the [Chrome Web Store](https://chromewebstore.google.com/detail/ofhajfbdajpjidhcpbofkoeaegjjafeg).
2. Navigate to any webpage containing a table or data grid.
3. Click the **RowCraft** extension icon in your Chrome toolbar.
4. Preview the detected rows and select **Export CSV**.

---

## 🛠️ Supported Formats & Common Use Cases

* **HTML Data Tables:** Standard `<table>`, `<thead>`, `<tbody>`, `<tr>`, `<td>` structures.
* **Dynamic SPAs:** Tables rendered client-side via React, Angular, Vue, and ag-Grid.
* **Enterprise Portals:** Intranet dashboards, Salesforce tables, Wikipedia lists, and Jira ticket matrices.
* **Spreadsheet Platforms:** Fully compatible with Microsoft Excel, Google Sheets, Apple Numbers, and LibreOffice Calc.

---

## 📄 Legal & Compliance

* [Privacy Policy](https://github.com/talofer/rowcraft/blob/main/docs/privacy.html)
* [Terms of Service](https://github.com/talofer/rowcraft/blob/main/docs/terms.html)
* Licensed under the [MIT License](LICENSE).
