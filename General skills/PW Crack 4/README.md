## Name: PW Crack 4
#### Points: 100
#### Description: Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag and the hash in the same directory too. There are 100 potential passwords with only 1 being correct. You can find these by examining the password checker script.
#### Files: `level4.flag.txt.enc`, `level4.hash.bin`, `level4.py`

This is the 4th password-cracking challenge, and follows the exact same setup as in [the third one](https://github.com/GGrottan/picoMini-2022-writeup/tree/main/PW%20Crack%203).
The only difference in this challenge, is that is has 100 possible passwords instead of 7.

Luckily, I automated the previous PW Crack challenge with two different approaches that can also be utilised here. We simply need to modify the scripts that were used in
the previous challenge! I am not going to explain any details of the scripts used here, since I've already done a detailed explanation in the write-up for PW Crack 3. 
I will, however, use both approaches, to illustrate that there are multiple ways to go about solving this challenge.

## Bruteforce approach
This approach uses bruteforce to find the correct password. We only need to replace the old list of possible passwords, and the name of the script to run in the terminal.

```python

import subprocess
import hashlib

pos_pw_list = ["6b3e", "989c", "4b17", "d06f", "f495", "6ea1", "44e4", "1d45", "3e1a", "b0b4", "8c65", "3276", "c496", "9d3d", "2476", "6ef4", "6b7f", "c184", "c2a8", "9708", "7bea", "9a2d", "4a22", "93ae", "826b", "9a50", "8b39", "5410", "a86c", "3760", "6426", "ec8e", "c294", "a909", "cbc6", "2e75", "f137", "9cb3", "79e7", "469f", "a9f9", "3e37", "b33e", "3f31", "4b27", "2f06", "cc2f", "d9e4", "2de7", "7328", "b4d4", "8e74", "a677", "b139", "9c74", "8ea4", "36f6", "613b", "7a7a", "5710", "838c", "44d5", "7190", "99d9", "c0a6", "b218", "3223", "477e", "38e5", "19b4", "3267", "2287", "b947", "a8d0", "fd9c", "e99c", "d8b7", "4c82", "b289", "332b", "bba5", "716d", "653e", "eb5d", "ad77", "ad3a", "3922", "7565", "947d", "928c", "2937", "823f", "f362", "79cf", "4582", "c0d0", "ed20", "d89a", "129c", "4e81"]

for pw in pos_pw_list:
    process = subprocess.run('echo ' + pw + '|' + 'python3 level4.py', shell=True, stdout=subprocess.PIPE)

    feedback = process.stdout.decode('utf-8')
    falseFlag = 'That password is incorrect'
    if (falseFlag not in feedback):
            print('[+] Flag found!\n' + feedback)

```

The script takes a few seconds, but it fulfills its purpose to retrieve the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```console
  ┌──(gagr㉿desktop)-[/picomini2022/pw_crack_4]
  └─$ python3 bruteforce.py
  [+] Flag found!
  Please enter correct password for flag: Welcome back... your flag, user:
  picoCTF{fl45h_5pr1ng1ng_89490f2d}
  ```

  </details>

## Hash-check approach
This approach is based on comparing generated hashes with a known hash. In the same fashion as the above-presented script, we only need to replace 
the known hash, and the list of potential passwords: 

```python

import hashlib

solution = "952ec6d4be3d1a40b28c020e88d5c775"
pos_pw_list = ["6b3e", "989c", "4b17", "d06f", "f495", "6ea1", "44e4", "1d45", "3e1a", "b0b4", "8c65", "3276", "c496", "9d3d", "2476", "6ef4", "6b7f", "c184", "c2a8", "9708", "7bea", "9a2d", "4a22", "93ae", "826b", "9a50", "8b39", "5410", "a86c", "3760", "6426", "ec8e", "c294", "a909", "cbc6", "2e75", "f137", "9cb3", "79e7", "469f", "a9f9", "3e37", "b33e", "3f31", "4b27", "2f06", "cc2f", "d9e4", "2de7", "7328", "b4d4", "8e74", "a677", "b139", "9c74", "8ea4", "36f6", "613b", "7a7a", "5710", "838c", "44d5", "7190", "99d9", "c0a6", "b218", "3223", "477e", "38e5", "19b4", "3267", "2287", "b947", "a8d0", "fd9c", "e99c", "d8b7", "4c82", "b289", "332b", "bba5", "716d", "653e", "eb5d", "ad77", "ad3a", "3922", "7565", "947d", "928c", "2937", "823f", "f362", "79cf", "4582", "c0d0", "ed20", "d89a", "129c", "4e81"]

for pw in pos_pw_list:
    hashed = hashlib.md5(pw.encode('utf-8')).hexdigest()
    if (hashed == solution):
        print('[+] Password found! ' + pw)
        break

```

This approach uses less time than the bruteforce method, and works as intended 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```console

  ┌──(gagr㉿desktop)-[/picomini2022/pw_crack_4]
  └─$ python3 hashGen.py
  [+] Password found! c2a8

  ┌──(gagr㉿DESKTOP-UU62MC8)-[/mnt/…/kali/pico/picomini2022/pw_crack_4]
  └─$ python3 level4.py
  Please enter correct password for flag: c2a8
  Welcome back... your flag, user:
  picoCTF{fl45h_5pr1ng1ng_89490f2d}

  ```

  </details>



