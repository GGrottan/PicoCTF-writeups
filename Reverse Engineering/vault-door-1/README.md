### Name: vault-door-1
### File: VaultDoor1.java
### Description: This vault uses some complicated arrays! I hope you can make sense of it, special agent. The source code for this vault is here: VaultDoor1.java
### Points: 100

Downloading and inspecting the source code of the given file:

```java
import java.util.*;

class VaultDoor1 {
    public static void main(String args[]) {
        VaultDoor1 vaultDoor = new VaultDoor1();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
	String userInput = scanner.next();
	String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
	if (vaultDoor.checkPassword(input)) {
	    System.out.println("Access granted.");
	} else {
	    System.out.println("Access denied!");
	}
    }

    // I came up with a more secure way to check the password without putting
    // the password itself in the source code. I think this is going to be
    // UNHACKABLE!! I hope Dr. Evil agrees...
    //
    // -Minion #8728
    public boolean checkPassword(String password) {
        return password.length() == 32 &&
               password.charAt(0)  == 'd' &&
               password.charAt(29) == '3' &&
               password.charAt(4)  == 'r' &&
               password.charAt(2)  == '5' &&
               password.charAt(23) == 'r' &&
               password.charAt(3)  == 'c' &&
               password.charAt(17) == '4' &&
               password.charAt(1)  == '3' &&
               password.charAt(7)  == 'b' &&
               password.charAt(10) == '_' &&
               password.charAt(5)  == '4' &&
               password.charAt(9)  == '3' &&
               password.charAt(11) == 't' &&
               password.charAt(15) == 'c' &&
               password.charAt(8)  == 'l' &&
               password.charAt(12) == 'H' &&
               password.charAt(20) == 'c' &&
               password.charAt(14) == '_' &&
               password.charAt(6)  == 'm' &&
               password.charAt(24) == '5' &&
               password.charAt(18) == 'r' &&
               password.charAt(13) == '3' &&
               password.charAt(19) == '4' &&
               password.charAt(21) == 'T' &&
               password.charAt(16) == 'H' &&
               password.charAt(27) == 'f' &&
               password.charAt(30) == 'b' &&
               password.charAt(25) == '_' &&
               password.charAt(22) == '3' &&
               password.charAt(28) == '6' &&
               password.charAt(26) == 'f' &&
               password.charAt(31) == '0';
    }
}

```

So the flag content is written in plaintext, but given a character at a time in a random position. 
Using an online java compiler might give us the flag right away, but I decided to use Python to solve the task.

A possible solution could be using a Python dictionary, and copy the characters and positions in the way that they are given in the file.
It is then possible to loop through the dictionary by the index of the characters, which will give us the flag content in the correct order:

```Python

dic = {0:'d', 29: '3', 4: 'r', 2: '5', 23: 'r', 3: 'c', 17: '4', 
1:'3', 7:'b', 10:'_', 5:'4', 9:'3', 11:'t', 15:'c', 8:'l', 12:'H',
20:'c', 14:'_', 6:'m', 24:'5', 18:'r', 13:'3', 19:'4', 21:'T', 
16:'H', 27:'f', 30:'b', 25:'_', 22:'3', 28:'6', 26:'f', 31:'0'}

flag_content = ""

for i in range(0, len(dic)):
    flag_content += dic[i]

print('picoCTF{' + flag_content + '}')
```
Running the python script above outputs the flag 🚩: 

```console
> python3 char_solver.py 
picoCTF{d35cr4mbl3_tH3_cH4r4cT3r5_ff63b0}
```
