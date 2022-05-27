## Name: morse-code
#### Points: 100
#### Description: Morse code is well known. Can you decrypt this? Download the file here. Wrap your answer with picoCTF{}, put underscores in place of pauses, and use all lowercase. 
#### Files: `morse_char.wav`

We receive an audio file for this challenge. Opening the file, it is pretty clear that this is morse code, as the name would suggest.
The obvious solution to this challenge would be to listen and decode the message manually. Although it might be useful and maybe even *cool* to learn
morse code, I would rather prefer not to spend time listening to this audio file over and over until I manage to find the right pieces. 

Since we are dealing with dots and dashes in audio format, they should be pretty distinct when viewed in an audio editor. 
I uploaded the file on [Audiomass](https://audiomass.co/), and sure enough, there is a clear difference between the dots and dashes. Luckily, the file isn't 
long, and we can easily take note of the dots and dashes:

```morse

.-- .... ....- --... .... ....- --... .... ----. ----- -.. .-- ..--- ----- ..- ----. .... --...

```

Using [Cyberchef](https://gchq.github.io/CyberChef/), we can easily convert the morse code to readable text. 

We also need to convert the output to lowercase, as well as adding `_` where they are needed. This is also easy to figure out, 
as the pauses are clearly marked in the audio file. Doing all this gives us the correct flag 🚩: 

<details>
  <summary>Flag [Spoiler]</summary>
  
  ```
  
  picoCTF{wh47_h47h_90d_w20u9h7}
  
  ```
  
</details>

