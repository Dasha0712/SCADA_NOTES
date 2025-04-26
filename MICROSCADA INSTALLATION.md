# MicroSCADA Installation Guide

> Step-by-step installation procedure for MicroSCADA SYS600 on site laptop.

---

## Before You Start

> ⚠️ **Run ALL `.exe` files in Administrator mode** — this applies to every step below without exception.

---

## Installation Steps

### Step 1 — Install Broker First

- From the installation package, **install "Broker" first** before anything else
- Order matters — Broker must be installed before SYS600

---

### Step 2 — Restart Laptop

- After Broker installation, **restart your laptop**

---

### Step 3 — Check Data Connection

- After restart, make sure your **data/network connection is active**
- Installation requires connectivity — verify before proceeding

---

### Step 4 — Install SYS600

- Now install **SYS600** from the package
- This will take time — **be patient and let it complete fully**

---

### Step 5 — Enter Password When Prompted

- During installation, it will ask for a password

| Version | Password |
|---------|----------|
| 10.7 | `Micro10SCADA` |

> ⚠️ Password is case-sensitive — enter it exactly as shown.

---

### Step 6 — Restart Twice

- After installation completes, **restart the laptop twice**
- Do not skip the second restart

---

### Step 7 — Change System Date to 2025 Before Adding License

- Before opening the license tool, **change your system date to year 2025**
- The license will not activate correctly if the date is wrong

> ⚠️ This step is easy to forget — always change the date before adding the license.

---

### Step 8 — Add License via SYS Control Panel

1. Open **SYS Control Panel**
2. Click the **Expand button**
3. Add the **license file** provided in the installation package

---

## Step Summary

| Step | Action | Note |
|------|--------|------|
| 0 | Run all `.exe` as Administrator | Applies to every step |
| 1 | Install Broker | Must be first |
| 2 | Restart laptop | After Broker |
| 3 | Check data connection | Must be active |
| 4 | Install SYS600 | Takes time — wait fully |
| 5 | Enter password | `Micro10SCADA` (v10.7) |
| 6 | Restart twice | Both restarts required |
| 7 | Change system date to 2025 | Before adding license |
| 8 | Add license in SYS Control Panel | Expand → Add license |

---

*Last updated: April 2026 | MicroSCADA Installation Notes*