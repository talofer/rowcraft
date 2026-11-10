# The Hidden Security Risks of Cloud Web-Scraping Extensions (`<all_urls>` vs `activeTab`)

When installing a browser extension to export HTML tables to Excel or CSV, most users click "Add to Chrome" without inspecting the permissions dialog:

> *"Read and change all your data on all websites"*

That single line corresponds to `"host_permissions": ["<all_urls>"]` (or `*://*/*`) in Chrome's extension manifest—and in enterprise environments, financial dashboards, CRM portals, and internal admin tools, it represents a massive data-exfiltration surface.

---

## 1. `<all_urls>` vs. `activeTab`: What Actually Happens in Chrome

### The `<all_urls>` Cloud Scraper Pattern
Extensions that request `<all_urls>` inject background content scripts into **every single page you visit**—including your bank, corporate ERP, healthcare portal, and private Google Docs—whether or not you ever click the extension icon. Worse, many "cloud scraper" extensions transmit DOM snapshots to external backend servers for parsing or AI formatting.

### The Strict `activeTab` + Client-Side In-Memory Pattern (RowCraft)
RowCraft is architected on a zero-trust, zero-backend Manifest V3 foundation:
1. **Zero `<all_urls>` Host Permissions:** RowCraft cannot read any tab in the background. Chrome grants temporary access to the active tab **only** at the exact moment you explicitly click the RowCraft toolbar icon (or press `Alt+Shift+E`).
2. **100% Client-Side In-Memory Parsing:** Both RFC 4180 CSV serialization and binary `.xlsx` generation (via SheetJS) execute entirely inside your local browser memory using `Blob` and `URL.createObjectURL()`. Zero table cells, URLs, or headers are ever transmitted to any server.
3. **Strict Content Security Policy (CSP):**
   ```json
   "content_security_policy": {
     "extension_pages": "script-src 'self'; object-src 'self';"
   }
   ```
4. **Zero `innerHTML` DOM Rendering:** When previewing scraped table headers and cells inside the extension popup, RowCraft renders every string strictly via `document.createTextNode()` and `element.textContent`. Even if a scraped webpage contains an XSS payload like `<img src=x onerror=alert(1)>` inside a `<td>` cell, it is rendered as inert plain text.

---

## 2. Security Comparison Matrix

| Security Dimension | Typical Cloud Scraper Extension | RowCraft Extractor (`v1.2.0`) |
| :--- | :--- | :--- |
| **Host Permissions** | `<all_urls>` (runs on every page 24/7) | `activeTab` only (runs only when clicked) |
| **Where Table Data Is Parsed** | External cloud API / remote server | 100% local browser memory (`Blob`) |
| **DOM XSS Protection** | Often uses `innerHTML` for previews | Strict `textContent` / `createTextNode` |
| **CSV Formula / DDE Guard** | None (raw `=CMD\|...` exported) | OWASP DDE neutralization (`'=`) |
| **Account / Sign-Up Required** | Requires email or OAuth login | Zero sign-up required |

---

## Verify It Yourself
- **Install on Chrome Web Store:** [Table to CSV & Excel: RowCraft Extractor](https://chromewebstore.google.com/detail/ofhajfbdajpjidhcpbofkoeaegjjafeg)
- **Inspect the Public Documentation & Online Converter:** [https://talofer.github.io/rowcraft/](https://talofer.github.io/rowcraft/)
