# Section 4 — Python

[← Back to Learning Log](../../README.md)

**Platform:** CyLab Security Academy / picoCTF  
**Module:** Beginner's Guide to the Challenge Library — Section 4  
**Date:** June 2026  
**Skills demonstrated:** Running Python scripts, reading source code, hex-to-ASCII conversion, MD5 hashing, dictionary attacks

#### Overview

This section builds Python skills through a series of password-cracking challenges. Each one adds a new layer of complexity — from a hardcoded plain-text password, to hex-encoded characters, to MD5 hashing. Reading the provided source code before doing anything else is the key skill throughout.

---

### Python Wrangling ✅

The challenge gave a Python script (`ende.py`), an encrypted flag file, and a password file. Reading the script revealed it needed a `-d` flag to decrypt and would prompt for a password.

```bash
cat pw.txt           # read the password
python3 ende.py -d flag.txt.en
# enter password when prompted
```

The script used **Fernet encryption** — a symmetric scheme where the same key encrypts and decrypts. Since the key was in `pw.txt`, decryption was straightforward once the script's usage was understood.

> Always `cat` or `nano` a script before running it — even a quick skim tells you what arguments it expects.

---

### PW Crack 1 ✅

The password was hardcoded directly in the source code as plain text inside an `if` condition.

```bash
nano level1.py      # find: if user_pw == "1e1a"
python3 level1.py -d level1.flag.txt.enc
# enter: 1e1a
```

Hardcoded credentials are a real-world vulnerability — penetration testers use `grep -r "password" .` to hunt for exactly this in source code and config files.

---

### PW Crack 2 ✅

The password was stored as four hex character codes: `chr(0x33) + chr(0x39) + chr(0x63) + chr(0x65)`. Each `chr()` call converts a number to its ASCII character.

```python
# In Python shell — decode each value:
chr(0x33)  # → '3'
chr(0x39)  # → '9'
chr(0x63)  # → 'c'
chr(0x65)  # → 'e'
# Password: 39ce
```

```bash
python3 level2.py -d level2.flag.txt.enc
# enter: 39ce
```

When you see `chr(0x__)` in Python source code, the script is building a string character by character from hex values — decode each one and join them.

---

### PW Crack 3 ✅
**Skills demonstrated:** MD5 hash comparison, dictionary attack, Python function modification, binary file inspection, reading source code

#### Overview

In this challenge, the password was no longer stored as plain text or a simple encoding — it was stored as an **MD5 hash** in a binary file (`level3.hash.bin`). Because MD5 is a one-way function, it can't be reversed to recover the original password. The solution was a **dictionary attack**: hash each candidate password from the provided list using the same MD5 function, and compare the result to the stored hash. When they match, the original password is found.

This mirrors a core real-world technique used by both attackers (cracking leaked password databases) and defenders (understanding why MD5 is no longer safe for password storage).

---

#### Understanding MD5 Hashing

A **hash** is a one-way transformation of data. You put a password in, you get a fixed-length string of bytes out. Three key properties:

- **One-way:** You cannot reverse a hash to recover the original input
- **Deterministic:** The same input always produces the exact same output
- **Fixed length:** MD5 always produces a 16-byte (128-bit) result, regardless of input size

```
Password → MD5 hash function → Hash output
"87ab"   → MD5             → E1 6D 55 A5 5D 80 DD DD 52 A8 3E AB EA 57 2B 7B
```

> 💡 **Why `bvi`?** The hash is stored in binary format (`.bin`), not plain text. Opening it with `cat` produces garbled output. `bvi` is a binary file viewer that shows the raw hex bytes — which is exactly what you see when you inspect `level3.hash.bin`: `E1 6D 55 A5 5D 80 DD DD 52 A8 3E AB EA 57 2B 7B`. That's the MD5 hash of the correct password stored on disk.

---

#### Reading the Script

The relevant parts of `level3.py` provided by the challenge:

```python
import hashlib

flag_enc = open('level3.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level3.hash.bin', 'rb').read()   # the stored hash

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()   # returns the MD5 hash as raw bytes

# 7 possible passwords — only 1 is correct
pos_pw_list = ["f09e", "4dcf", "87ab", "dba8", "752e", "3961", "f159"]
```

The script already has everything needed:
- `hash_pw()` — hashes any password string using MD5
- `correct_pw_hash` — the stored hash to match against
- `pos_pw_list` — the 7 candidates to test

---

#### Step by Step

**1. Inspect the hash file (optional — useful to understand what you're working with):**
```bash
bvi level3.hash.bin
# Shows raw bytes: E1 6D 55 A5 5D 80 DD DD 52 A8 3E AB EA 57 2B 7B
# :q to exit
```

**2. Modify the script to automate the search:**

The key technique is to **modify the original script** rather than writing a separate loop. Three changes are needed:
- Add `user_pw` as a **parameter** to `level_3_pw_check()` — so passwords can be fed in automatically instead of typed
- **Comment out** the `input()` line — prevents the program from stopping to ask for keyboard input on every iteration
- **Comment out** the error print — keeps output clean (no noise for wrong guesses)
- Replace the single `level_3_pw_check()` call with a **for loop** over `pos_pw_list`

```python
import hashlib

flag_enc = open('level3.flag.txt.enc', 'rb').read()
correct_pw_hash = open('level3.hash.bin', 'rb').read()

def hash_pw(pw_str):
    pw_bytes = bytearray()
    pw_bytes.extend(pw_str.encode())
    m = hashlib.md5()
    m.update(pw_bytes)
    return m.digest()

def level_3_pw_check(user_pw):          # ← added parameter
#   user_pw = input("Please enter correct password for flag: ")  # ← commented out
    user_pw_hash = hash_pw(user_pw)
    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
#   print("That password is incorrect")  # ← commented out

pos_pw_list = ["f09e", "4dcf", "87ab", "dba8", "752e", "3961", "f159"]

for pw in pos_pw_list:    # ← replaces the single call
    level_3_pw_check(pw)
```

Output: the flag prints directly when the matching password is found.

---

#### What's Happening Under the Hood

| Step | What the code does |
|---|---|
| `hash_pw(pw)` | Takes the candidate password, converts it to bytes, runs MD5, returns the hash |
| `hash_pw(pw) == correct_pw_hash` | Compares the freshly computed hash to the one stored in `.hash.bin` |
| Match found | The loop prints the password — you now know which one to enter |
| `str_xor()` | The challenge script uses the password to XOR-decrypt the flag from the `.enc` file |

---

#### Key Concepts Reference

| Concept | What it means |
|---|---|
| MD5 | One-way hash function — same input always gives same output, cannot be reversed |
| Dictionary attack | Hash every candidate and compare — when hashes match, password is found |
| Rainbow tables | Pre-computed hash lookup tables — why storing passwords as plain MD5 is insecure |
| `bvi` | Binary file viewer — needed since `cat` on a `.bin` file produces garbled output |
| `m.digest()` | Returns the MD5 result as raw bytes — must match the binary stored in `.hash.bin` |

---

#### Takeaways

- MD5 is a one-way function — you can't reverse it, but you can replicate it. If you can hash all the candidates and compare, you don't need to reverse anything.
- Dictionary attacks are the standard approach to cracking hashed passwords when a candidate list exists. In the real world, attackers use wordlists with millions of entries (like `rockyou.txt`) and tools like `hashcat` to crack MD5 hashes from breached databases almost instantly.
- Storing passwords as plain MD5 hashes is considered insecure precisely because of dictionary and rainbow table attacks. Modern systems use slow hashing algorithms like **bcrypt** or **Argon2** with salting to make this computationally infeasible.
- Reading source code is always the first step — the script told us exactly which hash function was used (`hashlib.md5`), where the stored hash lived (`level3.hash.bin`), and what the candidate passwords were (`pos_pw_list`).
- Python is case-sensitive: `correct_pw_hash` ≠ `Correct_pw_hash`. A `NameError` almost always means a capitalisation mistake.

---

---

### PW Crack 4 ✅
**Skills demonstrated:** MD5 dictionary attack, function parameterisation, automated password testing at scale

#### Overview

PW Crack 4 builds directly on PW Crack 3 — same MD5 hashing technique, same dictionary attack approach — but scales up from 7 candidates to 100. The challenge reinforces the same script-modification pattern while adding a practical detail: with 100 candidates, uncontrolled `input()` calls and noisy error output become real friction that must be handled cleanly.

#### Key Concepts

The same three modifications from PW Crack 3 apply here:

| Change | Why it matters |
|---|---|
| Add `user_pw` as a function parameter | Allows the loop to feed each candidate in automatically — no keyboard input needed |
| Comment out `input()` | Without this, Python stops and waits for typed input on every single iteration |
| Comment out error print | With 100 candidates, leaving it active produces 99 noise lines before the flag appears |

```python
def level_4_pw_check(user_pw):          # ← parameter instead of input()
#   user_pw = input("...")              # ← commented out
    user_pw_hash = hash_pw(user_pw)
    if( user_pw_hash == correct_pw_hash ):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), user_pw)
        print(decryption)
        return
#   print("That password is incorrect") # ← commented out

for pw in pos_pw_list:
    level_4_pw_check(pw)
```

#### Practical Application

The modified script iterates through all 100 candidates, hashing each one with MD5 and comparing the result to the hash stored in `level4.hash.bin`. When the hashes match, the function calls `str_xor()` internally and prints the decrypted flag — no manual step required.

#### Takeaways

- The pattern from PW Crack 3 is reusable: add parameter → comment out input → comment out error print → replace single call with loop. This generalises to any number of candidates.
- Passing values as **function parameters** instead of relying on `input()` is a foundational programming concept — it's what makes functions testable and automatable.
- Commenting out noise (error prints, prompts) is good practice when automating repetitive operations. Clean output is faster to read and less error-prone.
- At scale (millions of candidates), this same logic is what tools like `hashcat` implement — just much faster, with GPU acceleration and optimised data structures.

---

### PW Crack 5 ✅

**Skills demonstrated:** MD5 dictionary attack, reading passwords from an external file, Python file I/O, `strip()` for text cleaning

#### Overview

PW Crack 5 introduces a key real-world pattern: instead of hardcoding candidate passwords inside the script, they are read from an external file (`dictionary.txt`) — one password per line. The core MD5 hashing and comparison logic is identical to PW Crack 3 and 4. The new challenge is reading the file correctly and cleaning each line before hashing.

This mirrors how password cracking works in practice: tools like `hashcat` and `john` consume wordlist files (such as `rockyou.txt` with 14 million entries) and hash each candidate against a target hash. The only difference is scale.

#### Key Concepts

**Reading a file and building the candidate list:**

```python
with open('dictionary.txt', 'r') as f:
    pos_pw_list = [line.strip() for line in f]
```

| Component | What it does |
|---|---|
| `open('dictionary.txt', 'r')` | Opens the file in text read mode |
| `with ... as f` | Safely opens the file; Python closes it automatically when the block ends |
| `f` | A variable name for the file object — could be called anything |
| `for line in f` | Reads each line from the file one at a time |
| `line.strip()` | Removes the `\n` newline character from the end of each line |

**Why `strip()` is essential:**

Python includes the newline character when reading lines from a file:
```
"password123\n"
```
Hashing `"password123\n"` produces a completely different MD5 result from hashing `"password123"` — so without `strip()`, nothing would ever match, even the correct password.

**The complete modified function:**

```python
def level_5_pw_check():
    with open('dictionary.txt', 'r') as f:
        pos_pw_list = [line.strip() for line in f]

    for pw in pos_pw_list:
        if hash_pw(pw) == correct_pw_hash:
            print("Correct password found:", pw)
            decryption = str_xor(flag_enc.decode(), pw)
            print(decryption)
            return

    print("Password not found in dictionary")
```

Note: unlike PW Crack 3 and 4, no `input()` existed in the original script — the function body was replaced entirely rather than modified.

#### Comparison: PW Crack 4 vs PW Crack 5

| Feature | PW Crack 4 | PW Crack 5 |
|---|---|---|
| Password source | Hardcoded list in script | External `dictionary.txt` file |
| File I/O needed | No | Yes — `open()` + iteration |
| `strip()` needed | No | Yes — removes `\n` from each line |
| `input()` to comment out | Yes | No (not in original) |
| Hash comparison loop | Same | Same |

#### Takeaways

- Reading passwords from an external file is the standard approach in real-world tooling. Hardcoding candidates is fine for small CTF lists but doesn't scale — a file can hold millions of entries without changing any code.
- `strip()` is mandatory when processing text files line by line. Forgetting it is one of the most common bugs when working with file input in Python.
- The `with open(...) as f` pattern is the correct way to open files in Python — it guarantees the file is closed even if an error occurs inside the block.
- `f` is a conventional short name for a file object, the same way `i` is conventional for loop counters. Variable names in Python are arbitrary — what matters is consistency and readability.
