## Name: Enhance!
#### Points: 100
#### Description: Download this image file and find the flag.
#### File: `drawing.flag.svg`

We receive an svg-file with a drawing of a black circle with a hole in it. Inspecting the image itself doesn't reveal anything, however, inspecting the 
svg-markup, we can see that there is some text being drawn: 

```svg

<text
       xml:space="preserve"
       style="font-style:normal;font-weight:normal;font-size:0.00352781px;
       ...

```

The text is really small, which is why we cannot see it. Additionally, looking at the text, we can see the flag being written vertically.
We could change the svg code to display the flag, however, we need to change the font-size and x-position for each entry, which creates more work than just 
copying the entries.

Copying and putting all the text together creates the flag 🚩:

<details>
  <summary>Flag [SPOILER]</summary>
  
  ```
  
  Flag: picoCTF{3nh4nc3d_58bd3420}
  
  ```
  
</details>
