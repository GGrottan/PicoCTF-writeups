## Name: substitution0
#### Points: 100
#### Description: A message has come in but it seems to be all scrambled. Luckily it seems to have the key at the beginning. Can you crack this substitution cipher? 
#### Files: `message.txt`

We receive a text file containing the following information:

```console 

PAQZTNDSRYWFEXGJKCVLBUHOMI

Stctbjgx Ftdcpxz pcgvt, hrls p dcput pxz vlpltfm prc, pxz acgbdsl et lst attlft
ncge p dfpvv qpvt rx hsrqs rl hpv txqfgvtz. Rl hpv p atpblrnbf vqpcpaptbv, pxz, pl
lspl lret, bxwxghx lg xplbcpfrvlv—gn qgbcvt p dctpl jcrit rx p vqrtxlrnrq jgrxl
gn urth. Lstct htct lhg cgbxz afpqw vjglv xtpc gxt tolcterlm gn lst apqw, pxz p
fgxd gxt xtpc lst glstc. Lst vqpftv htct toqttzrxdfm spcz pxz dfgvvm, hrls pff lst
pjjtpcpxqt gn abcxrvstz dgfz. Lst htrdsl gn lst rxvtql hpv utcm ctepcwpaft, pxz,
lpwrxd pff lsrxdv rxlg qgxvrztcplrgx, R qgbfz spczfm afpet Ybjrltc ngc srv gjrxrgx
ctvjtqlrxd rl.

Lst nfpd rv: jrqgQLN{5BA5717B710X_3U0FB710X_PP1QQ2A7}

```

We receive a scrambled text, and it is pretty clear that the flag is at the very bottom, as we can see that any other characters apart from letters
still have their original form. This means that we already have parts of the flag ready. Furthermore, these kinds of ciphers are also weak to statistical analysis. 
This means that even though the letters are substituted, they still retain their original statistical form. 

For example, we can see that `p` is used a lot by itself in-between words, meaning that it's the enciphered form of the letter `a`. 
This theory is also strengthened by the fact that we are given a key at the top. This looks to be a permutation of the alphabet, and we can also see that 
the letter `P` is placed where `A` should be. Based on these observations, we can make use of the given key:

| Permutation | Alphabet  |
|------------|---|
| Ciphertext  | PAQZTNDSRYWFEXGJKCVLBUHOMI|
| Plaintext  |  ABCDEFGHIJKLMNOPQRSTUVWXYZ |

Also knowing that every flag starts with `picoCTF`, we can double check that our hypothesis is correct. Using the table above, we can see
that `j --> p, r --> i, q --> c, g --> o`! If we are really interested in what the enciphered text says, we could make a script that re-substitutes 
the letters for us, but the flag contains a lot of numbers and re-use of the same letters, and so it's just easier to decipher it manually 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```
  picoCTF{5UB5717U710N_3V0LU710N_AA1CC2B7}
  ```

</details>
