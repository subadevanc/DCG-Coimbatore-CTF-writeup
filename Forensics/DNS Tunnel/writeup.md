# Forensics
## DNS Tunnel

![](assets/image%2024.png)

classic network challenge

strings reveals the key and repeted exfil this looks sus

![](assets/image%2025.png)

open wireshark:

```
dns && dns.flags.response == 0 && dns.qry.name contains "exfil"
```

9 Fragments are there collect all those stuffs and join.

![](assets/image%2026.png)

```
HY7TYOJVGQQT4NDPAUXC6NBUH5VQKKJPHA7GUN3ONM2AKPZCHRVTMBJ6NE4WUPTJHYTQ
```

intial its is a base32 then xor(0x5a) and i sucks there in packet chiper were in samll case but base32 wont decode it propely then i try with capital case the i got flag

base32 → XOR(5a HEX)

![](assets/image%2027.png)

```
defcon{dn5_tunne1_subd0m41n_exf1l_d3c0d3d}
```
