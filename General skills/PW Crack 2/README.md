## Name: PW Crack 2
#### Points: 100
#### Description: Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag in the same directory too.
#### Files: `level2.flag.txt.enc`, `level2.py`

This is the second level of PW Crack, and follows the same pattern as the previous task. I followed the same steps as in the first task, and instead
of the password being in plaintext in the python file, it is now a bit more obfuscated:

```python

def level_2_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == chr(0x34) + chr(0x65) + chr(0x63) + chr(0x39) ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

```

The password is now encrypted in hex instead of not being encrypted at all. Since the hex is being converted with a built-in function in Python, we can
simply copy and paste the password into a print-statement to retrieve the password:

```python

>>> print(chr(0x34) + chr(0x65) + chr(0x63) + chr(0x39))
4ec9

```

Entering this password gives us the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```
  ┌──(gagr㉿desktop)-[/picomini2022/pw_crack_2]
  └─$ python3 level2.py
  Please enter correct password for flag: 4ec9
  Welcome back... your flag, user:
  picoCTF{tr45h_51ng1ng_9701e681}
  ```
</details>
