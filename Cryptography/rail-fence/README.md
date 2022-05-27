## Name: rail-fence
#### Points: 100
#### Description: A type of transposition cipher is the rail fence cipher, which is described here. Here is one such cipher encrypted using the rail fence with 4 rails. Can you decrypt it? Download the message here. Put the decoded message in the picoCTF flag format, picoCTF{decoded_message}.
#### Files: `message.txt`

We receive a text file with its contents being enciphered with the rail fence cipher. Inspecting the contents:

```console 
┌──(gagr㉿desktop)-[/pico/picoCTF2022/cryptography/rail-fence-solved]
└─$ cat message.txt
Ta _7N6D54hlg:W3D_H3C31N__510ef sHR053F38N43D28 i33___N2
```

We could create a script for deciphering, but using [Cyberchef](https://gchq.github.io/CyberChef/) is much faster 😎.
As noted in the description, the cipher uses 4 rails, meaning that we have a key length of 4.

Setting `key = 4` and `offset = 0` produces the correct flag 🚩:


<details>
  <summary>Flag [Spoiler]</summary>
  
  ```
  
  picoCTF{WH3R3_D035_7H3_F3NC3_8361N_4ND_3ND_55228140}
  
  ```
  
</details>
