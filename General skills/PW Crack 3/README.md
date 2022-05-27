## Name: PW Crack 3
#### Points: 100
#### Description: Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag and the hash in the same directory too. There are 7 potential passwords with 1 being correct. You can find these by examining the password checker script.
#### Files: `level3.flag.txt`, `level3.py`, `level3.hash.bin`

This is the third installment of the password cracking series of tasks, and for this challenge, we receive an additional file.
This time, it is stated in the description that we need to inspect the Python script. 

Inspecting the script, we can see that the password check is more or less the same as the two previous tasks: 

```python

def level_3_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    user_pw_hash = hash_pw(user_pw)

    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
    print("That password is incorrect")

```

However, and additional function is presented: `hash_pw()`:

```python

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()

```

Since we already created a similar function in one of the previous challenges for this competition, it is pretty clear that this function generates an MD5
hash for a given string. Additionally, we are also given 7 different possible passwords:

```python

# The strings below are 7 possibilities for the correct password.
#   (Only 1 is correct)
pos_pw_list = ["8799", "d3ab", "1ea2", "acaf", "2295", "a9de", "6f3d"]

```

What's also important to note is that the `correct_pw_hash` variable is imported from the `level3.hash.bin`:

```python
correct_pw_hash = open('level3.hash.bin', 'rb').read()
```

To summarize our findings so far:

* We have a list of 7 different possible passwords. This list is not used anywhere in the code, but serves as a hint
* When prompted to type a password, our input is hashed
* Our hashed input is then compared with the hash that is found in the `level3.hash.bin` file

Based on our findings, we have multiple options for how to solve this task. Therefore, I will present two different approaches.

## Solution #1 - Plain and simple bruteforce 🔨

We only have seven different combinations, which is more than ideal to bruteforce. Additionally, we don't even have to process the possible passwords,
since our input gets hashed in the authentication process. Therefore, we simply need to call the script and pass each possible password until we get it right!

We could probably do this manually in less than a minute, however, I am a big fan of using way more time trying to automate the task instead! 💪

A way this could be automated, is to iterate through each possible password, and then submit the password when calling the script. Simply put, 
we call the script and write a new password until we get the right one. 

But how do we know if a password is correct or not? 🤔 Easy - when we type an invalid password, we always receive the following message: `That password is incorrect`.
This means that if we check the output for this particular string, we try the next password until the message isn't found. We could of course make this shorter by
only checking for `incorrect`, but then we might run into the scenario of receiving `That password is not incorrect`, which would omit the correct solution.

Below is the script I used for bruteforcing the password: 

```python

# Import libraries
import subprocess
import hashlib

# List of possible passwords
pos_pw_list = ["8799", "d3ab", "1ea2", "acaf", "2295", "a9de", "6f3d"]

for pw in pos_pw_list:
    # Send commands to the terminal
    # Print the password along with calling the script to get the response
    process = subprocess.run('echo ' + pw + '|' + 'python3 level3.py', shell=True, stdout=subprocess.PIPE)
    
    # Handle the response according to what it says
    feedback = process.stdout.decode('utf-8')
    falseFlag = 'That password is incorrect'
    if (falseFlag not in feedback):
            print('[+] Flag found!\n' + feedback)
            break

```

The script works as intended, and the flag is found 🚩: 


<details>
  <summary>Flag [Spoiler]</summary>

  ```
  ┌──(gagr㉿desktop)-[/picomini2022/pw_crack_3]
  └─$ python3 pw_bruteforce.py
  [+] Flag found!
  Please enter correct password for flag: Welcome back... your flag, user:
  picoCTF{m45h_fl1ng1ng_6f98a49f}
  ```
</details>


## Solution #2 - A methodological approach 🧐

If we imagine that instead of 7 possible passwords, we were given millions of possible passwords, the bruteforce approach might not be the best.
We know that the correct password is stored in the `level3.hash.bin` file, and so we should probably see what the contents of that file is. Opening the file
as a normal text file won't work, because it is a binary file. We can, however, inspect the file with tools such as `hexdump`:

```console
┌──(gagr㉿desktop)-[/picomini2022/pw_crack_3]
└─$ hexdump -C level3.hash.bin
00000000  8f 60 45 8c c6 42 43 ba  3b 88 f8 bf cf a2 69 eb  |.`E..BC.;.....i.|
```
It doesn't tell us much at first glance, but there is a very important detail to this: when the file is opened and read from the script, 
the `rb` argument is specified: 
```python 
correct_pw_hash = open('level3.hash.bin', 'rb').read()
```
This means that the file is opened in a binary format for reading, 
and so the text output for this file is not relevant. Looking at the source code for the script, the contents from the `level3.hash.bin` file is never
further processed. It is only read from the file, and then compared with the generated hash of our input:
```python
if( user_pw_hash == correct_pw_hash ):
```

This means that the the hex presented above is actually the MD5 hash for the correct password!

We simply need to loop through all of the possible passwords, generate a hash for the current password, and compare it to the binary contents of `level3.hash.bin`:

```python
import hashlib

# Known hash and list of possible passwords
solution = "8f60458cc64243ba3b88f8bfcfa269eb"
pos_pw_list = ["8799", "d3ab", "1ea2", "acaf", "2295", "a9de", "6f3d"]

for pw in pos_pw_list:
    # Generate hash for each possible password and compare with known hash
    # Alternative: Generate all hashes outside of loop and compare list of hashes with known hash
        # The drawback of this is that we might generate a number of unecessary hashes
    hashed = hashlib.md5(pw.encode('utf-8')).hexdigest()
    if (hashed == solution):
        print('[+] Password found! ' + pw)
        break
```

The script works as intended, and we can use the output to get the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```console
    ┌──(gagr㉿desktop)-[/picomini2022/pw_crack_3]
    └─$ python3 hashGen.py
    [+] Password found! 1ea2

    ┌──(gagr㉿desktop)-[/picomini2022/pw_crack_3]
    └─$ python3 level3.py
    Please enter correct password for flag: 1ea2
    Welcome back... your flag, user:
    picoCTF{m45h_fl1ng1ng_6f98a49f}

  ```

  </details>












