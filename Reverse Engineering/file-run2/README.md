## Name: file-run2
#### Points: 100
#### Description: Another program, but this time, it seems to want some input. What happens if you try to run it on the command line with input "Hello!"?
#### Files: `run`

This is essentially the same challenge as the previous `file-run` task, however, we need to pass the phrase "Hello!" when running the executable.
Adding this argument outputs the flag 🚩:

<details>
  <summary>Flag [SPOILER]</summary>
  
  ```console
  
  ┌──(gagr㉿DESKTOP-UU62MC8)-[/mnt/…/pico/picoCTF2022/reverse-engineering/file-run2]
  └─$ ./run Hello!
  The flag is: picoCTF{F1r57_4rgum3n7_981abfb5}
  
  ```
  
</details>
