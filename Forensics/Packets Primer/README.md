## Name: Packets Primer
#### Points: 100
#### Description: Download the packet capture file and use packet analysis software to find the flag.
#### Files: `network-dump.flag.pcap`

We receive a packet-capture file, and the obvious first step would be to open the file in Wireshark to analyse the contents.
When inspecting the packets, there are only a couple of TCP and ARP packets. Most interesting would be the TCP packets, and therefore we can 
right-click any of them, and choose `follow TCP stream`. Doing this displays the flag right away 🚩:


<details>
  <summary>Flag [Spoiler]</summary>
  
  ```
  picoCTF{p4ck37_5h4rk_7d32b1de}
  ```
  
  Alternatively, we could also use the `strings` command, since the capture file is so small:
  
  ```console
  
  ┌──(gagr㉿desktop)-[/pico/picoCTF2022/forensics/packets-primer]
  └─$ strings network-dump.flag.pcap
  k&Nar
  n#('
  k&Na
  k&Na`
  n#('
  k&Na;
  n#('
  p i c o C T F { p 4 c k 3 7 _ 5 h 4 r k _ 7 d 3 2 b 1 d e }
  k&Naa
  ep&Na(
  p&NaX
  p&Na28
  p&Na
  
  ```
  
</details>
