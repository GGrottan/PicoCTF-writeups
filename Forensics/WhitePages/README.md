### Name: WhitePages
### File: whitepages.txt
### Description: I stopped using YellowPages and moved onto WhitePages... but the page they gave me is all blank!
### Points: 250

As the description suggests, inspecting the contents of the given file shows nothing in particular.
However, the file seems to have some kind of content, as there are multiple blank lines. 

Inspecting the file with hexdump gives the following output: 

```console
┌──(user㉿kali)-[/pico/white_pages]
└─$ hexdump -C whitepages.txt
00000000  e2 80 83 e2 80 83 e2 80  83 e2 80 83 20 e2 80 83  |............ ...|
00000010  20 e2 80 83 e2 80 83 e2  80 83 e2 80 83 e2 80 83  | ...............|
00000020  20 e2 80 83 e2 80 83 20  e2 80 83 e2 80 83 e2 80  | ...... ........|
00000030  83 e2 80 83 20 e2 80 83  e2 80 83 20 e2 80 83 20  |.... ...... ... |
00000040  20 20 e2 80 83 e2 80 83  e2 80 83 e2 80 83 e2 80  |  ..............|
00000050  83 20 20 e2 80 83 20 e2  80 83 e2 80 83 20 e2 80  |.  ... ...... ..|
00000060  83 20 20 e2 80 83 e2 80  83 e2 80 83 20 20 e2 80  |.  .........  ..|
00000070  83 20 20 e2 80 83 20 20  20 20 e2 80 83 20 e2 80  |.  ...    ... ..|
00000080  83 e2 80 83 e2 80 83 e2  80 83 20 20 e2 80 83 20  |..........  ... |
00000090  e2 80 83 20 e2 80 83 20  e2 80 83 e2 80 83 e2 80  |... ... ........|
000000a0  83 20 e2 80 83 e2 80 83  e2 80 83 20 20 e2 80 83  |. .........  ...|
000000b0  e2 80 83 e2 80 83 e2 80  83 e2 80 83 20 e2 80 83  |............ ...|
000000c0  20 e2 80 83 e2 80 83 e2  80 83 e2 80 83 e2 80 83  | ...............|
.....
.....
```
At first I thought this might be morse code, however, there are way too many "dots" for it to make sense.

Taking a portion of the blank text and using an [online interpreter](https://babelstone.co.uk/Unicode/whatisit.html)
shows that the blank content is a mix of two different spaces:

```console
U+2003 : EM SPACE {mutton}
U+0020 : SPACE [SP]
U+2003 : EM SPACE {mutton}
U+2003 : EM SPACE {mutton}
U+0020 : SPACE [SP]
U+2003 : EM SPACE {mutton}
U+2003 : EM SPACE {mutton}
U+0020 : SPACE [SP]
```

Two types of characters that are alternating at seemingly random to create some kind of message sounds pretty similar to a famous
base-2 number system 🤔 (**Hint: It is binary**)

Converting these spaces to `0's` and `1's` shouldn't be too painful, so I created a Python script that loops through the file content
and replaces `EM space` with `1` and `space` with `0`. This turned out to only produce garbage, so I switched the replacements, and got the flag 🚩:


Script to replace spaces with binary
```python

f = open('whitepages.txt', 'r')

file_content = f.read()

emspace = '\u2003'
space = '\u0020'

output = ''

for element in file_content:
    if element == emspace:
        output += '0'
    else:
        output += '1'

print(output)
```

Output of [a script I made](https://github.com/GGrottan/Snake-potion-of-minor-decipherment) for decoding common CTF encipherments:

```console
┌──(user㉿kali)-[/pico/white_pages]
└─$ python3 ~/tools/Snake-potion-of-minor-decipherment/spomd.py --binary --message "00001010000010010000100101110000011010010110001101101111010000110101010001
0001100000101000001010000010010000100101010011010001010100010100100000010100000101010101000010010011000100100101000011001000000101001001000101010000110100111101
0100100100010001010011001000000010011000100000010000100100000101000011010010110100011101010010010011110101010101001110010001000010000001010010010001010101000001
00111101010010010101000000101000001001000010010011010100110000001100000011000000100000010001100110111101110010011000100110010101110011001000000100000101110110011
0010100101100001000000101000001101001011101000111010001110011011000100111010101110010011001110110100000101100001000000101000001000001001000000011000100110101001
10010001100010011001100001010000010010000100101110000011010010110001101101111010000110101010001000110011110110110111001101111011101000101111101100001011011000
1101100010111110111001101110000011000010110001101100101011100110101111101100001011100100110010101011111011000110111001001100101011000010111010001100101011001
000101111101100101011100010111010101100001011011000101111101100011001101010011010001100110001100100011011101100011011001000011000000110101011000110011001000
11000100111000001110010110011000111000001100010011010000110111011000110110001100110110011001100011010101100100011001010110001000110010011001010011010100110110
01111101000010100000100100001001" --verbose

[+] Consuming snake potion of minor decipherment


[+] Gathering critical information


[+] Potion effects are kicking in


================================================================================================


                picoCTF

                SEE PUBLIC RECORDS & BACKGROUND REPORT
                5000 Forbes Ave, Pittsburgh, PA 15213
                picoCTF{not_all_spaces_are_created_equal_c54f27cd05c2189f8147cc6f5deb2e56}


================================================================================================
```
