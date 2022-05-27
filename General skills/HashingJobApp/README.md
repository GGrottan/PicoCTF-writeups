## Name: HashingJobApp
#### Points: 100
#### Description: If you want to hash with the best, beat this test! nc saturn.picoctf.net 65352

For this challenge, we receive a netcat connection. Executing the given connection prompts us to provide an MD5 hash of a short sentence:

```console

┌──(gagr㉿desktop)-[/picomini2022/hashingjobapp]
└─$ nc saturn.picoctf.net 65352
Please md5 hash the text between quotes, excluding the quotes: 'Muhammad Ali'
Answer:

```

This task is fairly trivial due to all of the convertion tools one can find online, however, it's always good practice to make our own converter.
Using a built-in Python library called `hashlib`, we can easily generate the MD5 hash for a given string:

```python

import hashlib

sentence = input('string to generate hash for: ')

res = hashlib.md5(sentence.encode('utf-8')).hexdigest()

print(res)

```

In order to beat the challenge, we have to generate three different hashes for three different strings - all of them MD5.
This is simply a matter of copy-pasting strings. After the third conversion, we are given the flag 🚩: 

<details>
  <summary>Flag [Spoiler]</summary>

  ```

  ┌──(gagr㉿DESKTOP-UU62MC8)-[/mnt/c/Users/User]
  └─$ nc saturn.picoctf.net 65352
  Please md5 hash the text between quotes, excluding the quotes: 'Muhammad Ali'
  Answer:
  81d1d45dbb19a90fdfae4f87865b136a
  81d1d45dbb19a90fdfae4f87865b136a
  Correct.
  Please md5 hash the text between quotes, excluding the quotes: 'killer whales'
  Answer:
  7339bcecf2fe2c278ef6e90586669fbc
  7339bcecf2fe2c278ef6e90586669fbc
  Correct.
  Please md5 hash the text between quotes, excluding the quotes: 'Cleopatra'
  Answer:
  f8cbe5a99675fff11ed4d83fc16e2071
  f8cbe5a99675fff11ed4d83fc16e2071
  Correct.
  picoCTF{4ppl1c4710n_r3c31v3d_674c1de2}
  
  ```
</details>
