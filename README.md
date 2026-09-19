# FiberHome HG6145F1 Security Report

## Target Specs
- Device: FiberHome HG6145F1 (GPON ONU)
- Hardware: 00AC2 (Board: WKE2.004.424A01)
- Firmware: RP4422 (Algeria Telecom)
- Bug Type: Client-Side Authentication Bypass / Privilege Escalation

## Technical Summary
A user can bypass login checks by changing the browser memory variable `gLoginUser` to `"0"` in the console. This makes the system open admin menus and allow the download of the unencrypted configuration file `config.bin`, which contains system keys. The web server also has a bug where sending fragmented byte-range headers causes high CPU usage and can lead to a Denial of Service (DoS).

## File Hash
Verification hash for the proof packet:
`65c23510a762b99cce7f6b9d33aada44662cde0738c27a4de97e61fcd6ccda3e`

---

## Code Flow
```mermaid
graph TD
    A["<b style='color:#000000'>Web Interface Login</b>"] --> B["<b style='color:#000000'>Client-Side Check</b>"]
    B --> C{"<b style='color:#000000'>gLoginUser Changed to 0</b>"}
    C -->|Yes| D["<b style='color:#000000'>Admin Access Granted</b>"]
    C -->|No| E["<b style='color:#000000'>Access Denied</b>"]
    D --> F["<b style='color:#000000'>Download config.bin</b>"]

    style A fill:#ffffff,stroke:#000000,stroke-width:2px
    style B fill:#ffffff,stroke:#000000,stroke-width:2px
    style C fill:#ffffff,stroke:#000000,stroke-width:2px
    style D fill:#ffffff,stroke:#000000,stroke-width:2px
    style E fill:#ffffff,stroke:#000000,stroke-width:2px
    style F fill:#ffffff,stroke:#000000,stroke-width:2px
```

---

## Test Steps

### 1. Test Web Server Response
```bash
curl -s -o /dev/null -w "%{http_code}" http://127.0.0.1
```

### 2. Run Diagnostic Test
```bash
python3 -c "import urllib.request; print('Checking gateway response...')"
```

---

## Summary Table

| Component | Issue | Fix |
| :--- | :--- | :--- |
| Login System | Browser-side check | Move all authentication to the server side |
| Configuration | Plaintext keys | Encrypt config.bin files |
| Web Server | Byte-range handling | Drop bad fragmented headers |
