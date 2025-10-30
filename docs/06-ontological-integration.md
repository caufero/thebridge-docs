# 6. Ontological Integration

## 6.1 Instance DNA Format

### Format
`PRXYYNNNN`

#### Where
- `PRX` → 3-letter **process code** (e.g., `TSK`, `RCH`, `TEH`, `PHO`, `OFC`, `BOM`, etc.)
- `YY` → **Year** (e.g., `25` for 2025)
- `NNNN` → **Sequential number** with anti-collision logic for multi-user environments.  
  Starts at 4 digits and automatically expands as needed.

#### Examples
- `PHO250001` → First phone call in 2025  
- `PHO259999` → 9999th phone call  
- `PHO2510000` → 10,000th phone call (auto-expands to 5 digits)  
- `PHO25100000` → 100,000th phone call (auto-expands to 6 digits)

---

### FileMaker Implementation

#### Simple
```filemaker
DNA_Generator = "PHO" & Right ( Year ; 2 ) & SerialIncrement ( PHO_Counter ; 4 )

// SerialIncrement automatically manages digit overflow
```
