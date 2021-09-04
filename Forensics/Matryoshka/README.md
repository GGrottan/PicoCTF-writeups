### Name: Matryoshka doll
### Category: Forensics
### Description: Matryoshka dolls are a set of wooden dolls of decreasing size placed one inside another. What's the final one?
### Points: 30

The given file for this task is named `dolls.jpg`. The first thing I did was to check whether this is actually a jpg file. 
According to the file-command, it is not a .jpg, but a .png:

![](https://github.com/GGrottan/PicoCTF-writeups/blob/main/Forensics/Matryoshka/img/file.png)

It doesn't tell us much, however, since this file is actually a .png, we cannot use Steghide to extract embedded files.

Before attempting to extract any data, the strings-command might give some additional useful information, and although the output was mostly garbage, one of the strings stood out: `base_images/2_c.jpgUT` - which looks like a directory containing another image file. 

Following the advice of [0xRick](https://0xrick.github.io/lists/stego/), I used `Foremost -i dolls.jpg` to check for embedded files.

Sure enough, Foremost found an image file and a .zip file. The image file seemed to be the same image that we initially got, while the zip file contained the `base_images/2_c.jpg` directory and file that we found from the strings-command. 

Not surprisingly, the `2_c.jpg` also contained a .zip with the directory and file `base_images/3_c.jpg`. 

At this point I was thinking of creating a script to keep extracting and unzipping until there were no more zip-files, however after extracting `3_c.jpg`, and looking at the zip-file in `4_c.jpg`, instead of containing another image, it contained `flag.txt`. 

Looking at the contents of this file gives us the flag! 🚩

![](https://github.com/GGrottan/PicoCTF-writeups/blob/main/Forensics/Matryoshka/img/flag.jpg)


