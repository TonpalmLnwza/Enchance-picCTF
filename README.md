# Quantum Scrambler 
**From-->** [picoCTF.org](https://play.picoctf.org/practice/challenge/479?category=3&page=1&search=)

This's a part of Reverse engineering from picoCTF.  
![image](<img width="940" height="787" alt="Image" src="https://github.com/user-attachments/assets/552eb60b-f4bb-4b78-9897-340c92f41f41" />)

**Download-->** [Quantum_Scrambler.py](https://challenge-files.picoctf.net/c_verbal_sleep/8ae2427fa0efa37bd8fcb87e6d5ff72813c9f594334ae1270fddb981dfdc756b/quantum_scrambler.py)

---

## First step

Take a look at the encrypted message.  
Does it look familiar to you?  
If not, take a little time to observe.

```
pico → xqkw  
CTF  → KBN
```

It follows with the curly bracket `{` and ends with the curly bracket `}` too. What a coincidence!

So, the next question is:  
**"Yo, how will I turn `pico` into `xqkw`, or how will I turn `CTF` into `KBN`?"**  
This is the fun part of the problem. Take a moment to observe.  
If you're tired of trying — take a look below.

---

It's actually so simple. If you convert each character to its alphabetical index:

```
x q k w = 24 17 11 23  
p i c o = 16  9  3 15
```

So, it's just a **-8 shift**.

What about the uppercase part? Is it the same?

```
K B N = 11 2 14  
C T F = 3 20 6
```

Sure it is! For example, if `'B'` got minus 8 (with wrap-around), it becomes `'T'`.

So the function looks like this:

```
f(x) = (x - 8 + 26) % 26

```

Where `f(x)` is the encrypted alphabetical order of `x`.

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
