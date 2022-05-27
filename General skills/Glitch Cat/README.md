## Name: Glitch Cat
#### Points: 100
#### Description: Our flag printing service has started glitching! $ nc saturn.picoctf.net 52026

When executing the provided netcat connection, the following data is displayed:

```console

┌──(gagr㉿DESKTOP-UU62MC8)-[/mnt/…/kali/pico/picomini2022/glitch_cat]
└─$ nc saturn.picoctf.net 52026
'picoCTF{gl17ch_m3_n07_' + chr(0x62) + chr(0x65) + chr(0x63) + chr(0x66) + chr(0x33) + chr(0x38) + chr(0x36) + chr(0x31) + '}'
```

We receive the first part of the flag, but the rest is seemingly encrypted in hexadecimal. We can see that each hex-value is wrapped in a `chr`-function,
which is a built in Python function used to convert a number to its ascii-representation. 

In order to solve this task, we can simply wrap the entire output in a print statement in Python 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```python

  >>> print('picoCTF{gl17ch_m3_n07_' + chr(0x62) + chr(0x65) + chr(0x63) + chr(0x66) + chr(0x33) + chr(0x38) + chr(0x36) + chr(0x31) + '}')
      picoCTF{gl17ch_m3_n07_becf3861}
  
  ```
</details>


