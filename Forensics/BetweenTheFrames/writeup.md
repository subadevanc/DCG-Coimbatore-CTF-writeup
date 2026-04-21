# Forensics
## BetweenTheFrames

![](assets/image%2028.png)

this is mkv file….. same like challege I created in my college ctf so this easy for me…. check out [doom by doom](https://github.com/subadevanc/SPYIREX-CTF/blob/main/Steganography/Doom By Doom/solution.md)on my github

here the link: [https://github.com/subadevanc/SPYIREX-CTF/blob/main/Steganography/Doom By Doom/solution.md](https://github.com/subadevanc/SPYIREX-CTF/blob/main/Steganography/Doom By Doom/solution.md)

```
mkdir -p frames
ffmpeg -hide_banner -loglevel error -i secrets.mkv frames/frame_%04d.bmp
```

this make new folder and split the mkv file frame by frame as bmp file

Run `steghide` on every frame using an empty password and save that seprate folder

```
mkdir -p extracted
found=0
for f in frames/*.bmp; do
  b=$(basename "$f" .bmp)
  out="extracted/${b}.bin"
  if steghide extract -sf "$f" -xf "$out" -p "" -f >/dev/null 2>&1; then
    echo "$f -> $out"
    found=$((found+1))
  fi
done
echo "FOUND=$found"
```

this extract and save as bin file, 165 frames produced hidden chunks.

just do

```
cat extracted/*

If you can read this, your technique has proven successful!
My hearty congratulations on making it this far. I'm proud of you, young one.
Now, for the moment you've been waiting for. Here is your flag!
REVGQ09Oe1RIM19WMUQzMF9DSDRMTDNORzNfMVNfMFYzUn0K
Whoops, that wasn't supposed to happen. I guess it's been encoded. You shouldn't have too much trouble decoding this!
Pssst... think of a number below 100 that's both a perfect square and a perfect cube.
***************************************%         
```

```
REVGQ09Oe1RIM19WMUQzMF9DSDRMTDNORzNfMVNfMFYzUn0K
```

then just decode it

![](assets/image%2029.png)

```
DEFCON{TH3_V1D30_CH4LL3NG3_1S_0V3R}
```
