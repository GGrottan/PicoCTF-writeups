### Name: So Meta
### File: pico_img.png
### Description: Find the flag in this picture.
### Points: 150

Given an image file `pico_img.png`, I always try to start simple by checking whether the file can
be displayed, and checking whether any suspicious strings are present in the file.

The image displays properly, and the strings-command revealed something interesting:

```console
gagr@ubntu:~/Documents/pico/Forensics/so_meta$ strings pico_img.png
...
picoCTF{s0_m3ta_fec06741}
...
```

Although the task was solved, I believe that the intention of this tasks was to hide the flag
in the metadata of the file (given the task name "meta", and giving "metadata" as a hint for the task).
Using `exiftool`, the "Artist" name of the file seem to look like something useful 🚩:

```console
gagr@ubntu:~/Documents/pico/Forensics/so_meta$ exiftool pico_img.png 
ExifTool Version Number         : 12.16
File Name                       : pico_img.png
Directory                       : .
File Size                       : 106 KiB
File Modification Date/Time     : 2021:09:07 21:20:08+02:00
File Access Date/Time           : 2021:09:07 21:21:04+02:00
File Inode Change Date/Time     : 2021:09:07 21:20:31+02:00
File Permissions                : rw-rw-r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 600
Image Height                    : 600
Bit Depth                       : 8
Color Type                      : RGB
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Software                        : Adobe ImageReady
XMP Toolkit                     : Adobe XMP Core 5.3-c011 66.145661, 2012/02/06-14:56:27
Creator Tool                    : Adobe Photoshop CS6 (Windows)
Instance ID                     : xmp.iid:A5566E73B2B811E8BC7F9A4303DF1F9B
Document ID                     : xmp.did:A5566E74B2B811E8BC7F9A4303DF1F9B
Derived From Instance ID        : xmp.iid:A5566E71B2B811E8BC7F9A4303DF1F9B
Derived From Document ID        : xmp.did:A5566E72B2B811E8BC7F9A4303DF1F9B
Warning                         : [minor] Text chunk(s) found after PNG IDAT (may be ignored by some readers)
Artist                          : picoCTF{s0_m3ta_fec06741}
Image Size                      : 600x600
Megapixels                      : 0.360
```
