# POS-Pen-Testing-Bugz

A practical POS Pen Testing Cheat Sheet with test cases. Built from real-world project
experience with full commitment to Responsible Disclosure.

> The goal is to share the *techniques* and the *thought process* — not anyone's production
> secrets. Shared for educational and defensive purposes only;
---

## Ingenico MOVE/2500

The Ingenico MOVE/2500 was evaluated largely in the configuration used in production, reflecting
vendor and integrator defaults.

On the Ingenico MOVE/2500, entering download mode (by pressing the green button and `.` during boot)
exposes the device to the LLT maintenance tool over USB without any authentication prompt. Once in
this mode, LLT provides direct access to the internal POS file system, allowing an operator to
browse, download, and upload files.



---

## Reconnaissance: learning the device before attacking it

Before touching the terminal as an attacker, I spent time as a *user* — understanding exactly how
this device is meant to work. A black-box terminal gives up very little until you know its boot
sequence, its key combinations, its service modes, and the tooling the vendor uses to manage it. So
the first real win came from **researching the vendor and the device's own documentation.**

Publicly available setup and driver-installation guides for the Ingenico Move/2500 turned out to be
genuinely valuable. They documented things like:

<img width="1003" height="764" alt="image" src="https://github.com/user-attachments/assets/9cd5da12-6921-4b96-8dab-64b5746e43f8" />

- How to put the device into **download mode** (the boot + key-press sequence).
- Which **USB drivers** the host needs to talk to the terminal.
- How the vendor's **Local Loading Tool (LLT)** connects to and provisions the device.

In other words, the official "how to install and service this terminal" material doubled as a map of
the exact attack path used below — the same connection workflow a technician uses is the one an
attacker abuses. This is a recurring theme in hardware/embedded testing: **vendor documentation is
reconnaissance gold.**

---

## Unauthenticated filesystem access over USB (Critical)

### The flaw

The terminal can be put into a "download mode" (boot + a key combo) and then spoken to over USB with
the vendor's **Local Loading Tool (LLT)** — the same software legitimately used to provision and
service the device. The problem: in this configuration the tool would **read, modify, and upload
files with no authentication at all.**

### What that gave up

- **Read everything:** configuration files, log files, certificates, and assets.
- **Modify everything:** including the bank/branding logos rendered on the screen — a ready-made
  phishing primitive (swap the logo, hand the device to a victim).
- **Download everything** to the tester's machine for offline analysis.

In short: temporary physical access → full filesystem control → persistence and tampering.

### Steps to reproduce

1. Boot the terminal into download mode (power + key combo).

   <img width="292" height="578" alt="POS in download mode" src="https://github.com/user-attachments/assets/4adc7edb-e50d-4b5d-a1fd-a2fadeaaa099" />

2. Connect over USB and open the vendor loading tool.

   <img width="728" height="421" alt="LLT connected over USB" src="https://github.com/user-attachments/assets/7ac13361-ae1b-42d4-8a7e-c2314dbcf61e" />

3. Browse / pull / push files freely.

   <img width="407" height="521" alt="Direct access to POS files; download and upload enabled" src="https://github.com/user-attachments/assets/bd6d4f7c-ab43-42b3-9a51-f4d4794389ae" />

*Terminal information and stored files readable over USB.*


---

#
