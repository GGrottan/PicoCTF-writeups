## Name: Safe Opener
#### Points: 100
#### Description: Can you open this safe? I forgot the key to my safe but this program is supposed to help me with retrieving the lost key. Can you help me unlock my safe? Put the password you recover into the picoCTF flag format like: picoCTF{password}
#### Files: `SafeOpener.java`

We receive a `.java` file, and since this is marked as a reverse engineering challenge, the first step would be to inspect the file contents.
We are also told that we don't actually have to run the program, as we only need the correct password. 

Inspecting the source code, the first notable piece of code is the following:

```java

public class SafeOpener {
    public static void main(String args[]) throws IOException {
        BufferedReader keyboard = new BufferedReader(new InputStreamReader(System.in));
        Base64.Encoder encoder = Base64.getEncoder();
        String encodedkey = "";
        String key = "";
        int i = 0;
        boolean isOpen;
        ...

```

It looks like it prepares some encoding in `base64`. Looking further down in the source code, we find:

```java
    public static boolean openSafe(String password) {
        String encodedkey = "cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz";

        if (password.equals(encodedkey)) {
            System.out.println("Sesame open");
            return true;
        }
        else {
            System.out.println("Password is incorrect\n");
            return false;
        }
    }
```

The password is hardcoded, and from the previous code-block, we know what kind of encipherment scheme is used.
Using a [script i created](https://github.com/GGrottan/Snake-potion-of-minor-decipherment) for these kinds of challenges, we can easily decode the password:




<details>
  <summary>Flag [Spoiler]</summary>

  ```console
  
  ┌──(gagr㉿desktop)-[/pico/picoCTF2022/reverse-engineering/safe-opener]
  └─$ python3 ~/tools/Snake-potion-of-minor-decipherment/spomd.py --base64 -m "cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz"

  ============================

  pl3as3_l3t_m3_1nt0_th3_saf3

  ============================

  ```

</details>
