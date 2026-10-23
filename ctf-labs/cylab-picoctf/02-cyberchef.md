# Section 2 — CyberChef

[← Back to Learning Log](../../README.md)

**Platform:** CyLab Security Academy / picoCTF  
**Module:** Beginner's Guide to the Challenge Library — Section 2  
**Date:** June 2026  
**Skills demonstrated:** ROT13, hex/decimal/binary conversion, Base64 decoding, CyberChef

#### Overview

This section introduces encoding and number systems — the foundation of almost all cryptography challenges. CyberChef is the primary tool: a free browser-based encoder/decoder built by GCHQ. Drag an operation into the Recipe, paste your input, and it transforms it instantly.

**CyberChef:** https://gchq.github.io/CyberChef/

#### Key Concepts

**Challenge 1 — Mod 26 (ROT13):** ROT13 replaces every letter with the one 13 positions away in the alphabet. Applying it twice returns the original — it's its own reverse. Not real encryption, just obfuscation.

```bash
# Terminal method
echo "cvpbPGS{...}" | tr A-Za-z N-ZA-Mn-za-m
```

**Challenge 2 — Warmed Up (Hex → Decimal):** The prefix `0x` signals a hexadecimal number. Converting to decimal:

```
0x3D = (3 × 16) + 13 = 61
```
```bash
echo $((0x3D))    # → 61
```

**Challenge 3 — 2Warm (Decimal → Binary):** Divide by 2 repeatedly, read remainders bottom-up.

```
42 → 101010 (binary)
```
```bash
echo "obase=2; 42" | bc    # → 101010
```

**Challenge 4 — Bases (Base64):** Base64 encodes binary data as readable text. It's used in JWTs, email attachments, and data URLs. It's not encryption — anyone can decode it instantly. Recognise it by the trailing `=` or `==` padding.

```bash
echo "bDNhcm5fdGgzX3IwcDM1" | base64 -d
```

| Base | Name | Prefix | Example |
|---|---|---|---|
| 2 | Binary | none / `0b` | `101010` = 42 |
| 10 | Decimal | none | `42` |
| 16 | Hexadecimal | `0x` | `0x2A` = 42 |

| Encoding | Recognise it by | Decode with |
|---|---|---|
| ROT13 | Scrambled letters | CyberChef ROT13 or `tr` |
| Base64 | Alphanumeric + `+/=`, no spaces | CyberChef or `base64 -d` |
| Hex | `0x` prefix or pairs like `48 65 6c` | CyberChef "From Hex" |

#### Takeaways

- Number system conversions (hex, binary, decimal) appear constantly in CTFs and real security work — memory addresses, file signatures, and colour codes all use hex.
- Base64 is not encryption. Seeing it in a web token or URL parameter is worth investigating.
- CyberChef's "Magic" operation can auto-detect the encoding — useful when you're not sure what you're looking at.
