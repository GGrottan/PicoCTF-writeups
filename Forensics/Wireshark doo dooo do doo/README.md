### Name: Wireshark doo dooo do doo
### File: shark1.pcagng
### Desc: Can you find the flag?
### Points: 50

Since the given file has a `pcapng` extension, we open it in wireshark. 
The first thing I usually do with these kinds of tasks is to follow the available streams, and in this case, we can follow the TCP stream.

Following the TCP stream, a partiular text stood out: `Gur synt vf cvpbPGS{c33xno00_1_f33_h_qrnqorrs}`

Considering it has the same textual properties as a flag, it looks like a simple shift cipher.
Using [Cryptii](https://cryptii.com/pipes/caesar-cipher), it turns out that it ideed was a classical shift cipher.

Shifting the enciphered text by 13 places, we get the following message:

`The flag is picoCTF{p33kab00_1_s33_u_deadbeef}`
