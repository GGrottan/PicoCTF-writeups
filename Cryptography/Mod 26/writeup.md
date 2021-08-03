#### Name: Mod 26
#### Category: Crytpography
#### Description: Cryptography can be easy, do you know what ROT13 is? `cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_MAZyqFQj}`

This challenge is pretty straightforward, as we are given the ciphertext and the enciperhment method in the task description.

Since we know how the flag is enciphered, we could solve this task programatically, but we can also simply decipher the message
using a single-line command in the terminal. One option is using `echo` and `rot13` for deciphering. 

I chose to write the results to a file since I want to keep the result:

![](https://github.com/GGrottan/PicoCTF-writeups/blob/main/Cryptography/Mod%2026/img/decipher.png)

Inspecting the contents of the file, we get the plaintext of the flag 🚩:

![](blurredimage)


