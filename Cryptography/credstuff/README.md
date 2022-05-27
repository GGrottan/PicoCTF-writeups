## Name: credstuff
#### Points: 100
#### Description: We found a leak of a blackmarket website's login credentials. Can you find the password of the user cultiris and successfully decrypt it? Download the leak here. The first user in usernames.txt corresponds to the first password in passwords.txt. The second user corresponds to the second password, and so on.
#### Files: `leak.tar`

We receive a `.tar`-file, which is a UNIX based archive file (aka zip file for linux). Firstly, we unzip the archive to reveal the contents:

```console

┌──(gagr㉿desktop)-[/picoCTF2022/cryptography/credstuff]
└─$ tar -xvf leak.tar
leak/
leak/passwords.txt
leak/usernames.txt

```
The result is a folder containing `passwords.txt` and `usernames.txt`. Inspecting these file, the usernames are written in plaintext,
while the passwords are encrypted. The description states that each line in both files correspond, and therefore we should firstly try to find the
correct password for the user `cultiris`. Using Python, we can find the correct index with the following: 

```python

indexCounter = 0
with open('usernames.txt') as unames:
    for name in unames:
        name = name.strip() # Remove whitespace
        if name != 'cultiris':
            indexCounter += 1 # Go to next name
        else:
            print('[+] Name found at index ' + str(indexCounter))

```

Running the script outputs the following:

```console

┌──(gagr㉿desktop)-[/picoCTF2022/cryptography/credstuff]
└─$ python3 locator.py
[+] Name found at index 377

```

We can also add a password check in the same script to output the corresponding password:

```python
indexCounter = 0
with open('usernames.txt') as unames:
    for name in unames:
        name = name.strip()
        if name != 'cultiris':
            indexCounter += 1
        else:
            print('[+] Name found at index ' + str(indexCounter))
            break

passwords = open('passwords.txt')
password = passwords.readlines()

print('Password on line 377 is ' + str(password[377]))

```

The output is now the following:

```console

┌──(gagr㉿desktop)-[/picoCTF2022/cryptography/credstuff]
└─$ python3 locator.py
[+] Name found at index 377
Password on line 377 is cvpbPGS{P7e1S_54I35_71Z3}

```

The password looks to be a shift cipher, since all of the characteristics of a flag is still present (e.g. capitalization and special characters).
We also know that every flag starts with `picoCTF`, so it is only a matter of shifting the letters until the flag is found. Alternatively, I like to
check for `ROT13` in these cases (this is just a normal shift done 13 times, but I've found that a lot of challenges uses this encipherment when shifting is involved).

Attempting to decipher the password with ROT13 reveals the flag 🚩:

<details>
  <summary>Flag [SPOILER]</summary>
  
  ```console
  
  ┌──(gagr㉿desktop)-[/picoCTF2022/cryptography/credstuff]
  └─$ echo "cvpbPGS{P7e1S_54I35_71Z3}" | rot13
  picoCTF{C7r1F_54V35_71M3}
  
  ```
  
  Note: you might not be able to use the 'rot13' command unless you have hxtools installed
  
</details>












