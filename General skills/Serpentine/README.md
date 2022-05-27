## Name: Serpentine
#### Points: 100
#### Description: Find the flag in the Python script!
#### Files: `serpentine.py`

As the description tells us, we have to look into the code for the flag. Looking at the code, it is a program that prompts us for a choice when we execute it.
One of the choices will give us the flag, however, the function call for decoding the flag is missing. All we have to do is call the `print_flag` function on any of the
returns: 

```python

  while True:
    print('a) Print encouragement')
    print('b) Print flag')
    print('c) Quit\n')
    choice = input('What would you like to do? (a/b/c) ')

    if choice == 'a':
      print_encouragement()

    elif choice == 'b':
        print_flag()

    elif choice == 'c':
      sys.exit(0)

    else:
      print('\nI did not understand "' + choice + '", input only "a", "b" or "c"\n\n')

```

When the script is executed and we choose `b`, the flag is printed 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```
  What would you like to do? (a/b/c) b
  picoCTF{7h3_r04d_l355_7r4v3l3d_8e47d128}
  a) Print encouragement
  b) Print flag
  c) Quit
  ```
</details>
