## Name: File types
#### Points: 100
#### Description: This file was found among some files marked confidential but my pdf reader cannot read it, maybe yours can.
#### Files: `Flag.pdf`

We receive a file named `Flag.pdf`. Based on the given description, I suspected that the file we receive isn't a pdf at all, or possible hidden files.
Checking the file type, we can see that the file isn't a pdf at all: 

```console

┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/file-types]
└─$ file Flag.pdf
Flag.pdf: shell archive text

```

It is determined to be a `shell archive text` file, or `.shar`. 

Now what exactly is `shar`? A quick google search reveals: 
> Shar is a command-line tool and acts on a bunch of files at once, placing them in a single archive.

Okay, so we are most likely dealing with multiple files. Is there a way to extract them? In the same [article](https://www.maketecheasier.com/create-self-extracting-archives-shar-linux/) as I quoted above, 
the extraction process is simple: 
> When your friend receives the shar archive, all they need to do is make it executable and then run it. 

The article also states that we have to have `sharutils` installed in order to extract the contents. 

In order to change the shar-file to an executable, we can do the following: 

```console 

┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/file-types]
└─$ chmod +x Flag.pdf

```

Executing the file gives us the following output:

```console

┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/file-types]
└─$ ./Flag.pdf
x - created lock directory _sh00047.
x - extracting flag (text)
x - removed lock directory _sh00047.

```

The `lock directory` lines are just some standard output when executing shar-files, and we can see that a file named `flag` is extracted in the process.
The file is identified as text, however, attempting to inspect the file with the `strings` command only gives us nonsense output.

Checking the file type of `flag`:

```console

┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/file-types]
└─$ file flag
flag: current ar archive

```

So, the flag is a `current ar archive`. Some more googling revealed:
> a Unix utility that maintains groups of files as a single archive file.

Meaning that we have another file containing more files? 🤔 
In order to extract this type of archive, we can use a built-in tool called `ar`: 
> ar command is used to create, modify and extract the files from the archives

Looking at the help page for the `ar` command tells us that we need to add the `x` paramater in order to extract from the archive.

```console

┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/file-types]
└─$ ar x flag

```

Inspecting the directory, nothing has been added, but the file type of `flag` has been changed (possibly replaced the `.ar` file):

```console

┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/file-types]
└─$ file flag
flag: cpio archive

```

> File archive created in the Unix CPIO (Copy In, Copy Out) format, an uncompressed file container format used for grouping files together

Okay, another archive method. This took me a couple of minutes to figure out, but you should create another directory, and then unzip the `cpio` archive there,
because it won't extract when there's a file with the same name in the same directory. I managed to extract the `cpio` file with the following:

```console

┌──(gagr㉿desktop)-[/mnt/…/picoCTF2022/forensics/file-types/extracted]
└─$ cpio -id < /mnt/c/Users/User/Documents/kali/pico/picoCTF2022/forensics/file-types/flag
2 blocks

```
I created a new folder in the challenge folder, and specified the whole path for the `cpio` file. 
We are given a new file named `flag`:

```console
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ file flag
flag: bzip2 compressed data, block size = 900k
```

> bz2 file is a TAR archive, compressed with a Burrows-Wheeler (BZ2) compression algorithm, along with Run-Length Encoding (RLE) for better compression

Okay, trying to extract the `bz2` file gives the following output: 

```console

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ bzip2 -d flag
bzip2: Can't guess original name for flag -- using flag.out
```

Checking the file type of `flag.out`:

```console
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ file flag.out
flag.out: gzip compressed data, was "flag", last modified: Tue Mar 15 06:50:46 2022, from Unix, original size modulo 2^32 330
```

> Gzip is the standard file compression for Unix and Linux systems

Extracting gzip files, we can use a tool called `gunzip`. We simply need to pass it the file to unzip. There is, however, important to note that
gunzip cares about file-extensions! Therefore, we first need to create a copy that will work with gunzip:

```console
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ cp flag.out flag.gz
```

We can now extract the contents:

```console
┌──(gagr㉿DESKTOP-UU62MC8)-[/mnt/…/picoCTF2022/forensics/file-types/extracted]
└─$ gunzip flag.gz
```

We receive a new file called `flag`:

```console
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ file flag
flag: lzip compressed data, version: 1
```
.... *sigh* 😪 

using lzip, we can decompress the flag once more:

```console

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ lzip -d flag

```

Next, we have the following format:

```console

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ file flag.out
flag.out: LZ4 compressed data (v1.4+)
```

Using `lz4`, we can decompress and inspect the next compression:

```console
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ lz4 -d flag.out flag-lz

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ file flag-lz
flag-lz: LZMA compressed data, non-streamed, size 255
```

Using `lzma` to decompress and inspect the next compression:

```console 
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ cp flag-lz flag.lzma

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ lzma -d flag.lzma

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ file flag
flag: lzop compressed data - version 1.040, LZO1X-1, os: Unix

```
Performing the same operation with `lzop`:

```console
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ mv flag flag.lzop

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ lzop -d flag.lzop

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ file flag
flag: lzip compressed data, version: 1

```
Continuing:

```console 
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ mv flag.out flag.out.lz4

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ lzip -d flag

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ file flag.out
flag.out: XZ compressed data, checksum CRC64

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ cp flag.out flag.tar.xz

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ unxz -d flag.tar.xz

┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ file flag.tar
flag.tar: ASCII text

```
Finally, we get ASCII text! Inspecting the text outputs the following:

```console 
┌──(gagr㉿desktop)-[/picoCTF2022/forensics/file-types/extracted]
└─$ cat flag.tar
7069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f
6630725f3062326375723137795f35613833373565307d0a
````

* Letters from A-F
* No special characters
* Must be hex

Decoding the text as hexadecimal outputs the flag 🚩:

<details>
  <summary>Flag [SPOILER]</summary>
  
  ```
  picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_5a8375e0}
  ````
  
</details>



















