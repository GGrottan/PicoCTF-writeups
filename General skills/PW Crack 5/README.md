 ## Name: PW Crack 5
 #### Points: 100
 #### Description: Can you crack the password to get the flag? Download the password checker here and you'll need the encrypted flag and the hash in the same directory too. Here's a dictionary with all possible passwords based on the password conventions we've seen so far.
 #### Files: `dictionary.txt`, `level5.flag.txt.enc`, `level5.hash.bin`, `level5.py`
 
 > NOTE: I downloaded the files for this challenge through the `wget` command, and somehow the hash for the flag changed (most likely my fault). I got stuck on this
 > task because I was searching and submitting passwords for a hash that did not exist. I had to re-download the files in order for it to work properly. 
 
 This is the fifth and last challenge related to the PW Crack series, and is a more elaborate version of the previous challenges.
 Instead of having a hardcoded password, or 7 to a 100 different potential passwords, we know have a whole dictionary of potential passwords.
 However, this doesn't change the methods that we've used in previous PW Crack challenges. All we need to do is to change the scripts according to a file with a list of 
 passwords. 
 
 Before getting started, I was a bit curious of exactly how many potential passwords we've got for this challenge. Finding the amount of potential passwords can
 be done by counting all of the lines in the `dictionary.txt` file:
 
 ```python
# Open the dictionary file
file = open('dictionary.txt', 'r')
lc = 0

# Increase the counter by 1 for each line
for line in file:
    lc += 1

print(lc)
```

We get the following result:

```console

┌──(gagr㉿desktop)-[/picomini2022/pw_crack_5]
└─$ python3 lineCounter.py
65536

```

I had to double check this number, but it turns out that for this task we actually have ~65 000 potential passwords. This is good, because then the difference between 
our previous methods (bruteforce and hash-comparison) might be better highlighted in terms of applicable scenarios. 
I am going to use the same two approaches as done in [PW Crack 3](https://github.com/GGrottan/picoMini-2022-writeups/tree/main/PW%20Crack%203) and [PW Crack 4](https://github.com/GGrottan/picoMini-2022-writeups/tree/main/PW%20Crack%204), 
because it is simply a matter of tweaking the scripts a bit.

## The Bruteforce Approach 🔨 

I suspected that performing the bruteforce method in the same fashion as in previous tasks will probably highlight how sluggish and uneffective this method can be.
It's also important to note that this approach will probably be very slow, not only because we are checking a lot of potential passwords, but also because 
the script that we're using is by no means optimized. At the core of our script, we are sending commands to the terminal and calling another external script - 
instead of performing all of the work at a single place. I am sure that it is possible to bruteforce this task in a much more effective manner, although that is 
beyond the scope of this write-up. 

I modified the script that I've used for previous tasks, to include the dictionary of potential passwords:

```python

import subprocess
import time

start_time = time.time()

pw_counter = 0
with open('dictionary.txt', 'r') as pos_pw:
    for pw in pos_pw:
        pw = pw.strip() # Remove new lines in order to send command
        pw_counter += 1
        process = subprocess.run('echo ' + pw + '|' + 'python3 level5.py', shell=True, stdout=subprocess.PIPE)

        feedback = process.stdout.decode('utf-8')
        falseFlag = 'That password is incorrect'
        print('ATTEMPTING: ' + pw, end='\r')
        if (falseFlag not in feedback):
            print('[+] Flag found!\n' + feedback)
            break

print('[+] Bruteforce complete. Attempted ' + str(pw_counter) + ' passwords')
end_time = round(time.time() - start_time, 2)
print('[+] Finished execution in ' + str(end_time) + ' seconds')
```

Executing the bruteforce script results in the following:

```console

┌──(gagr㉿desktop)-[/picomini2022/pw_crack_5]
└─$ python3 bruteforce.py
[+] Flag found!
Please enter correct password for flag: Welcome back... your flag, user:
picoCTF{<FLAG REMOVED FOR SPOILER>}

[+] Bruteforce complete. Attempted 63777 passwords
[+] Finished execution in 2276.48 seconds
```

From the final output, we can see that it took quite a while (in computer standards) to find the correct password, as we needed to check `~64 000` passwords by calling
another script, and then retrieving the output from the console. Converted to minutes, this approach used roughly <b>38 minutes</b> to complete, and the list of passwords
isn't that long either. This has shown that using the provided bruteforce method is not very sufficient when dealing with more than a couple of hundred passwords at the time.


## The Hash-comparison Approach #️

I theorized that this approach will be much more time-efficient compared to the bruteforce method when dealing with a lot more data - this turned out to be quite true.
Modifying the scripts that I've been using in previous tasks, I used the following configuration for checking hashes:

```python

import hashlib
import time

start_time = time.time()

# Use function from original script
def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()

solution = open('level5.hash.bin', 'rb').read()
pw_counter = 0
with open('dictionary.txt', 'r') as pos_pw:
    for pw in pos_pw:
        pw = pw.strip() # Remove any whitespace and newlines, tabs, etc.
        pw_counter += 1
        print('Current password: ' + pw, end='\r')
        hashed = hash_pw(pw)
        if (hashed == solution):
            print('[+] Password found! ' + pw)
            break

print('[+] Check complete. Checked ' + str(pw_counter) + ' passwords')
end_time = round(time.time() - start_time, 2)
print('[+] Finished execution in ' + str(end_time) + ' seconds')

```

I implemented the hashing function from the original script, as well as retrieving the correct hash in the same way. Everything else is more or less the same
that I've been executing on the previous challenges. At the end of executing this script, we get the same result as in the bruteforce approach, however, the
time it took this script to find the correct password is reduced significantly:

```console

┌──(gagr㉿desktop)-[/picomini2022/pw_crack_5]
└─$ python3 hashGen.py
[+] Password found! f920
[+] Check complete. Checked 63777 passwords
[+] Finished execution in 0.71 seconds

```

It is clear that this approach is way more efficient in finding the password when dealing with a lot of data. It should also be noted that the times that are
shown can vary a lot, depending on how the time is calculated and how your setup is configured. The times are only generous estimates for displaying the broad difference
in effectiveness. 

Executing `level5.py` and entering the password we got from our approaches reveals the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```
  ┌──(gagr㉿desktop)-[/picomini2022/pw_crack_5]
  └─$ python3 level5.py
  Please enter correct password for flag: f920
  Welcome back... your flag, user:
  picoCTF{h45h_sl1ng1ng_2f021ce9}
  ```
</details>



 
