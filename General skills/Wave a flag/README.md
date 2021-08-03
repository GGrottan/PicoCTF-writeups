#### Name: Wave a flag
#### Category: General skills
#### Description: Can you invoke help flags for a tool or binary? This program has extraordinarily helpful information...
#### Points: 10

This challenge provides us with a file named `warm`. By inspecting the file type, it appears to be an executable. 
Using `chmod +x`, we can execute the file to see what it does:

![](https://github.com/GGrottan/PicoCTF-writeups/blob/main/General%20skills/Wave%20a%20flag/img/chmod.png)

Executing the file, we receive the following message:

![](https://github.com/GGrottan/PicoCTF-writeups/blob/main/General%20skills/Wave%20a%20flag/img/exec.png)

Executing the program once again, but adding `-h` as an argument, we receive the flag 🚩:

![](https://github.com/GGrottan/PicoCTF-writeups/blob/main/General%20skills/Wave%20a%20flag/img/flag.png)
