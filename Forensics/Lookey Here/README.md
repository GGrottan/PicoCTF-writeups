## Name: Lookey Here
#### Points: 100
#### Description: Attackers have hidden information in a very large mass of data in the past, maybe they are still doing it.
#### Files: `anthem.flag.txt`

We receive a text file with a lot of text. There might be many ways to find potential hidden information in such files, and one of these methods
is to use the `strings` and `grep` commands to look for specific text. I've previously made a 
[custom script](https://github.com/GGrottan/plainflag-finder) that looks for several flag-indicators in these types
of files. In short, this script automates the use of `strings` and `grep` based on a list of keywords associated with flags, and prints the results to the console.

Running the [plainflag-finder](https://github.com/GGrottan/plainflag-finder) script outputs the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>
  
  ```console
  
  ┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/lookey-here]
  └─$ python3 ~/tools/plainflag-finder/plainflag-finder.py -f anthem.flag.txt

  [+]      we think that the men of picoCTF{gr3p_15_@w3s0m3_429334b2}
  
  ```
  
</details>
