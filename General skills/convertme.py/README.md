## Name: convertme.py
#### Points: 100
#### Description: Run the Python script and convert the given number from decimal to binary to get the flag.
#### Files: convertme.py

Downloading and running the python file prompts us to convert a decimal number to binary: 

```console

┌──(gagr㉿desktop)-[/picomini2022/convertme]
└─$ python3 convertme.py
If 78 is in decimal base, what is it in binary base?
Answer:
```

Normally when doing these challenges under timed events, it is fairly trivial to find or use existing conversion tools - however, since the event has already ended, I
decided I could use the practice of how such a program could be written. Therefore, I created a small script to help with the conversion:

```python
# Take number as input
n_to_convert = input('number to convert: ')

def convert(n):
    binary_str = ''

    while n != 0: # While our number is not null
        remainder = int(n) % 2 # Calculate the binary digit
        binary_str += str(remainder) # Add the binary digit to the final output
        n = round(int(n)/2) # Define n for the next iteration
    return binary_str # Return the result

# Print the results
res = convert(n_to_convert)
print(res)
```

For the rules about converting decimal to binary, [this page](https://www.rapidtables.com/convert/number/decimal-to-binary.html) helped a lot.

Using our converter, we only needed to convert a decimal once, and we get the flag! 🚩
<details>
  <summary>Flag [Spoiler]</summary>
  
  ```console
  ┌──(gagr㉿desktop)-[/picomini2022/convertme]
  └─$ python3 convertme.py
  If 17 is in decimal base, what is it in binary base?
  Answer: 10001
  That is correct! Here's your flag: picoCTF{4ll_y0ur_b4535_e2a58836}
  ```
  
</details>

## A closer look on `convertme.py` 🕵️

Inspecting the contents of the file, it is very similar, if not exactly the same as I went through in one of the previous challenges, [Codebook](https://github.com/GGrottan/picoMini-2022/tree/main/Codebook).
Since I went over the previous script in great detail, I won't bother for this challenge, as the core functionality is exactly the same.

However, summarized, the flag is encrypted with by a password and a key - which is then XOR'ed to output a readable string. Both the encrypted flag and the password
is present in the file, so we could have reversed it if we needed to (foreshadowing? 🤔). 

This was a nice challenge for learning a bit more of how exactly we can convert decimal to binary, which turned out to be much less work
than converting a string to binary! I also discovered that a few "to binary" converters online actually converts a number as an ascii string instead of decimal,
which could be useful information for knowing why some converters don't seem to work at times!
