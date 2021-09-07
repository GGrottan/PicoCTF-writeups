### Name: Trivial Flag Transfer Protocol
### File: tftp.pcapng
### Description: Figure out how they moved the flag.
### Points: 90

The given file in this task is a `.pcapng`, so the obvious first step was to view the contents in Wireshark.
Upon viewing the contents of the file, the first thing I noticed was that the TFTP protocol was very prominent.

Not being very familiar with this spesific protocol, I scimmed some explanations on the internet, and the protocol is apparently used 
for transferring files (TFTP - Trivial File Transfer Protocol (who would have guessed)).

What is interesting about this protocol is that, unlike FTP, [TFTP uses UDP for its transportation, and requires no user authentication.](https://www.firewall.cx/networking-topics/protocols/126-tftp-protocol.html)
(Spoiler: It turned out that this information isn't really necessary, and it would be enough to know that files are being tranferred - though this information might be useful for other CTF's!)

Knowing that we are looking at files being transferrede, Wireshark has a handy function that let's us export these!
`File -> Export Objects -> TFTP ..` lets us see and save the files that are present:

- instructions.txt
- plan (unknown filetype)
- program.deb
- picture1.bmp
- picture2.bmp
- picture3.bmp

Inspecting the `instructions.txt` first, gives us the following:

```console
gagr@ubntu:~/Documents/pico/Forensics/trivial_flag_transfer$ cat instructions.txt 
GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA

```
- The file suggests that it is something that should be read (either by input in some application, or by another human)
- The string only contains letters in the English alphabet, which excludes a lot of encryption methods

Considering the points above, I decided to pass this string to a small script that I made for [dealing with shift ciphers](https://github.com/GGrottan/classic-shift-cipher)

A bit surpised myself, the ecniphered string turned out to be shifted 13 places (also known as `ROT13`) : 

```console 
gagr@ubntu:~/Documents/python/cryptology/tools$ python3 shift_cipher.py --bruteforce "GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA"
...
shifting 13 places: tftpdoesntencryptourtrafficsowemustdisguiseourflagtransfermfigureoutawaytohidetheflagandiwillcheckbackfortheplan
...

```

Cleaning up the output, we get the following message: `tftp doesnt encrypt  our traffic so we must disguise our flag transfer. figure out a way to hide the flag and i will check back for the plan`

"the plan" - very convinient that we also have a file called "plan" 🤔

Inspecting the file type and content, we get the following:

```console
gagr@ubntu:~/Documents/pico/Forensics/trivial_flag_transfer$ file plan
plan: ASCII text

gagr@ubntu:~/Documents/pico/Forensics/trivial_flag_transfer$ cat plan
VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF

```

Looks like to be enciphered with the same scheme as before. Since we already know that it is shifted 13 places, we can try to
decipher the string directly in the terminal instead of using a script for it:

```console
gagr@ubntu:~/Documents/pico/Forensics/trivial_flag_transfer$ echo "VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF" | rot13
IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS

```

`I used the program and hid it with - duediligence. check out the photos`

My next step was to see if there was anything interesting about the photos that we obtained. I usually begin with something obvious, 
like seeing if there are any "hidden in plain sight" strings that are embedded. 

None of the images provided any interesting strings, however, `picture2.bmp` had a LOT more compared to the two other images.
Looking at the file sizes for each image:

- `picture1.bmp`: 824 KB
- `picture2.bmp`: 36 MB
- `picture3.bmp`: 1,4 MB

At this point I was pretty sure that the second image had some embedded secrets, as none of the images gave any apparent information when displayed. I decided to try to see if I could extract any files using Steghide.
When attempting to extract, I was prompted to give a passphrase:

```console
gagr@ubntu:~/Documents/pico/Forensics/trivial_flag_transfer$ steghide extract -sf picture2.bmp
Enter passphrase: 
steghide: could not extract any data with that passphrase!

```

Looking at the deciphered message we got from the `plan` file: `I used the program and hid it with - duediligence. check out the photos`, 
I thought that the word "duediligence" was written in a suspicious manner, as if the word itself was a passphrase 🤔

Using this passphrase - both in either all uppercase or lowercase gave no results for `picture2.bmp`, so I thought maybe it only works
for one of the pictures, and then we get another passphrase, and so on. The passphrase did not work for `picture1.bmp` either, however,
using the passphrase in all uppercase for `picture3.bmp` showed some interesting results 🚩:

```console
gagr@ubntu:~/Documents/pico/Forensics/trivial_flag_transfer$ steghide extract -sf picture3.bmp 
Enter passphrase: 
wrote extracted data to "flag.txt".

gagr@ubntu:~/Documents/pico/Forensics/trivial_flag_transfer$ cat flag.txt 
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}

```

But what about the `program.deb` file?
Extracting the contents of the file gives us a directory called `usr`. Inside of this directory, we have two new directories: `bin` and `share`.
Looking inside these directories, it turned out that the `program.deb` is actually Steghide! 
I guess the intention was to provide the correct application for extracting the flag from the third image, and I coincidentally used the intended 
program; resulting in that I practially skipped a step in the process.

Also, why was the file size for `picture2.bmp` so large compared to the others, when the flag was hidden in `picture3.bmp`? It doesn't really matter
since we already obtianed the flag, but my very limited educational guess is that it is a red herring, and it is only supposed to waste our time.

