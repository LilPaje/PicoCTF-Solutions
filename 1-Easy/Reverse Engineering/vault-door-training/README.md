### Author: Mark E. Haase
# Description

Your mission is to enter Dr. Evil's laboratory and retrieve the blueprints for his Doomsday Project. The laboratory is protected by a series of locked vault doors. Each door is controlled by a computer and requires a password to open. Unfortunately, our undercover agents have not been able to obtain the secret passwords for the vault doors, but one of our junior agents obtained the source code for each vault's computer! You will need to read the source code for each level to figure out what the password is for that vault door. As a warmup, we have created a replica vault in our training facility. The source code for the training vault is here: [VaulDoorTraining.java](https://jupiter.challenges.picoctf.org/static/1afdf83322ee9c0040f8e3a3c047e18b/VaultDoorTraining.java)

__Hints__: 
- 1: The password is revealed in the program's source code.


# Solution: 
First, we're gonna download the file, clicking on the link located at the end of description, with the name "VaultDoorTraining.java". Looking at it at first glance, it's just a basic java file. By runing it on your terminal or your IDE, we got the following: 

```console
$ java ./VaultDoorTraining.java
Enter vaul password:

```

By typing the password, following the "picoCTF" format we get: 
```console
Enter vaul password: picoCTF{my_password_1234}
Access denied!
```
Well, at least we tried. 
So, let's take a look at our java file: 

The first line is simple a import, probably for the I/O operations: 
```java 
[vault-door-training/VaultDoorTraining.java] 
import java.util.*;
```

After that, we got the class declaration and the _main_ function: 

```java 
[vault-door-training/VaultDoorTraining.java]
class VaultDoorTraining {
    public static void main(String args[]) {
        VaultDoorTraining vaultDoor = new VaultDoorTraining();
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
```
I don't know a lot about java code, but the line
```java
VaultDoorTraining vaultDoor = new VaultDoorTraining();
```
looks like it's starting an object of the class _VaultDoorTraining_. The lines: 
```java
Scanner scanner = new Scanner(System.in); 
System.out.print("Enter vault password: ");
String userInput = scanner.next();
```
are probably for the Input text, which is the the same Input we saw before when we executed the code. The _Stromg Input_ is probably where the CTF password if stored, since it has the same format (__picoCTF{}__).
Here it goes a simples explanation for what is going on inside the _input_ variable: 
> Imagine the user typed the key following the picoCTF format "picoCTF{key}"
> So the code creates a substring of the _userInput_ variable, which starts at the length of "picoCTF{" (which is 8) and ends on the last -1 character, excluding the last "}" character. 

After that we compare to a function we will talk later. Basically if the return of the function is _True_, this is our password. If return _False_, well... It's not. 

Let's look now at the end of the code where we have the _checkPasswor_ function, that is key for us to solve this CTF. Let's take a deeper look: 

```java
[vault-door-training/VaultDoorTraining.java]
    // The password is below. Is it safe to put the password in the source code?
    // What if somebody stole our source code? Then they would know what our
    // password is. Hmm... I will think of some ways to improve the security
    // on the other doors.
    //
    // -Minion #9567
    public boolean checkPassword(String password) {
        return password.equals("w4rm1ng_Up_w1tH_jAv4_eec0716b713");
    }
}
 ```

It has some comments... Funny enough! It says that the password is bellow: __w4rm1ng_Up_w1tH_jAv4_eec0716b713__
 So let's try it out: 
 ```console
$ java ./VaultDoorTraining.java
Enter vaul password: picoCTF{w4rm1ng_Up_w1tH_jAv4_eec0716b713}
Access granted.
```

Voilà, we cracked our very first _reverse engineering_ picoCTF!  