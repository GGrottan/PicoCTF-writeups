### Name: What Lies Within
### File: buildings.png
### Description: There's something in the building. Can you retrieve the flag?
### Points: 150

We are given an image file `buildings.png`, and I always start these types of tasks with 
checking if there are any "in plain sight" string. Using the strings-command gave no results.
Checking the metadata with exiftool didn't show any promising results either. 

Displaying the image also works, so it is unlikely that we received a file with the wrong extension.
Since we are dealing with a `.png`, steghide won't be able to help, although we can try to use Foremost and zsteg
to see if there are any embedded info.

Foremost gave no results, however, using zsteg, the flag was displayed in plain sight 🚩:

```console
gagr@ubntu:~/Documents/pico/Forensics/what_lies_within$ zsteg -a buildings.png 
b1,r,lsb,xy         .. text: "^5>R5YZrG"
b1,rgb,lsb,xy       .. text: "picoCTF{h1d1ng_1n_th3_b1t5}"
b1,abgr,msb,xy      .. file: PGP Secret Sub-key -
b2,b,lsb,xy         .. text: "XuH}p#8Iy="
b3,abgr,msb,xy      .. text: "t@Wp-_tH_v\r"
b4,r,lsb,xy         .. text: "fdD\"\"\"\" "
b4,r,msb,xy         .. text: "%Q#gpSv0c05"
b4,g,lsb,xy         .. text: "fDfffDD\"\""
b4,g,msb,xy         .. text: "f\"fff\"\"DD"
b4,b,lsb,xy         .. text: "\"$BDDDDf"
b4,b,msb,xy         .. text: "wwBDDDfUU53w"
```
