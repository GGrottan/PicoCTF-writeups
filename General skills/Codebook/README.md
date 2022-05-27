## Name: Codebook
#### Points: 100
#### Description: Run the Python script code.py in the same directory as codebook.txt.
#### Files: `code.py`, `codebook.txt`

This is the first challenge of the competition, and was pretty straight forward to solve. Simply download both given files into the same directory, and run the python script.
The script uses a permutation given in the `.txt` to create the flag 🚩:

<details>
  <summary>Flag [Spoiler]</summary>

  ```
  ┌──(gagr㉿desktop)-[/picomini2022/codebook]
  └─$ python3 code.py
  picoCTF{c0d3b00k_455157_8100c7c1}
  ```
</details>

## A closer look on `code.py` 🕵️

Although we are given the flag straight away, I like to take a look at the files to see if I can understand what is happening.
Inspecting the `code.py`-file, this seems to be the main part of the file:

```python
def print_flag():
  try:
    codebook = open('codebook.txt', 'r').read()

    password = codebook[4] + codebook[14] + codebook[13] + codebook[14] +\
               codebook[23]+ codebook[25] + codebook[16] + codebook[0]  +\
               codebook[25]

    flag = str_xor(flag_enc, password)
    print(flag)
  except FileNotFoundError:
    print('Couldn\'t find codebook.txt. Did you download that file into the same directory as this script?')

```

Firstly, the script checks whether the `codebook.txt` file is present or not, and gives a warning if the file cannot be found.
If `codebook.txt` is present, open it for reading, and extract a set of indexes to be stored in a `password` variable.

Inspecting the text file:

```
azbycxdwevfugthsirjqkplomn
```
This looks like a permutation of the alphabet, and we can check the contents of the password variable by copying the `codebook` and `password` variables into
our own script, and print the `password`:

```console
┌──(gagr㉿desktop)-[/picomini2022/codebook]
└─$ python3 password.py
chthonian
```

The next step in the script is to call the `str_xor`-function, and store the results in a variable called `flag`. The flag is then printed, which is 
our initial result of running the script. Let's take a look at this function: 

```python
def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])
```

The function takes two arguments: `secret` and `key`. When this function is called, the `secret` is set as `flag_enc`, and the `key` is set as `password` (chthonian).
What is the value of `flag_enc`? Before our previous function is called, a variable named `flag_enc` is declared:

```python
flag_enc = chr(0x13) + chr(0x01) + chr(0x17) + chr(0x07) + chr(0x2c) + chr(0x3a) + chr(0x2f) + chr(0x1a) + chr(0x0d) + chr(0x53) + chr(0x0c) + chr(0x47) + chr(0x0a) + chr(0x5f) + chr(0x5e) + chr(0x02) + chr(0x3e) + chr(0x5a) + chr(0x56) + chr(0x5d) + chr(0x45) + chr(0x5d) + chr(0x58) + chr(0x31) + chr(0x51) + chr(0x50) + chr(0x5e) + chr(0x53) + chr(0x0b) + chr(0x43) + chr(0x0b) + chr(0x5e) + chr(0x13)
```
This variable uses the built-in chr-function of Python, which returns the character represented by the given unicode (i.e. retrieves the string representation of a number). 
Next, a `new_key` and `i` and defined in preparation for a loop. The given loop will run as long as the length of the key is shorter than the secret. 

The `new_key` is appended a letter from the original key's index, and the counter variable (`i`) is also calculated based on the length of the original key.
The first couple of iteration will look something like this:

```python
key = 'chthonian'
new_key = 'chthonian'
i = 0

# Iteration 0
new_key = 'chthonian' + 'c'
i = (0 + 1) % 9 # (1 modulo 9 = 1)

# Iteration 1
new_key = 'chthonianc' + 'h'
i = (1 + 1) % 9 # (2 modulo 9 = 2)
```

This will continue as long as the length of the `new_key` is smaller than the length of the `secret`, where the length of the `new_key` is 9, and the length
of the `secret` is 33 (length of the `flag_enc`-variable) - thus running a total of 24 times.

To make this easier, I implemented the loop in my own script, and printed the `new_key`-variable for each iteration. This is the result:

```python
┌──(gagr㉿desktop)-[/picomini2022/codebook]
└─$ python3 loop.py
chthonianc
chthonianch
chthoniancht
chthonianchth
chthonianchtho
chthonianchthon
chthonianchthoni
chthonianchthonia
chthonianchthonian
chthonianchthonianc
chthonianchthonianch
chthonianchthoniancht
chthonianchthonianchth
chthonianchthonianchtho
chthonianchthonianchthon
chthonianchthonianchthoni
chthonianchthonianchthonia
chthonianchthonianchthonian
chthonianchthonianchthonianc
chthonianchthonianchthonianch
chthonianchthonianchthoniancht
chthonianchthonianchthonianchth
chthonianchthonianchthonianchtho
chthonianchthonianchthonianchthon
```
We can see that the `new_key` is simply repeated over and over until it reaches its desired length of 33 characters.

The last, and most important part of this function, is the return-statement:

```python
return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])
```

In order to understand what is happening here, it might be best to look at the end of the statement first.
It uses another built-in function called `zip`, which maps each item in two arrays together (first item of list 1 with first item of list 2) into a new list, as such:

List 1: `(1, 2, 3)`

List 2: `(4, 5, 6)`

Zipped: `((1,4), (2,5), (3,6))`

This is why there is a need for the loop; since the script is zipping the secret and the key, they need to be of the same length. 
At this point, the zipped results of the `new_key` and `secret` is the following:

`new_key` initial value: `chthonianchthonianchthonianchthon`

`secret` initial value: `\x13\x01\x17\x07,:/\x1a\rS\x0cG\n_^\x02>ZV]E]X1QP^S\x0bC\x0b^\x13`

zipped result: `(('\x13', 'c'), ('\x01', 'h'), ('\x17', 't'), ('\x07', 'h'), (',', 'o'), (':', 'n'), ('/', 'i'), ('\x1a', 'a'), ('\r', 'n'), ('S', 'c'), ('\x0c', 'h'), ('G', 't'), ('\n', 'h'), ('_', 'o'), ('^', 'n'), ('\x02', 'i'), ('>', 'a'), ('Z', 'n'), ('V', 'c'), (']', 'h'), ('E', 't'), (']', 'h'), ('X', 'o'), ('1', 'n'), ('Q', 'i'), ('P', 'a'), ('^', 'n'), ('S', 'c'), ('\x0b', 'h'), ('C', 't'), ('\x0b', 'h'), ('^', 'o'), ('\x13', 'n'))`

Looking at the next step, two new variables are created in order to loop through the zipped list: `secret_c` and `new_key_c`. 
These are simply placeholders for values that are going to get converted in the next step.

Now it is time for the real magic:

`"".join([chr(ord(secret_c) ^ ord(new_key_c))`

Both variables are converted to integers using the `ord` function, and again converted by a binary XOR. This is a simple operation that copies a bit (`1`) if 
it is present in one of the binary representations, and not in both. A simple example of this could be:

`a_variable = 5 (110101)`

`b_variable = 7 (110111)`

XOR:

`110101 XOR 110111 = 000010` (we can see that if a `1` is in the same spot (index) in both binary strings, it is replaced with a 0).

After being converted to an integer, and again XOR'ed, they are lastly converted to a character, and joined together (since we have a list of two values, as a result of the zip).
In order to better visualise these operations, we can manually convert the first tuple of values:

Initial value after zipping: `('x\13', 'c')`

Value after converted to integer: `(19, 99)`

Value for XOR: `(10011, 1100011)`

Result of XOR:

`0010011`

`1100011`

= `1110000` 

Finally, converting the output of the XOR to decimal gives us `112` - which is the letter `p` in ascii, and the first letter of the decrypted flag.

All in all, it was an easy first challenge, and it was interesting to see how the script actually worked! It was a clever encryption scheme, and could have been
a nice reverse-engineering challenge in itself.


