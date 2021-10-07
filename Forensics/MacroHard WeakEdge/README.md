### Name: MacroHard WeakEdge
### File: 'Forensics is fun'.pptm
### Description: I've hidden a flag in this file. Can you find it? Forensics is fun.pptm
### Points: 60

The given file for this task has the extension `.pptm`, meaning that it is a PowerPoint file.
Besides looking at the file type, the first thing that I did was inspecting the strings of the file,
just in case the flag can be found in plain sight.

Inspecting the strings did not reveal the flag, however, it revealed some interesting information:
- A `bin` file named "vbaProjects"
- A directory with a suspicious file: `ppt/slideMasters/hiddenPK`

From this, we know that the PowerPoint most likely uses macros, and that there is a file called hidden that we might want to take a look at.
Since we are interested in the files that are embedded, we won't need to view the presentation - we want to extract the files.

Having no clue on how to extract files from a PowerPoint, some searching on the internet told me that you can simply use regular `unzip` to 
extract the individual files. 

These are the files and directories after unzipping:

```console
┌──(user㉿kali)-[/pico/macrohard_weakedge]
└─$ ls
'[Content_Types].xml'   docProps  'Forensics is fun.pptm'   ppt   _rels
```
Remembering the suspicious file path from the strings, the `hidden` file should be located in `ppt/slideMasters`.

```console
┌──(user㉿kali)-[/pico/macrohard_weakedge/ppt/slideMasters]
└─$ ls
hidden  _rels  slideMaster1.xml
```
Inside the `hidden` file is the following:

```console
┌──(user㉿kali)-[/pico/macrohard_weakedge/ppt/slideMasters]
└─$ cat hidden
Z m x h Z z o g c G l j b 0 N U R n t E M W R f d V 9 r b j B 3 X 3 B w d H N f c l 9 6 M X A 1 f Q
```
Looking at the characters, it seems very likely that this is base64 - Mix of upperace and lowercase, numbers, and the range of characters in the alphabet.
Using a small [script that I created for these kinds of tasks](https://github.com/GGrottan/Snake-potion-of-minor-decipherment), 
the text was indeed base64, and the flag was revealed 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```
  ┌──(user㉿kali)-[~/tools/Snake-potion-of-minor-decipherment]
└─$ python3 spomd.py -B -m "ZmxhZzogcGljb0NURntEMWRfdV9rbjB3X3BwdHNfcl96MXA1fQ=="                                   

======================================

flag: picoCTF{D1d_u_kn0w_ppts_r_z1p5}

======================================
  ```
  
  Note: I removed the spaces between each character, and added padding in order for the string to be deciphered correctly.
</details>

