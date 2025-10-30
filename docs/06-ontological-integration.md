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

#### Extended
```filemaker
# Generate unique DNA code for PHO
Set Variable [$year; Value: Right(Year(Get(CurrentDate)); 2)]
Set Variable [$type; Value: "PHO"]

# Get next sequence with collision prevention
Perform Script ["Get_Next_Sequence"; Parameter: "PHO"]
Set Variable [$sequence; Value: Get(ScriptResult)]

# Format with automatic overflow handling
If [$sequence < 10000]
    Set Variable [$dna; Value: $type & $year & Right("0000" & $sequence; 4)]
Else If [$sequence < 100000]
    Set Variable [$dna; Value: $type & $year & $sequence]
End If

Exit Script [Result: $dna]
