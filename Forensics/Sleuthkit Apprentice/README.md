## Name: Sleuthkit Apprentice
#### Points: 200
#### Description: Download this disk image and find the flag.
#### Files: `disk.flag.img.gz`

## 

Downloading and extracting the zip file gives us the disk image to analyze: `disk.flag.img`.

Using `mmls`, we can inspecting the partitions on the disk:

```console
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/sleuthkit-apprentice]
└─$ mmls disk.flag.img
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000206847   0000204800   Linux (0x83)
003:  000:001   0000206848   0000360447   0000153600   Linux Swap / Solaris x86 (0x82)
004:  000:002   0000360448   0000614399   0000253952   Linux (0x83)
```
There are three partitions available, all of them Linux. In the `Start` column, we can see where each of them start, and we need to specify the start-offset
when we want to inspect a certain partition. Starting with the first partition that has an offset of `2048`:

```console
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/sleuthkit-apprentice]
└─$ fls -o 2048 disk.flag.img
d/d 11: lost+found
r/r 12: ldlinux.sys
r/r 13: ldlinux.c32
r/r 15: config-virt
r/r 16: vmlinuz-virt
r/r 17: initramfs-virt
l/l 18: boot
r/r 20: libutil.c32
r/r 19: extlinux.conf
r/r 21: libcom32.c32
r/r 22: mboot.c32
r/r 23: menu.c32
r/r 14: System.map-virt
r/r 24: vesamenu.c32
V/V 25585:      $OrphanFiles
```

These files looks like configuration files, but might not give us that much information at this point. Inspecting the other partitions first might give us a
better starting point. 

One of the partitions are described as a `Linux Swap / Solaris x86` partition. This type of partition is used as an extra RAM storage, where if the RAM memory
runs out, the overflow will be stored in this partition instead. This might contain useful information, however, `fls` wouldn't work with this partition,
and so I moved on to the next one in case we could get some more information.

Inspecting the last partition with fls reveals the following:

```console
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/sleuthkit-apprentice]
└─$ fls -o 360448 disk.flag.img                                                                                     
d/d 451:        home
d/d 11: lost+found
d/d 12: boot
d/d 1985:       etc
d/d 1986:       proc
d/d 1987:       dev
d/d 1988:       tmp
d/d 1989:       lib
d/d 1990:       var
d/d 3969:       usr
d/d 3970:       bin
d/d 1991:       sbin
d/d 1992:       media
d/d 1993:       mnt
d/d 1994:       opt
d/d 1995:       root
d/d 1996:       run
d/d 1997:       srv
d/d 1998:       sys
d/d 2358:       swap
V/V 31745:      $OrphanFiles
```
This is good, we now have access to user files and directories!
We can start investigating whatever directory we want, however, the `root` directory seems like a good place to start, considering that this is a CTF-challenge.

Inspecting the contents of the `root` folder: 

```console
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/sleuthkit-apprentice]
└─$ fls -o 360448 disk.flag.img 1995
r/r 2363:       .ash_history
d/d 3981:       my_folder

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/sleuthkit-apprentice]
└─$ fls -o 360448 disk.flag.img 3981
r/r * 2082(realloc):    flag.txt
r/r 2371:       flag.uni.txt
````

🤔 `flag.uni.txt` seems like a file that we would be interested in.

Inspecting the contents of the file reveals the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```

    ┌──(gagr㉿desktop)-[/picoCTF2022/forensics/sleuthkit-apprentice]
    └─$ icat -o 360448 disk.flag.img 2371
    picoCTF{by73_5urf3r_42028120}

  ```

</details>
