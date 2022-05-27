## Name: substitution1
#### Points: 100
#### Description: A second message has come in the mail, and it seems almost identical to the first one. Maybe the same thing will work again. 
#### Files: `message.txt`

This is the second in the substitution-challenge series, and this time, we are only given the ciphertext, and not the key:

```console 

┌──(gagr㉿desktop)-[/picoCTF2022/cryptography/substitution1]
└─$ cat message.txt
WILh (hjpai lpa wrtikan ijn lbrc) ran r ietn pl wpgtkina hnwkazie wpgtnizizpu. 
Wpuinhiruih ran tanhnuiny szij r hni pl wjrbbnucnh sjzwj inhi ijnza wanrizdzie, 
inwjuzwrb (ruy cppcbzuc) hfzbbh, ruy tapobng-hpbdzuc rozbzie. 
Wjrbbnucnh khkrbbe wpdna r ukgona pl wrincpaznh, ruy sjnu hpbdny, 
nrwj eznbyh r hiazuc (wrbbny r lbrc) sjzwj zh hkogziiny ip ru pubzun hwpazuc hnadzwn. 
WILh ran r canri sre ip bnrau r szyn raare pl wpgtkina hnwkazie hfzbbh zu r hrln, bncrb nudzapugnui, 
ruy ran jphiny ruy tbreny oe grue hnwkazie capkth rapkuy ijn spaby lpa lku ruy tarwizwn. Lpa ijzh tapobng, 
ijn lbrc zh: tzwpWIL{LA3VK3UWE_4774WF5_4A3_W001_O810YY84}
```

Based on what we initially can see, is that we already know `picoCTF` is `tzwpWIL`. We can also theorize that `r` is `a` due to due it's use in the text.
We can also see a lot of `ryu`, or `ayu`, which we can assume is `and`. We can continue deciphering the text this way, but I think a substitution script for 
this challenge would be better.

In order for this to work, we should probably start off carefully, and only substitute the letters that we are certain of, such as `picoCTF`. This way, we 
won't start off with a false translation, and we won't have to backtrack later on. I created the following script as our baseline for deciphering:

```python
# Read the whole text. Set as lower to avoid replacing already substituted letters.
ciphertext = open('message.txt', 'r').read().lower()

# Map of ciphertext letters and their corresponding plaintext
replace_map = { 't' : 'P',
                'z' : 'I',
                'w' : 'C',
                'p' : 'O',
                'i' : 'T',
                'l' : 'F'
                }

# Replace each character according to the given map
for key, value in replace_map.items():
    ciphertext = ciphertext.replace(key, value)

# Print the final result
print(ciphertext)


```

Now it's just a matter of determening what each letter should be substituted with. In our first iteration, the output is the following:

```console

┌──(gagr㉿desktop)-[/pico/picoCTF2022/cryptography/substitution1]
└─$ python3 desubstitute.py
CTFh (hjOaT FOa CrPTkan Tjn Fbrc) ran r TePn OF COgPkTna hnCkaITe COgPnTITIOu. COuTnhTruTh ran PanhnuTny sITj r hnT OF Cjrbbnucnh sjICj TnhT TjnIa CanrTIdITe, TnCjuICrb (ruy cOOcbIuc) hfIbbh, ruy PaOobng-hObdIuc roIbITe. Cjrbbnucnh khkrbbe COdna r ukgona OF CrTncOaInh, ruy sjnu hObdny, nrCj eInbyh r hTaIuc (Crbbny r Fbrc) sjICj Ih hkogITTny TO ru OubIun hCOaIuc hnadICn. CTFh ran r canrT sre TO bnrau r sIyn raare OF COgPkTna hnCkaITe hfIbbh Iu r hrFn, bncrb nudIaOugnuT, ruy ran jOhTny ruy Pbreny oe grue hnCkaITe caOkPh raOkuy Tjn sOaby FOa Fku ruy ParCTICn. FOa TjIh PaOobng, Tjn Fbrc Ih: PICOCTF{Fa3vk3uCe_4774Cf5_4a3_C001_o810yy84}
```
What's great about this approach is that we can clearly see which characters have been substituted, and we can now determine more characters with greater 
confidense. For instance, `h` is most likely `s`, `a` is `r`, and `j` is `h` and so on. The more iterations, the faster the solving gets, and there is only one missing letter in the final iteration, which is easy guesswork, based on the already surrounding deciphered characters.

I managed to decipher the message with the following map:

```python
replace_map = { 't' : 'P',
                'z' : 'I',
                'w' : 'C',
                'p' : 'O',
                'i' : 'T',
                'l' : 'F',
                'r' : 'A',
                'u' : 'N',
                'y' : 'D',
                'h' : 'S',
                'a' : 'R',
                'n' : 'E',
                'c' : 'G',
                'j' : 'H',
                'k' : 'U',
                'b' : 'L',
                'e' : 'Y',
                'g' : 'M',
                's' : 'W',
                'd' : 'V',
                'f' : 'K',
                'o' : 'B',
                'v' : 'Q'
                }
```

Running the full script outputs the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```console
  ┌──(gagr㉿desktop)-[/pico/picoCTF2022/cryptography/substitution1]
  └─$ python3 desub.py
 
  CTFS (SHORT FOR CAPTURE THE FLAG) ARE A TYPE OF COMPUTER SECURITY COMPETITION. CONTESTANTS ARE PRESENTED WITH A SET OF CHALLENGES WHICH TEST THEIR
  CREATIVITY, TECHNICAL (AND GOOGLING) SKILLS, AND PROBLEM-SOLVING ABILITY. CHALLENGES USUALLY COVER A NUMBER OF CATEGORIES, AND WHEN SOLVED, EACH YIELDS A 
  STRING (CALLED A FLAG) WHICH IS SUBMITTED TO AN ONLINE SCORING SERVICE. CTFS ARE A GREAT WAY TO LEARN A WIDE ARRAY OF COMPUTER SECURITY SKILLS IN A SAFE, 
  LEGAL ENVIRONMENT, AND ARE HOSTED AND PLAYED BY MANY SECURITY GROUPS AROUND THE WORLD FOR FUN AND PRACTICE. FOR THIS PROBLEM, THE FLAG IS: 
  PICOCTF{FR3QU3NCY_4774CK5_4R3_C001_B810DD84}


  ```

</details>
