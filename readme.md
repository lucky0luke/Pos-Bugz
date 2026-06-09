
# POS Pen Testing Bugz 
 
> A practical POS penetration testing cheat sheet with real-world test cases, built from actual
> project experience  shared under the principles of **Responsible Disclosure**.
>
> The goal is to share the *techniques* and the *thought process*, not production secrets.
> Everything here is for **educational and defensive purposes only.**
 
---
 
## Vendors & Findings Index

| # | Vendor       | Device          | Vulnerability                                      | Write-up | Author |
|---|--------------|-----------------|----------------------------------------------------|----------|----------|
| 1 | Ingenico     | MOVE/2500       | Unauthenticated Filesystem Access over USB (LLT / Download Mode) | [→ Read](https://github.com/lucky0luke/Pos-Bugz/blob/main/Vendor/Ingenico-MOVE2500/unauthenticated-usb-filesystem-access.md) | Muhammad Matter  |
| 2 | Android POS  | Android         | Intercept Traffic Via SIM                          | [→ Read](https://github.com/lucky0luke/Pos-Bugz/blob/main/Vendor/Android-Pos/traffic-via-sim.md) | Muhammad Matter  |
| 3 |  PAX   | D230 device         | Intercepting Everything: A PAX PoS Pentest Case Study                      | [→ Read](https://medium.com/@omarelshopky/intercepting-everything-a-pax-pos-pentest-case-study-442f09cd9c8d) |  Omar Elshopky 
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
 
