### Name: Plumbing
### File: None
### Description: Sometimes you need to handle process data outside of a file. Can you find a way to keep the output from this program and search for the flag? Connect to jupiter.challenges.picoctf.org 7480.
### Points: 200

The description gives us pretty good info, and I connected to the given address with netcat.
Additionally, I specified that the output should be saved to a file:

```console
gagr@ubntu:~/Documents/pico/General_skills/plumbing$ nc jupiter.challenges.picoctf.org 7480 > output.txt
```

Inspecting the contents of `output.txt`:

```console
gagr@ubntu:~/Documents/pico/General_skills/plumbing$ cat output.txt
Not a flag either
Again, I really don't think this is a flag
This is defintely not a flag
This is defintely not a flag
This is defintely not a flag
Not a flag either
I don't think this is a flag either
Not a flag either
Not a flag either
Not a flag either
I don't think this is a flag either
Not a flag either
Again, I really don't think this is a flag
Not a flag either
Not a flag either
I don't think this is a flag either
Not a flag either
Again, I really don't think this is a flag
Not a flag either
Not a flag either
Not a flag either
I don't think this is a flag either
Again, I really don't think this is a flag
Not a flag either
Not a flag either
Not a flag either
This is defintely not a flag
This is defintely not a flag
Not a flag either
Again, I really don't think this is a flag
This is defintely not a flag
I don't think this is a flag either

...

```

The file goes on for much longer, so I added a grep command to look for the beginning of a flag 🚩: 

```console
gagr@ubntu:~/Documents/pico/General_skills/plumbing$ cat output.txt | grep picoCTF{
picoCTF{digital_plumb3r_06e9d954}
```
