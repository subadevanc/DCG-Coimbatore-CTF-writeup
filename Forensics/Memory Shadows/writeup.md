# Forensics
## Memory Shadows

![](assets/image%2030.png)

![](assets/image%2031.png)

I got like this on event but real solution is

```
python3 parse_memdump.py memdump.bin --list

python3 parse_memdump.py memdump.bin --list

PID      PPID     Name                 HeapSize
------------------------------------------------------------
4        0        System               138
348      4        smss.exe             238
512      348      csrss.exe            180
620      348      winlogon.exe         146
728      620      services.exe         128
736      620      lsass.exe            105
1024     728      svchost.exe          221
1288     728      svchost.exe          224
1920     728      explorer.exe         335
2048     1920     chrome.exe           442
3331     1920     gh0st_srv.exe        330
3332     3331     conhost.exe          113
```

exctly gh0st_srv.exe  is looks sus then

```
python3 parse_memdump.py memdump.bin --dump-pid 3331

=== Heap dump for PID 3331 ===
Connection established.
Exfiltration module loaded.
Target: NERDSLAB_CTF_VAULT
Status: ACTIVE
SECRET_PAYLOAD: defcon{m3m0ry_f0r3ns1cs_pr0c3ss_dump}
C2: 185.220.101.47:443
Encrypted channel: AES-256-GCM
```

got flag:

```
defcon{m3m0ry_f0r3ns1cs_pr0c3ss_dump}
```
