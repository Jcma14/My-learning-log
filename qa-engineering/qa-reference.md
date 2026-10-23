# 🛠️ QA Engineering Reference

[← Back to Learning Log](../README.md)

**Source:** TripleTen QA Engineering Bootcamp  
**Topics:** Linux CLI, Python basics, Pytest, API testing

This section documents the technical foundations from QA engineering training — Linux command line, Python, automated testing with Pytest, and HTTP/API testing. These skills directly transfer to cybersecurity work.

---

### Linux Command Line

| Command | Description | Example |
|---|---|---|
| `pwd` | Print current working directory | `pwd` |
| `ls` | List files and folders | `ls -l` |
| `cd` | Change directory | `cd Documents` |
| `mkdir` | Create a new directory | `mkdir new_folder` |
| `rm` | Remove files | `rm file.txt` |
| `rm -r` | Remove directory and contents recursively | `rm -r folder_name` |
| `cp` | Copy files | `cp file.txt /home/user/Desktop` |
| `mv` | Move or rename files | `mv file.txt newfile.txt` |
| `touch` | Create a new empty file | `touch notes.txt` |
| `cat` | Display file contents | `cat file.txt` |
| `grep` | Search for text inside files | `grep "error" logfile.txt` |
| `find` | Search for files | `find /home -name notes.txt` |
| `sudo` | Execute as superuser | `sudo apt update` |
| `chmod` | Change file permissions | `chmod 755 script.sh` |
| `chown` | Change file ownership | `sudo chown user:group file.txt` |

💡 Use `--help` after any command (e.g. `ls --help`) to see available options.

---

### Python Basics

**Variables and Types**
```python
name = "Camilo"       # string
age = 30              # integer
height = 1.80         # float
is_active = True      # boolean
```

**Functions**
```python
def greet(name):
    return f"Hello, {name}!"

print(greet("Camilo"))  # Hello, Camilo!
```

**Lists and Loops**
```python
tools = ["nmap", "wireshark", "burpsuite"]

for tool in tools:
    print(tool)
```

**Conditionals**
```python
port = 80

if port == 80:
    print("HTTP")
elif port == 443:
    print("HTTPS")
else:
    print("Unknown service")
```

---

### Pytest — Automated Testing

Pytest is a testing framework for Python. A test is any function whose name starts with `test_`.

```python
def test_sum():
    assert 2 + 3 == 5

def test_deposit_positive():
    deposit = get_user_deposit()
    assert deposit > 0
```

**Running tests:**
```bash
pytest                   # run all tests in project
pytest calc_test.py      # run tests in a specific file
```

**Three rules for clean tests:**
- ✅ One test → one assertion
- ✅ Tests use independent data — no shared state
- ✅ Each test can run alone, in any order

---

### API Testing with Requests + Pytest

```python
import requests
import configuration

def get_docs():
    return requests.get(configuration.URL_SERVICE + configuration.DOC_PATH)

response = get_docs()
print(response.status_code)  # 200 = OK
```

**Common response attributes:**

| Attribute | Purpose |
|---|---|
| `status_code` | HTTP response code (200, 404, 500...) |
| `text` | Response body as a string |
| `json()` | Response body parsed as a dictionary |
| `headers` | Response headers |
| `ok` | `True` if status code is 2xx |

---

### Python Cheat Sheet

A Python quick reference made by Jack Rhysider (host of [Darknet Diaries](https://darknetdiaries.com/)).

[📘 Python Cheat Sheet — on Darknet Diaries](https://darknetdiaries.com/python)
