## Name: substitution2 
#### Points: 100
#### Description: It seems that another encrypted message has been intercepted. The encryptor seems to have learned their lesson though and now there isn't any punctuation! Can you still crack the cipher?
#### Files: `message.txt`

## 

This is the third substitution challenge, where in addition to the key being removed, the punctuation is also removed.
Inspecting `message.txt`:

```
┌──(gagr㉿desktop)-[/picoCTF2022/cryptography/substitution2]
└─$ cat message.txt
voxfxxtnuvuxjxfrycvoxfdxyyxuvraynuoxgonwousoccyscqmkvxfuxskfnvhscqmxvnvncpunpsykgnpwshaxfmrvfncvrpgkushaxfsoryyxpwxvoxuxscqmxvnvncpubcskumfnqrfnyhcpuhuvxqurgqnpnuvfrvncpbkpgrqxpvryudonsorfxjxfhkuxbkyrpgqrfexvrayxuenyyuocdxjxfdxaxynxjxvoxmfcmxfmkfmcuxcbronwousoccyscqmkvxfuxskfnvhscqmxvnvncpnupcvcpyhvcvxrsojrykrayxuenyyuakvryucvcwxvuvkgxpvunpvxfxuvxgnprpgxtsnvxgrackvscqmkvxfusnxpsxgxbxpunjxscqmxvnvncpurfxcbvxpyracfnckurbbrnfurpgscqxgcdpvcfkppnpwsoxseynuvurpgxtxskvnpwscpbnwusfnmvucbbxpuxcpvoxcvoxforpgnuoxrjnyhbcskuxgcpxtmycfrvncprpgnqmfcjnurvncprpgcbvxporuxyxqxpvucbmyrhdxaxynxjxrscqmxvnvncpvcksonpwcpvoxcbbxpunjxxyxqxpvucbscqmkvxfuxskfnvhnuvoxfxbcfxraxvvxfjxonsyxbcfvxsoxjrpwxynuqvcuvkgxpvunprqxfnsrponwousoccyubkfvoxfdxaxynxjxvorvrpkpgxfuvrpgnpwcbcbbxpunjxvxsopnzkxunuxuuxpvnrybcfqckpvnpwrpxbbxsvnjxgxbxpuxrpgvorvvoxvccyurpgscpbnwkfrvncpbcskuxpsckpvxfxgnpgxbxpunjxscqmxvnvncpugcxupcvyxrguvkgxpvuvcepcdvoxnfxpxqhruxbbxsvnjxyhruvxrsonpwvoxqvcrsvnjxyhvonpeynexrprvvrsexfmnscsvbnurpcbbxpunjxyhcfnxpvxgonwousoccyscqmkvxfuxskfnvhscqmxvnvncpvorvuxxeuvcwxpxfrvxnpvxfxuvnpscqmkvxfusnxpsxrqcpwonwousoccyxfuvxrsonpwvoxqxpckworackvscqmkvxfuxskfnvhvcmnzkxvoxnfskfncunvhqcvnjrvnpwvoxqvcxtmycfxcpvoxnfcdprpgxpraynpwvoxqvcaxvvxfgxbxpgvoxnfqrsonpxuvoxbyrwnumnscSVB{P6F4Q_4P41H515_15_73G10K5_B302B3A6}
```

We still have some statistical values, such as uppcase letters and curly braces for the flag. This means that just from looking at the ciphertext, we already
know that `mnscSVB` is `picoCTF`. Using the same script and approach as in the [previous challenge](https://github.com/GGrottan/picoCTF-2022-writeups/tree/main/Cryptography/substitution1), 
the following map can be used to decipher the message:

```python

replace_map = { 'm' : 'P',
                'n' : 'I',
                's' : 'C',
                'c' : 'O',
                'v' : 'T',
                'b' : 'F',
                'p' : 'N',
                'h' : 'Y',
                'x' : 'E',
                'q' : 'M',
                'u' : 'S',
                'o' : 'H',
                'y' : 'L',
                'r' : 'A',
                'w' : 'G',
                'k' : 'U',
                'f' : 'R',
                'g' : 'D',
                'a' : 'B'
                }

```

Using the same script with the provided map descrables just enough to get the correct flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```

    picoCTF{N6R4M_4N41Y515_15_73D10U5_F302F3B6}

  ```

</details>
