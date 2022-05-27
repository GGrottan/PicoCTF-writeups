## basic-mod2
#### Points: 100
#### Description: A new modular challenge! Download the message here. Take each number mod 41 and find the modular inverse for the result. Then map to the following character set: 1-26 are the alphabet, 27-36 are the decimal digits, and 37 is an underscore. Wrap your decrypted message in the picoCTF flag format (i.e. picoCTF{decrypted_message})
#### Files: `message.txt`

This is the second installment of the "basic-mod" challenges, and works more or less in the same way as the [fist one](https://github.com/GGrottan/picoCTF-2022-writeups/tree/main/Cryptography/basic-mod1).
There are some slight differences in ranges for conversion and the number to calculate the results with, but we are also doing an additional calculation this time:
we are finding the modular inverse for each result of the modulo.

The modular inverse can look a little complicated, but simply put, it is a matter of finding values that when multiplied together, equals
`1 % m`. Example:

We want to find the inverse of `5 % 7`. We then go from `1` and add `7`: `7 + 1 = 8`. This is not a multiplicative of `5`, so we continue. 
`8 + 7 = 15`. This is a multiplicative of `5`: `5 * 3 = 15`, meaning that `3` is the inverse value that we are looking for. 

Another example: 

`5 % 11`

`1 + 11 = 12 <-- not a multiplicative of 5. Go next.`

`12 + 11 = 23 <-- not a multiplicative of 5. Go next.`

`23 + 11 = 34 <-- not a multiplicative of 5. Go next.`

`34 + 11 = 45 <-- This is a multiplicative of 5!`

`5 * 9 = 45`

`9 is the inverse value that we are looking for.`

The above explanation is very simplistic, but highlights what value we are looking for. In practice, the calculation is done as the following:

`a = 3 (multiplicative), m = 11 (modulo value), x = value that we want to find`

`((a % m) * (x % m)) % m == 1`

We use modulo on the multiplicative of known value and unknown value, in addition to modulo on the entire calculation, where the end result is `1`.

In the same fashion as the last challenge, we could perform the conversion manually, but it is much faster with a script. 
I modified the script used in the last basic-mod task:

```python

# Function taken from https://www.geeksforgeeks.org/multiplicative-inverse-under-modulo-m/
def modInverse(a, m):
    for x in range(1, m):
        if (((a%m) * (x%m)) % m == 1):
            return x
    return -1

# Open file and split numbers
with open('message.txt', 'r') as enciphered:
    for num in enciphered:
        values = num.split()

        # Handle each number as specified in the description
        flag = ''
        alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
        for value in values:

            # Use modInverse function
            conversion_type = modInverse(int(value), 41)

            # Convert to text if between 1 and 26
            if 1 <= conversion_type <= 26:
                flag += alphabet[conversion_type - 1] # subtract 1 because of index

            # Keep decimal digit if between 27 and 36
            elif 27 <= conversion_type <= 36:
                flag += str(conversion_type - 27) # Remove 27 to get correct decimal

            # Any other value, which should be 36
            else:
                flag += '_'


print('picoCTF{' + flag + '}')

```

Running the script output the flag 🚩:

<details>
  <summary>Flag [SPOILER]</summary>
  
  ```console
  
  ┌──(gagr㉿desktop)-[/picoCTF2022/cryptography/basic-mod2]
  └─$ python3 converter.py
  picoCTF{1NV3R53LY_H4RD_F6747912}
  
  ```
    
  
</details>

