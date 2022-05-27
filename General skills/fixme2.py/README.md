## Name: fixme2.py
#### Points: 100
#### Description: Fix the syntax error in the Python script to print the flag.
#### Files: `fixme2.py`

This is the second part of this type of challenge, where the task is to find and fix a typo in a given python script.
Looking at the script, it might not be obvious at first glance - however, running the script will highlight the error: 

```console
┌──(gagr㉿desktop)-[/picomini2022/fixme2]
└─$ python3 fixme2.py
  File "/mnt/c/Users/User/Documents/kali/pico/picomini2022/fixme2/fixme2.py", line 22
    if flag = "":
            ^
SyntaxError: invalid syntax
```
The syntax is marked as invalid because we're missing a `=`. This is because a single `=` means that we're setting a variable, while a double `==` means
that we are comparing.

Fixing this typo and running the script reveals the flag 🚩: 

<details>
  <summary>Flag [Spoiler]</summary>

  ```
  ┌──(gagr㉿DESKTOP-UU62MC8)-[/mnt/…/kali/pico/picomini2022/fixme2]
  └─$ python3 fixme2.py
  That is correct! Here's your flag: picoCTF{3qu4l1ty_n0t_4551gnm3nt_4863e11b}
  ```
</details>
