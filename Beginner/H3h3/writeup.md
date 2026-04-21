# Beginner
## H3h3

![](assets/image%206.png)

![](assets/image%207.png)

File is not opening and header is missing. just a add header and open the file

```
A normal JFIF JPEG starts with:

`FF D8 FF E0 00 10 4A 46 49 46 00 01`
```

```
cp 'h3h3 (1).jpg' repaired.jpg
printf '\xFF\xD8\xFF\xE0\x00\x10\x4A\x46\x49\x46\x00\x01' \
 | dd of=repaired.jpg bs=1 seek=0 conv=notrunc status=none
```

then the repaired.jpg….here we get the flag

![](assets/h3h3.jpg)
