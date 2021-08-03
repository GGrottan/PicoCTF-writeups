#### Name: Tab, Tab, Attack
#### Category: General skills
#### Description: Using tabcomplete in the Terminal will add years to your life, esp. when dealing with long rambling directory structures and filenames: Addadshashanammu.zip
#### Points: 20

This task provides us with a `.zip` file, and based on the description, I already suspect that this file contains a long directory tree.
Downloading the `zip` and extracting the contents with the `unzip` command gives us a folder called `Addadshashanammu`. 
Moving into this folder and listing the contents, another directory called `Almurbalarammi` is present. 

My suspicion seems to be correct, and it seems like this challenge is meant to teach how we can use `TAB` to quickly redirect into directories.

By repeatedly pressing `TAB` until there are no more directories to move into, an executable called `fang-of-haynekhtnamet` is present.
Executing this file gives us the flag 🚩. I stored the results of the output to a file called `flag.txt` in the main directory:

![](flag)



