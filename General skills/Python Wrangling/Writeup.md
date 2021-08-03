#### Name: Python Wrangling
#### Category: General skills
#### Description: Python scripts are invoked kind of like programs in the Terminal... Can you run this Python script using this password to get the flag?
#### Points: 10

This challenge involves 3 files: `ende.py`, `flag.txt.en`, and `pw.txt`. 

We are asked to use the `ende.py` to decipher the `flag.txt.en` using the contents of `pw.txt`.

Firstly, we should probably learn how the python file is used. Programs often take the `--help` or `-h` arguement where
it outputs its intended use. Passing this argument gives us the following:

![](output)

We can see that the script takes 1 argument, either `-d` or `-e`, and gives us a pretty handy example as well.
Since `-d` means "decrypt", we can assume that `-e` means encrypt (which doesn't really matter in this case, as we are only interested in decrypting).

Using the provided example, our command for using the script should be the following: `python3 ende.py -d flag.txt.en`

Before executing the script, we should probably look at the contens of the `pw.txt`: 

![](pwcontent)

The content looks like it could be encrypted, however, we should probably try the password as it is before going any further.
Using the password provided, the flag is displayed in plaintext 🚩:

![](flag)

