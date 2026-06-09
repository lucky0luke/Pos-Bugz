
# POS Pen Testing Bugz 
 
> A practical POS penetration testing cheat sheet with real-world test cases, built from actual
> project experience — shared under the principles of **Responsible Disclosure**.
>
> The goal is to share the *techniques* and the *thought process*, not production secrets.
> Everything here is for **educational and defensive purposes only.**
 
---
 
## Vendors & Findings Index
 
| # | Vendor | Device | Vulnerability | Severity | Write-up |
|---|--------|--------|---------------|----------|----------|
| 1 | Ingenico | MOVE/2500 | Unauthenticated Filesystem Access over USB (LLT / Download Mode) | 🔴 Critical | [→ Read](vendors/Ingenico-MOVE2500/unauthenticated-usb-filesystem-access.md) |
 
> More vendors and findings will be added as research is documented.
 
---
 
## Repo Structure
 
```
POS-Pen-Testing-Bugz/
├── README.md                          ← This file — global index
├── Resources
└── vendors/
    └── Ingenico-MOVE2500/
        └── unauthenticated-usb-filesystem-access.md
```
 
Each vendor gets its own folder. Each vulnerability gets its own Markdown file inside it.
 
