### Name: Mus1c
### File: Lyrics.txt
### Description: I wrote you a song. Put it in the picoCTF{} flag format.
### Points: 300

We receive a `.txt` file, which contains the following ciphertext:

```console
gagr@ubntu:~/Documents/pico/General_skills/mus1c$ cat lyrics.txt 
Pico's a CTFFFFFFF
my mind is waitin
It's waitin

Put my mind of Pico into This
my flag is not found
put This into my flag
put my flag into Pico


shout Pico
shout Pico
shout Pico

My song's something
put Pico into This

Knock This down, down, down
put This into CTF

shout CTF
my lyric is nothing
Put This without my song into my lyric
Knock my lyric down, down, down

shout my lyric

Put my lyric into This
Put my song with This into my lyric
Knock my lyric down

shout my lyric

Build my lyric up, up ,up

shout my lyric
shout Pico
shout It

Pico CTF is fun
security is important
Fun is fun
Put security with fun into Pico CTF
Build Fun up
shout fun times Pico CTF
put fun times Pico CTF into my song

build it up

shout it
shout it

build it up, up
shout it
shout Pico
```

My initial reaction was that the "lyrics" look a bit like obfuscated assembly, with the "put", "shout" and "into"
words frequently being used. After many failed attempts at trying to google my way to what this could be, I checked the hint for the task:

`Do you think you can master rockstar?`

It turns out that there is a funny programming language called [Rockstar](https://codewithrockstar.com/docs), where code is converted into
song lyrics! Looking at the documentation and examples, this looks very similar to the lyrics in our file.

I also found a neat [Python compiler](https://github.com/yyyyyyyan/rockstar-py) for Rockstar code, however, I couldn't
manage to get it to work, as I kept getting syntax error for one of the variables in the code.

Luckily, the official rockstar webpage also has a [compiler](https://codewithrockstar.com/online) that lets me run the script!

The output of `lyrics.txt` looks like the following:

```bash
114
114
114
111
99
107
110
114
110
48
49
49
51
114
```
With numbers such as these, where there are 3-digit and 2-digit numbers above 32, I suspected that
they are ascii codes. I created a small Python script that converts the ascii values to letters:

```python
input_value = input('input some ascii: ')

output = ''

for n in input_value.split(' '):

    output += chr(int(n))


print('picoCTF{'+ output + '}')
```

Feeding our script with the output of the `lyrics.txt` file gives us the flag 🚩:

```console 
gagr@ubntu:~/Documents/pico/General_skills/mus1c$ python3 ascii.py 
input some ascii: 114 114 114 111 99 107 110 114 110 48 49 49 51 114
picoCTF{rrrocknrn0113r}
```
