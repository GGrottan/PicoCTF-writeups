## Name: fixme1.py
#### Points: 100
#### Description: Fix the syntax error in this Python script to print the flag.
#### Files: `fixme1.py`

This challenge can be solved in multiple ways, however, I have worked quite a bit with Python, and my initial step was to simply output the file contents
to see if I can spot the syntax error without running the file. 

Upon seeing the code, it was pretty clear that there was going to be an indentation error, as the last print-statement is indentet, which Python doesn't like.
Upon trying to run the file, my suspicion was confirmed:

```console

┌──(gagr㉿desktop)-[/picomini2022/fixme1]
└─$ python3 fixme1.py
  File "/picomini2022/fixme1/fixme1.py", line 20
    print('That is correct! Here\'s your flag: ' + flag)
IndentationError: unexpected indent
```
I opened the file, removed the indent, and the flag successfully printed 🚩: 


<details>
  <summary>Flag [Spoiler]</summary>

  ```
  ┌──(gagr㉿DESKTOP-UU62MC8)-[/mnt/…/kali/pico/picomini2022/fixme1]
  └─$ python3 fixme1.py
  That is correct! Here's your flag: picoCTF{1nd3nt1ty_cr1515_09ee727a}
  ```
</details>
