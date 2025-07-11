# Quantum Scrambler 
**From-->** [picoCTF.org](https://play.picoctf.org/practice/challenge/479?category=3&page=1&search=)

This's a part of Reverse engineering from picoCTF.  
<img width="940" height="787" alt="Image" src="https://github.com/user-attachments/assets/c27e0877-2886-4e79-854e-42aea1f3a2ac" />

---

## Problem description
This is a reverse engineering challenge which give access tp a program that running on a server and we have to connect to it. And when we connect to it using netcat it will output a list of hex values. Also we have a python sript of the program.

---

## Problem sloving
They give us a python file, and we will run on the webshell.picoctf.org by using "wget".
- wget is a command-line tool that makes it possible to download files from the internet directly to active directory.

<img width="1872" height="700" alt="Image" src="https://github.com/user-attachments/assets/c65d9408-6a9b-4083-8e50-31b018144e65" />

- We connet to the server using netcat, it will output a list of hex values like showing in the following screen shot.

<img width="1900" height="366" alt="Image" src="https://github.com/user-attachments/assets/23b91ff3-67bd-462a-bd2f-febd717417bc" />

What's the Hex values - [Video](https://youtu.be/mjb8R20eD1U?si=K-5TEBWYbRlZI0a6)

---
## Analyze the Python Script

```py
import sys

def exit():
  sys.exit(0)

def scramble(L):
  A = L
  i = 2
  while (i < len(A)):
    A[i-2] += A.pop(i-1)
    A[i-1].append(A[:i-2])
    i += 1
    
  return L

def get_flag():
  flag = open('flag.txt', 'r').read()
  flag = flag.strip()
  hex_flag = []
  for c in flag:
    hex_flag.append([str(hex(ord(c)))])

  return hex_flag

def main():
  flag = get_flag()
  cypher = scramble(flag)
  print(cypher)

if __name__ == '__main__':
  main()
```

In the providing python script, the has mainly two functions we have to look, which are “scramble” and “get_flag”.

First we look into the “scramble” funtion. In this function it scrambled the list that pass as argument by appending and removing list eliments and making it hard to understand.

```py
def scramble(L):
  A = L
  i = 2
  while (i < len(A)):
    A[i-2] += A.pop(i-1)
    A[i-1].append(A[:i-2])
    i += 1
    
  return L
```

In the “get_flag” function it read a file called “flag.txt” and then converting the read content to hex value and making a list of those values.

```py
def get_flag():
  flag = open('flag.txt', 'r').read()
  flag = flag.strip()
  hex_flag = []
  for c in flag:
    hex_flag.append([str(hex(ord(c)))])

  return hex_flag

```

Also, in the main function it will call this tow function and finally it prints the output of the “scramble” function.

---

## 🚫 Stop the Bruteforce Campaign

**Bruteforce is for monkeys only.**  
We are civilized humans — so it's kinda good if you don't solve this one with just paper or Notepad.

Here’s my decrypting code:

```cpp
#include "bits/stdc++.h"
using namespace std;

int main() {
    string s; //--> given string s as an encrypted string
    cin >> s;
    for (auto i : s) { //--> iterate over s as i (range-based for loop)
        char base; //--> base is the first order of i
        // we will use base to tell if i is uppercase or lowercase
        if (i >= 'a' && i <= 'z') base = 'a';     //--> lowercase
        else if (i >= 'A' && i <= 'Z') base = 'A'; //--> uppercase
        else {
            cout << char(i);
            continue;
            // we skip the curly bracket once, so I assumed there's no need to decrypt symbols
        }
        cout << char((i - base - 8 + 26) % 26 + base);
        // decrypt it as in the function above
    }
}
```

This code was written so simply.  
If you can't read it — go learn CP. It will help you a lot in programming.

If you don't get any part, feel free to ask me.  
📩 IG: [a_phkhn](https://www.instagram.com/a_phkhn)

---

**Have a nice day!**
