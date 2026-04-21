# Crypto
## Artifact

![](assets/image%2011.png)

this were i got idea they tell all order how i constructed

![](assets/image%2012.png)

same forat as they said then just write code to split and xored the chiper got the flag

```
from pathlib import Path

b = Path("xor_artifact.bin").read_bytes()

ct = b[0x10c:0x10c+41]      # ciphertext
key = b[0x5a:0x5a+41]       # schedule slice (correct alignment)

pt = bytes(c ^ k for c, k in zip(ct, key))
print(pt.decode())
```

```
defcon{x0r_c4sc4d3_k3y_sch3dul3_r3v3rs3d}
```
