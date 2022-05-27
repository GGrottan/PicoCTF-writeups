## Name: PW Crack 1
#### Points: 100
#### Description: Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag in the same directory too.
#### Files: `level1.py`, `level1.flag.txt.enc`

The first thing I usually do with these types of tasks is to run the executable files as normal in order to better get a grasp of what needs to be done.
Running the `level1.py`-file prompts us to enter a password:

```console

┌──(gagr㉿DESKTOP-UU62MC8)-[/mnt/…/kali/pico/picomini2022/pw_crack_1]
└─$ python3 level1.py
Please enter correct password for flag:

```

Okay, so we need a password. Inspecting the python file might give us some clues: 

```python

### THIS FUNCTION WILL NOT HELP YOU FIND THE FLAG --LT ########################
def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])
###############################################################################


flag_enc = open('level1.flag.txt.enc', 'rb').read()



def level_1_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == "691d"):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")



level_1_pw_check()

```

Surely enough, the password is hardcoded. Entering the password given in the python-file gives us the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```
  ┌──(gagr㉿desktop)-[/picomini2022/pw_crack_1]
  └─$ python3 level1.py
  Please enter correct password for flag: 691d
  Welcome back... your flag, user:
  picoCTF{545h_r1ng1ng_56891419}
  ```
</details>
