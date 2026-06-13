# 0-day Vulnerability Report: FiberHome HG6145F1 Privilege Escalation

## Target Baseline
- **Vendor:** FiberHome Telecommunication Technologies
- **Model:** HG6145F1 (GPON ONU)
- **Hardware Revision:** 00AC2 (Board: WKE2.004.424A01)
- **Firmware Build:** RP4422 (Algeria Telecom Variant)
- **Vulnerability Type:** Client-Side Logic Bypass leading to Local Privilege Escalation & Configuration Leak
- **Severity Score (Estimated CVSS):** 8.1 (High)

## Technical Summary
An authenticated local user can manipulate front-end dynamic validation parameters (specifically the global state `gLoginUser = "0"` inside the browser memory context via live console tampering), entirely bypassing client-side authentication filters located within `main.js` and `auth.js`. 

This exploitation chain forces the backend gateway to expose administrative sub-modules and leak the unencrypted master configuration bundle `config.bin` containing raw hardcoded cryptographic keys, granting full root-level administrative access. Additionally, the integrated web server responds to fragmented byte-range headers, leaving the core CPU susceptible to localized resource saturation and Denial of Service (DoS).

## Cryptographic Proof & Integrity Lock
To secure intellectual property and prevent retroactive modifications, the absolute sequential forensic bundle (containing step-by-step verification screenshots and text logs) has been cryptographically signed and anchored permanently to decentralized ledgers on **June 8, 2026**.

- **Evidence Package SHA-256 Hash:** 
  `65c23510a762b99cce7f6b9d33aada44662cde0738c27a4de97e61fcd6ccda3e`
- **Blockchain Verification Registry:** OpenTimestamps receipt issued and anchored to the Bitcoin Blockchain ledger.

*Note: This security research was conducted strictly pro bono, for altruistic defense purposes, and independent of monetary incentives.*
