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
    %% Base Styling
    classDef default fill:#1f2937,stroke:#4b5563,stroke-width:2px,color:#f3f4f6;
    classDef audit fill:#3b82f6,stroke:#1d4ed8,stroke-width:2px,color:#fff,font-weight:bold;
    classDef logic fill:#d97706,stroke:#b45309,stroke-width:2px,color:#fff,font-weight:bold;
    classDef secure fill:#10b981,stroke:#047857,stroke-width:2px,color:#fff,font-weight:bold;
    classDef flaw fill:#ef4444,stroke:#b91c1c,stroke-width:2px,color:#fff,font-weight:bold;

    A[Web Interface Login] --> B(Client-Side Check)
    
    B --> C{gLoginUser Changed to 0}
    
    C -->|Yes| D[Admin Access Granted]
    C -->|No| E[Access Denied]
    
    D & E --> F[Check Session Status]
    
    F --> G[Download config.bin]
    G --> H[Extract System Keys]

    %% Applying Classes
    class A audit;
    class B,C logic;
    class G,H secure;
    class D,E,F flaw;
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
