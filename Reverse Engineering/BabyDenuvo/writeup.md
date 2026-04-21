# Reverse Engineering
## BabyDenuvo

![](assets/image%2021.png)

```
#!/usr/bin/env python3
import socket

HOST = "crypto.labs.nerdslab.in"
PORT = 1339

# From the VM:
#   checked = ((input_byte + 0x42) & 0xff) ^ 0x13
# so invert it here.
table = [222, 173, 190, 239, 127, 106, 55, 175, 94, 250, 83, 109, 22, 57, 118, 198]
payload = bytes(((x ^ 0x13) - 0x42) & 0xff for x in table)

print("payload:", payload.hex())

with socket.create_connection((HOST, PORT), timeout=10) as s:
    s.sendall(payload)
    s.shutdown(socket.SHUT_WR)

    data = bytearray()
    while True:
        chunk = s.recv(4096)
        if not chunk:
            break
        data += chunk

print(data.decode(errors="replace"))
```

```
defcon{vM_r3v3r51nG_sUr3_1s_fUn_41n't_1t}
```
