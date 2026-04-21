# Misc
## Romeo and Juliet

![](assets/image%2022.png)

I dont want solve it again mannualy its pure brainrot….for me fr 💔 so just visualize what i done using clade……

---

SPL maps Shakespearean constructs to programming concepts:

| SPL construct | Meaning |
| --- | --- |
| Character declared in the header | Variable (initialised to 0) |
| `Thou art the sum of X and Y.` | Variable = X + Y |
| `Thou art the difference between X and Y.` | Variable = X − Y |
| `Speak your mind!` | Print current value as ASCII character |
| `thyself` | The character's own current value |
| Adjectives before `king` | Encode a number (each adjective doubles it) |

Adjective number encoding

The number encoded by a phrase is `2^(count of adjectives)`:

| Phrase | Adjective count | Value |
| --- | --- | --- |
| `a king` | 0 | 1 |
| `a noble king` | 1 | 2 |
| `a fair noble king` | 2 | 4 |
| `a bold fair noble king` | 3 | 8 |
| `a brave bold fair noble king` | 4 | 16 |
| `a sweet brave bold fair noble king` | 5 | 32 |
| `a gentle sweet brave bold fair noble king` | 6 | 64 |

---

---

### How the Flag is Built

Romeo starts at 0. The very first instruction in Scene II sets him to 96:

```
DIFF: a warm gentle sweet brave bold fair noble king (128)
      minus a sweet brave bold fair noble king (32)
→ Romeo = 128 − 32 = 96
```

From there, each group of add/subtract lines nudges Romeo to a new value, then `Speak your mind!` converts that value to a character via ASCII.

### Full line-by-line trace

| Line | Operation | Change | Romeo | Char |
| --- | --- | --- | --- | --- |
| 19 | DIFF: warm…noble (128) − sweet…noble (32) | −32 | 96 |  |
| 20 | SUM: thyself + fair noble king (4) | +4 | 100 |  |
| **21** | **Speak your mind!** | ASCII 100 | 100 | **d** |
| 22 | SUM: thyself + king (1) | +1 | 101 |  |
| **23** | **Speak your mind!** | ASCII 101 | 101 | **e** |
| 24 | SUM: thyself + king (1) | +1 | 102 |  |
| **25** | **Speak your mind!** | ASCII 102 | 102 | **f** |
| 26 | DIFF: thyself − fair noble king (4) | −4 | 98 |  |
| 27 | SUM: thyself + king (1) | +1 | 99 |  |
| **28** | **Speak your mind!** | ASCII 99 | 99 | **c** |
| 29 | SUM: thyself + bold fair noble king (8) | +8 | 107 |  |
| 30 | SUM: thyself + fair noble king (4) | +4 | 111 |  |
| **31** | **Speak your mind!** | ASCII 111 | 111 | **o** |
| 32 | DIFF: thyself − king (1) | −1 | 110 |  |
| **33** | **Speak your mind!** | ASCII 110 | 110 | **n** |
| 34 | SUM: thyself + bold fair noble king (8) | +8 | 118 |  |
| 35 | SUM: thyself + fair noble king (4) | +4 | 122 |  |
| 36 | SUM: thyself + king (1) | +1 | 123 |  |
| **37** | **Speak your mind!** | ASCII 123 | 123 | **{** |
| 38 | DIFF: thyself − bold fair noble king (8) | −8 | 115 |  |
| 39 | DIFF: thyself − king (1) | −1 | 114 |  |
| **40** | **Speak your mind!** | ASCII 114 | 114 | **r** |
| 41 | DIFF: thyself − gentle…noble king (64) | −64 | 50 |  |
| 42 | SUM: thyself + king (1) | +1 | 51 |  |
| **43** | **Speak your mind!** | ASCII 51 | 51 | **3** |
| 44 | SUM: thyself + king (1) | +1 | 52 |  |
| **45** | **Speak your mind!** | ASCII 52 | 52 | **4** |
| 46 | SUM: thyself + sweet…noble king (32) | +32 | 84 |  |
| 47 | SUM: thyself + brave bold fair noble king (16) | +16 | 100 |  |
| **48** | **Speak your mind!** | ASCII 100 | 100 | **d** |
| 49 | DIFF: thyself − fair noble king (4) | −4 | 96 |  |
| 50 | DIFF: thyself − king (1) | −1 | 95 |  |
| **51** | **Speak your mind!** | ASCII 95 | 95 | **_** |
| 52 | DIFF: thyself − sweet…noble king (32) | −32 | 63 |  |
| 53 | DIFF: thyself − bold fair noble king (8) | −8 | 55 |  |
| **54** | **Speak your mind!** | ASCII 55 | 55 | **7** |
| 55 | SUM: thyself + sweet…noble king (32) | +32 | 87 |  |
| 56 | SUM: thyself + brave bold fair noble king (16) | +16 | 103 |  |
| 57 | SUM: thyself + king (1) | +1 | 104 |  |
| **58** | **Speak your mind!** | ASCII 104 | 104 | **h** |
| 59 | DIFF: thyself − sweet…noble king (32) | −32 | 72 |  |
| 60 | DIFF: thyself − brave bold fair noble king (16) | −16 | 56 |  |
| 61 | DIFF: thyself − fair noble king (4) | −4 | 52 |  |
| 62 | DIFF: thyself − king (1) | −1 | 51 |  |
| **63** | **Speak your mind!** | ASCII 51 | 51 | **3** |
| 64 | SUM: thyself + sweet…noble king (32) | +32 | 83 |  |
| 65 | SUM: thyself + bold fair noble king (8) | +8 | 91 |  |
| 66 | SUM: thyself + fair noble king (4) | +4 | 95 |  |
| **67** | **Speak your mind!** | ASCII 95 | 95 | **_** |
| 68 | DIFF: thyself − sweet…noble king (32) | −32 | 63 |  |
| 69 | DIFF: thyself − bold fair noble king (8) | −8 | 55 |  |
| 70 | DIFF: thyself − noble king (2) | −2 | 53 |  |
| **71** | **Speak your mind!** | ASCII 53 | 53 | **5** |
| 72 | SUM: thyself + noble king (2) | +2 | 55 |  |
| **73** | **Speak your mind!** | ASCII 55 | 55 | **7** |
| 74 | DIFF: thyself − noble king (2) | −2 | 53 |  |
| 75 | DIFF: thyself − king (1) | −1 | 52 |  |
| **76** | **Speak your mind!** | ASCII 52 | 52 | **4** |
| 77 | SUM: thyself + noble king (2) | +2 | 54 |  |
| **78** | **Speak your mind!** | ASCII 54 | 54 | **6** |
| 79 | DIFF: thyself − noble king (2) | −2 | 52 |  |
| 80 | DIFF: thyself − king (1) | −1 | 51 |  |
| **81** | **Speak your mind!** | ASCII 51 | 51 | **3** |
| 82 | SUM: thyself + sweet…noble king (32) | +32 | 83 |  |
| 83 | SUM: thyself + bold fair noble king (8) | +8 | 91 |  |
| 84 | SUM: thyself + fair noble king (4) | +4 | 95 |  |
| **85** | **Speak your mind!** | ASCII 95 | 95 | **_** |
| 86 | SUM: thyself + fair noble king (4) | +4 | 99 |  |
| 87 | SUM: thyself + king (1) | +1 | 100 |  |
| **88** | **Speak your mind!** | ASCII 100 | 100 | **d** |
| 89 | DIFF: thyself − sweet…noble king (32) | −32 | 68 |  |
| 90 | DIFF: thyself − brave bold fair noble king (16) | −16 | 52 |  |
| 91 | DIFF: thyself − noble king (2) | −2 | 50 |  |
| 92 | DIFF: thyself − king (1) | −1 | 49 |  |
| **93** | **Speak your mind!** | ASCII 49 | 49 | **1** |
| 94 | SUM: thyself + gentle…noble king (64) | +64 | 113 |  |
| 95 | SUM: thyself + king (1) | +1 | 114 |  |
| **96** | **Speak your mind!** | ASCII 114 | 114 | **r** |
| 97 | DIFF: thyself − gentle…noble king (64) | −64 | 50 |  |
| 98 | SUM: thyself + king (1) | +1 | 51 |  |
| **99** | **Speak your mind!** | ASCII 51 | 51 | **3** |
| 100 | SUM: thyself + sweet…noble king (32) | +32 | 83 |  |
| 101 | SUM: thyself + brave bold fair noble king (16) | +16 | 99 |  |
| **102** | **Speak your mind!** | ASCII 99 | 99 | **c** |
| 103 | DIFF: thyself − sweet…noble king (32) | −32 | 67 |  |
| 104 | DIFF: thyself − bold fair noble king (8) | −8 | 59 |  |
| 105 | DIFF: thyself − fair noble king (4) | −4 | 55 |  |
| **106** | **Speak your mind!** | ASCII 55 | 55 | **7** |
| 107 | DIFF: thyself − fair noble king (4) | −4 | 51 |  |
| 108 | DIFF: thyself − noble king (2) | −2 | 49 |  |
| **109** | **Speak your mind!** | ASCII 49 | 49 | **1** |
| 110 | DIFF: thyself − king (1) | −1 | 48 |  |
| **111** | **Speak your mind!** | ASCII 48 | 48 | **0** |
| 112 | SUM: thyself + gentle…noble king (64) | +64 | 112 |  |
| 113 | DIFF: thyself − noble king (2) | −2 | 110 |  |
| **114** | **Speak your mind!** | ASCII 110 | 110 | **n** |
| 115 | DIFF: thyself − sweet…noble king (32) | −32 | 78 |  |
| 116 | DIFF: thyself − brave bold fair noble king (16) | −16 | 62 |  |
| 117 | DIFF: thyself − bold fair noble king (8) | −8 | 54 |  |
| 118 | DIFF: thyself − king (1) | −1 | 53 |  |
| **119** | **Speak your mind!** | ASCII 53 | 53 | **5** |
| 120 | SUM: thyself + gentle…noble king (64) | +64 | 117 |  |
| 121 | SUM: thyself + bold fair noble king (8) | +8 | 125 |  |
| **122** | **Speak your mind!** | ASCII 125 | 125 | **}** |

---

**Flag:** `defcon{r34d_7h3_57463_d1r3c710n5}`
