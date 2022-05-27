## Name: transposition-trial
#### Points: 100
#### Description: Our data got corrupted on the way here. Luckily, nothing got replaced, but every block of 3 got scrambled around! The first word seems to be three letters long, maybe you can use that to recover the rest of the message.
#### Files: `message.txt`

## 

Inspecting the contents of `message.txt`:

```console
heTfl g as iicpCTo{7F4NRP051N5_16_35P3X51N3_VCDE4CE4}7
```

We get receive some useful information in the description, where it is said that every block of 3 is scrambled. We can test this manually on the first couple of blocks:

`heT` --> Moving the first character to the front gives us `The`. 

`fl ` --> ` fl` --> We have to account for the spaces

`g a` --> `ag `

Putting the first three blocks together gives us: `The flag`.

Assuming that the last character of every block of 3 is moved to the front, we can do the rest with Python:

```python

ciphertext = 'heTfl g as iicpCTo{7F4NRP051N5_16_35P3X51N3_VCDE4CE4}7'

# Empty to hold plaintext
plaintext = ''

# For each character in the ciphertext
for i in range(0, len(ciphertext), 3): # Skip 3 because of block length
    block = ciphertext[i:i+3] # Create block of three letters
    plaintext += block[2] + block[:2] # Take the last char (index 2) and append first two (:2) chars.

print(plaintext)

```

Running the script presented above deciphers the message 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```

  ┌──(gagr㉿desktop)-[/picoCTF2022/cryptography/transpositional-trial]
  └─$ python3 descramble.py
  The flag is picoCTF{7R4N5P051N6_15_3XP3N51V3_ECDE4C74}

  ```

</details>
