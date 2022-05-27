## Name: basic-mod1
#### Points: 100
#### Description: We found this weird message being passed around on the servers, we think we have a working decrpytion scheme. Download the message here. Take each number mod 37 and map it to the following character set: 0-25 is the alphabet (uppercase), 26-35 are the decimal digits, and 36 is an underscore. Wrap your decrypted message in the picoCTF flag format (i.e. picoCTF{decrypted_message})
#### Files: `message.txt`

We are given an enciphered message that looks like the following:

```console
┌──(gagr㉿desktop)-[/picoCTF2022/basic-mod1]
└─$ cat message.txt
128 63 131 198 262 110 309 73 276 285 316 161 151 73 219 150 145 217 103 226 41 255
```
Luckily, we are also given the decipherment-scheme.
The remainder value when using modulus 37 should be converted by the following scheme: 

* 0-25 is the uppercase alphabet
* 26-35 is the decimal digit
* 36 is an underscore

It is possible to do this manually, but a lot more efficient if we create a script to convert these values instead.
I created a small python script that converts each value according to the provided scheme:

```python
# Open file and split numbers
with open('message.txt', 'r') as enciphered:
    for num in enciphered:
        values = num.split()

        # Handle each number as specified in the description
        flag = ''
        alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
        for value in values:
            conversion_type = int(value) % 37

            # Convert to text if between 0 and 25
            if 0 <= conversion_type <= 25:
                flag += alphabet[conversion_type]

            # Decimal digit if between 26 and 35
            elif 26 <= conversion_type <= 35:
                flag += str(conversion_type - 26)

            # Any other value, which should be 36
            else:
                flag += '_'


print('picoCTF{' + flag + '}')
```

I had to modify the script a couple of times before I got it, because the decimal conversion was a little confusing. I tried to keep the remainder value, as well
as keeping the original value, but the flags came back as wrong each time. Looking at the range for the numbers to be decimals, there are 9 numbers between 26 and 35 (and 10 indexes).
I then figured that I shouldn't have the original values or the remainder values, but rather the index of the number in the range between 26 and 35! 
This means that if a remainder value is 27, it is in the 1st index in the range, and should thus be 1. 

Running the script outputs the flag 🚩:

<details>
  <summary>Flag [SPOILER]</summary>
  
  ```console
  
  ┌──(gagr㉿desktop)-[/picoCTF2022/basic-mod1]
  └─$ python3 converter.py
  picoCTF{R0UND_N_R0UND_8C863EE7}
  
  ```
  
  
</details>
