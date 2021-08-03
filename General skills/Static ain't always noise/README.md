#### Name: Static ain't always noise
#### Category: General skills
#### Description: Can you look at the data in this binary: static? This BASH script might help!
#### Points: 20

This task provides us with a binary (`static`) and a bash (`ltdis.sh`) file. 

Inspecting the `static` file with `cat` gives us a lot of garbage - not surprisingly.
Somewhat surprising, however, when using the `strings` command on the `static` file, 
we actually receive the flag 🚩 in plantext, without even having to use the provided bash script:

![](https://github.com/GGrottan/PicoCTF-writeups/blob/main/General%20skills/Static%20ain't%20always%20noise/img/strings.png)

The provided bash script essentially does the same thing for us, where the output looks a little nicer and more structured. 
This means that it is probably a practice task to get familiar with the `chmod +x` command, and how we can pass files to scripts.

