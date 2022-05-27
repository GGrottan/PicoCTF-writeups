## Name: patchme.py
#### Points: 100
#### Description: Can you get the flag? Run this Python program in the same directory as this encrypted flag.
#### Files: `flag.txt.enc`, `patchme.flag.py`

We receive two files, where one is an encrypted flag, and the other is a python script to decipher the flag. 
Running the python file prompts us to input a password. Since we don't have any password yet, it might be useful to inspect the source code
for the file.

Inspecting the python file, this particular section is interesting:

```python

    if( user_pw == "ak98" + \
                   "-=90" + \
                   "adfjhgj321" + \
                   "sleuth9000"):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), "utilitarian")

```

The correct password is hardcoded, and we only need to put the hardcoded strings together, as such:

```
ak98-=90adfjhgj321sleuth9000
```

Running the python script again, and inputting the above string gives us the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>
  
  ```console 
  
  ┌──(gagr㉿desktop)-[/pico/picoCTF2022/reverse-engineering/patchme-py]
  └─$ python3 patchme.flag.py
  Please enter correct password for flag: ak98-=90adfjhgj321sleuth9000
  Welcome back... your flag, user:
  picoCTF{p47ch1ng_l1f3_h4ck_4d5af99c}
  
  ```
  
</details>
