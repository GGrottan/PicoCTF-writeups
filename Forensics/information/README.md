#### Name: information
#### Category: Forenics
#### Description: Files can always be changed in a secret way. Can you find the flag? cat.jpg
#### Points: 10

From the description and the provided `cat.jpg` file, it sounds like steganography, where a file or information is hidden inside
the provided image-file.

Firstly, I attempted to use the `strings` command, with and without `grep picoCTF{`, but it gave no results.
Using **Steghide** gave no results either, as I didn't have the correct passphrase. 

Looking at the first hint provided in the task, it says "Look at the details of the file".
Using **Exiftool** to view the metadata of the image, the _License_ entry looks a little odd:

![](exiftool_info)

Since it contains a mix of uppercase and lowercase letters, and numbers, it could be base64.

After grabbing the license entry, I printed the decoded result to a separate file, and upon viewing the contents of the file, the flag was revealed 🚩:

![](flag)










