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

So, we can observed from this work flow, this program will read something from the “flag.txt” file, then convert it to hex value list and scrambled that list and finally print that scrambled hex value list
