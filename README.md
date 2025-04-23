# Logseq XSS to RCE PoC Exploit
---

During security testing of the **Logseq** (Desktop/Android) application, version **0.10.9**, a critical-severity **DOM-based Cross-Site Scripting (XSS)** vulnerability was identified in the `marketplace.html` endpoint. An attacker can host a malicious Logseq plugin on GitHub with JavaScript embedded in the plugin’s `README.md`. When this README is rendered inside the Logseq plugin marketplace, unsanitized input from the document location is directly injected into `innerHTML` which results in arbitrary JavaScript execution. Furthermore, the absence of an allowlist for `shell.openExternal` (exposed via `window.cljs`) allows this DOM-based XSS to escalate to **Remote Code Execution (RCE)** by abusing system-level protocol handlers.

---
## Proof of Concept (PoC) Exploit

### DOM-based Cross-Site Scripting (XSS) in the marketplace for plugins

<img src=x onerror="alert('XSS')">

### DOM-based XSS leading to RCE via system-level protocol handlers

<img src=x onerror="window.location='ms-calculator://'">

*This PoC demonstrates on a benign example the critical impact of unsanitized user input combined with insufficient protocol filtering.*

---
## Recommended Mitigations

To address the DOM-based XSS and RCE risks stemming from the combination of unsanitized HTML rendering and protocol misuse, the following mitigations are recommended:

1. **Sanitize rendered plugin README content** in `marketplace.html`: Input from the `repo` query parameter is fetched from GitHub and injected directly into the DOM via `innerHTML` after being parsed by `marked.parse()`. This content should be sanitized before being inserted.
2. **Implement protocol allowlisting** in `window.cljs`: Introduce a strict allowlist of supported protocols and explicitly block others, especially system-level handlers unless explicitly needed for functionality.
