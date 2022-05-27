## Name: Sleuthkit Intro
#### Points: 100
#### Description: Download the disk image and use mmls on it to find the size of the Linux partition. Connect to the remote checker service to check your answer and get the flag.
#### Files: `disk.img.gz`

We receive a disk image, as well as a netcat connection to verify our findings.

The goal of this challenge is pretty simple, where we only need to find the size of the partition using sleuthkit. 
First off, since we receive a `.gz`-file, we extract the contents:

```console 
┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/sleuthkit-intro]
└─$ gunzip disk.img.gz
```

Next, it might be interesting to see what partitions are present on the disk:

```console
┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/sleuthkit-intro]
└─$ mmls disk.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000204799   0000202752   Linux (0x83)

```

We can see that only one partition is Linux, and we can see the start, end, and length of sectors.

The description asks for the size of the sector, which we can find by adding the `-B` argument to display the partition sizes in bytes:

```console

┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/sleuthkit-intro]
└─$ mmls -B disk.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Size    Description
000:  Meta      0000000000   0000000000   0000000001   0512B   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   1024K   Unallocated
002:  000:000   0000002048   0000204799   0000202752   0099M   Linux (0x83)

```

Connecting to the netcat connection, however, we are not prompted to give the partition size, but the length of the sector instead.
We already got the length in our first output, and so entering this information gives us the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```
    ┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/sleuthkit-intro]
    └─$ nc saturn.picoctf.net 52279
    What is the size of the Linux partition in the given disk image?
    Length in sectors: 202752
    202752
    Great work!
    picoCTF{mm15_f7w!}
  ```

</details>
