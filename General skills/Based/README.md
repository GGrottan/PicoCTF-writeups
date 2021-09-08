### Name: Based
### File: None
### Description: To get truly 1337, you must understand different data encodings, such as hexadecimal or binary. Can you get the flag from this program to prove you are on the way to becoming 1337? Connect with nc jupiter.challenges.picoctf.org 29221.
### Points: 200

When connecting with netcat, we are prompted with the following:

```console
gagr@ubntu:~/Documents/pico/General_skills/based$ nc jupiter.challenges.picoctf.org 29221
Let us see how data is stored
pear
Please give the 01110000 01100101 01100001 01110010 as a word.
...
you have 45 seconds.....

Input:

```

I initially thought that we have to convert the given binary to a string, however, looking more closely at the
output, a word is printed just before the "task" - in this case, "pear" is shown. 
The word that is displayed is different each time, but the result is the same:

```console
gagr@ubntu:~/Documents/pico/General_skills/based$ nc jupiter.challenges.picoctf.org 29221
Let us see how data is stored
falcon
Please give the 01100110 01100001 01101100 01100011 01101111 01101110 as a word.
...
you have 45 seconds.....

Input:
falcon
Please give me the  146 141 154 143 157 156 as a word.
Input:

```

So we didn't need to convert the binary after all. Doing a manual check for the displayed binary above 
confirms that the given word is the correct conversion: `01100110 01100001 01101100 01100011 01101111 01101110` = `falcon`.

The next challenge asks us to give a word based on a series of 3-digit numbers. 
I initially suspected that it was ascii codes, however, the values are way too high for it to make any sense.
Given that the task name is "based", and that the highest number used for the second task is 7,
it could be octal (base 8 - range between 0 and 7). I checked a few more times to confirm that we never get a number above 7.

Suspecting that this is octal, I created a small script that converts octal to string:

```python

input_value = input('enter octal: ')

converted = ''

for char in input_value.split(' '): # convert each 3-digit block

    converted += chr(int(char, 8))

print(converted)

```
Using this script, we get a step further:

```console
Please give me the  154 151 147 150 164 as a word.
Input:
light

Please give me the 6d6170 as a word.
Input:

```

```python
gagr@ubntu:~/Documents/pico/General_skills/based$ python3 octal.py 
enter octal: 154 151 147 150 164
light
```

The next step looks to be hexadecimal. This is also quite easy to convert using Python:

```python
input_value = input('input hex: ')

print(bytearray.fromhex(input_value).decode())

```

Using our two scripts, we obtain the flag after converting the hex code 🚩:

```console
gagr@ubntu:~/Documents/pico/General_skills/based$ nc jupiter.challenges.picoctf.org 29221
Let us see how data is stored
table
Please give the 01110100 01100001 01100010 01101100 01100101 as a word.
...
you have 45 seconds.....

Input:
table
Please give me the  164 145 163 164 as a word.
Input:
test
Please give me the 636f6c6f7261646f as a word.
Input:
colorado
You've beaten the challenge
Flag: picoCTF{learning_about_converting_values_00a975ff}
```

```python
gagr@ubntu:~/Documents/pico/General_skills/based$ python3 octal.py 
enter octal: 164 145 163 164
test
gagr@ubntu:~/Documents/pico/General_skills/based$ python3 hex.py 
input hex: 636f6c6f7261646f
colorado

```


