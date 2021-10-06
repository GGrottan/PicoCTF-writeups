### Name: Pitter, Patter, Platters
### File: suspicious.dd.sda1
### Description: 'Suspicious' is written all over this disk image. Download suspicious.dd.sda1
### Points: 200

The given file for this task is a disk image, and so I began with analysing the image with Sleuthkit. 
Using `img_stat` and `fsstat` tells us that the file system type is Ext3, meaning Linux. Not particularly useful information at this point, 
but worth taking note of.

I proceeded to use `fls` to list the root directory of the image, and an interesting file appeared: 

```console
> pico\pitter_patter>fls suspicious.dd.sda1
d/d 11: lost+found
d/d 2009:       boot
d/d 4017:       tce
r/r 12: suspicious-file.txt
V/V 8033:       $OrphanFiles
```

Using the `tsk_recover` command, I extracted the entire directory in order to view the contents of the text file.

```console
> pico\pitter_patter>tsk_recover -e suspicious.dd.sda1 .
Files Recovered: 54
```
Inspecting the recovered `suspicious-file.txt` file:

```console
┌──(user㉿kali)-[/pico/pitter_patter]
└─$ cat suspicious-file.txt
Nothing to see here! But you may want to look here -->
```
Maybe there is some hidden information in the file? 🤔 I tried checking the strings and inspecting with hexdump, but found nothing interesting.

One of the hints given for this task mentions something called "slack space". 
According to [Computer Hope](https://www.computerhope.com/jargon/s/slack-space.htm), slack space is the remaining space in a cluster in which a file is stored - 
meaning that the text file is probably not taking up the entire cluster, and the flag is hidden in the "unused space" (slack space). 

But how do I access the slack space? After some reading, I decided to download a tool called `Autopsy`, which is part of the Sleuth Kit - 
difference being that Autopsy has a GUI instead of being terminal based.

Loading the image in Autopsy, I am able to view the root directory in the same way as previously shown.
In order to view slack files, I opened the settings menu (cog icon on the left panel) and deselected the hiding of slack files.

The result was a new file called `suspicious-file.txt-slack`, which contained the following:

`}1937befc_3<_|Lm_111t5_3b{FTCocip`

It is pretty clear that this is the flag, and all we have to do is to reverse the string!

Using an online reverse string tool gives us the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```
  picoCTF{b3_5t111_mL|_<3_cfeb7391}
  ```
</details>

