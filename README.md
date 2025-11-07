<div align="center">

# 🌟 Personal Learning Log: My Tech Journey

<img src="assets/Neon Green and Black Tech Virtual Background.png" width="90%" alt="Tech banner"/>

### 🚀 Exploring QA Engineering • Cybersecurity • Networking

This repository documents my personal journey through **QA Engineering**, **Cybersecurity**, and **Networking**.  
It serves as a central place to store my **technical notes**, **code examples**, and **learning reflections** as I grow in the IT field.  
All notes come from **hands-on labs**, **formal training**, and **self-study** — continuously updated as I learn.  

---

### 🧠 Tech Stack & Tools

![QA](https://img.shields.io/badge/QA%20Engineering-blueviolet?style=for-the-badge&logo=testcafe&logoColor=white)
![Cybersecurity](https://img.shields.io/badge/Cybersecurity-darkred?style=for-the-badge&logo=kalilinux&logoColor=white)
![Networking](https://img.shields.io/badge/Networking-0A66C2?style=for-the-badge&logo=cisco&logoColor=white)
![Python](https://img.shields.io/badge/Python-14354C?style=for-the-badge&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Markdown](https://img.shields.io/badge/Markdown-000000?style=for-the-badge&logo=markdown&logoColor=white)

</div>

---

<!-- Start: Linux & Command Line Section -->

<details>
  <summary><h2>QA Engineering: Linux and Command Line Fundamentals</h2></summary>
   
### Directory Navigation

| Command | Meaning | Example |
|----------|----------|----------|
| `.` or no special character | Current directory | `cd ./logs` |
| `..` | Parent directory | `cd ..` |
| `/` | Root directory | `cd /` |
| `~` | Home directory | `cd ~` |

### Linux Command Line Basics
Here’s a list of the most commonly used and essential Linux commands for beginners.  
These help you navigate, manage files, and perform basic operations in the terminal.

| Command | Description | Example |
|----------|--------------|----------|
| `pwd` | Prints the current working directory (where you are) | `pwd` |
| `ls` | Lists files and folders in the current directory | `ls -l` |
| `cd` | Changes the current directory | `cd Documents` |
| `mkdir` | Creates a new directory (folder) | `mkdir new_folder` |
| `rmdir` | Deletes an **empty** directory | `rmdir old_folder` |
| `rm` | Removes files | `rm file.txt` |
| `rm -r` | Removes a directory and its contents recursively | `rm -r folder_name` |
| `cp` | Copies files or directories | `cp file.txt /home/user/Desktop` |
| `mv` | Moves or renames files or directories | `mv file.txt newfile.txt` |
| `touch` | Creates a new empty file or updates its timestamp | `touch notes.txt` |
| `cat` | Displays the contents of a file | `cat file.txt` |
| `echo` | Displays text or writes text to a file | `echo "Hello" > greeting.txt` |
| `man` | Displays the manual for a command (help guide) | `man ls` |
| `clear` | Clears the terminal screen | `clear` |
| `history` | Shows a list of recently used commands | `history` |
| `whoami` | Prints your current username | `whoami` |
| `uname -a` | Displays system and kernel information | `uname -a` |
| `df -h` | Shows disk space usage | `df -h` |
| `du -sh` | Displays the size of the current directory | `du -sh` |
| `top` | Shows running processes and system resource usage | `top` |
| `sudo` | Executes a command as a superuser (admin privileges) | `sudo apt update` |
| `apt update` | Updates the list of available packages | `sudo apt update` |
| `apt upgrade` | Installs available package upgrades | `sudo apt upgrade` |
| `exit` | Closes the terminal or SSH session | `exit` |

💡 **Tip:** Use `--help` after any command (for example, `ls --help`) to quickly see its available options and usage examples.

### Linux File Permissions & Ownership

Linux is a multi-user system where every file and directory has permissions that define **who can read, write, or execute** it.  
Understanding and managing permissions is essential for security and proper system administration.

| Command | Description | Example |
|----------|--------------|----------|
| `ls -l` | Lists files in long format, showing permissions and ownership | `ls -l` |
| `chmod` | Changes file or directory permissions (read, write, execute) | `chmod 755 script.sh` |
| `chown` | Changes the ownership of a file or directory | `sudo chown user:group file.txt` |
| `chgrp` | Changes the group ownership of a file or directory | `sudo chgrp developers project/` |
| `umask` | Sets default permission settings for new files | `umask 022` |

### File Search & Inspection Commands

| Command   | Description                                      | Example                               |
| --------- | ------------------------------------------------ | ------------------------------------- |
| `find`    | Search for files and directories                 | `find /home -name notes.txt`          |
| `grep`    | Search for text inside files                     | `grep "error" logfile.txt`            |
| `cat`     | Display file contents                            | `cat notes.txt`                       |
| `less`    | View file contents one screen at a time          | `less notes.txt`                      |
| `head`    | Show the first 10 lines of a file                | `head notes.txt`                      |
| `tail`    | Show the last 10 lines of a file                 | `tail notes.txt`                      |
| `tail -f` | Continuously monitor new lines (useful for logs) | `tail -f /var/log/syslog`             |
| `wc`      | Count lines, words, and characters               | `wc notes.txt`                        |
| `du`      | Show disk usage of files/directories             | `du -h /home/user`                    |
| `df`      | Show available disk space                        | `df -h`                               |
| `stat`    | Display detailed file information                | `stat notes.txt`                      |
| `sort`    | Sort lines of text                               | `sort names.txt`                      |
| `uniq`    | Remove duplicate lines (often used with `sort`)  | `sort names.txt \| uniq`              |
| `cut`     | Extract columns or fields from text              | `cut -d':' -f1 /etc/passwd`           |
| `awk`     | Advanced text processing and reporting           | `awk '{print $1}' data.txt`           |
| `sed`     | Stream editor — modify text in files             | `sed 's/error/warning/g' logfile.txt` |

### Creating Directories

You can create a new directory using the `mkdir` command:

```bash
$ mkdir folder_name
# creates a new directory called folder_name
```

### Removing Empty Directories
Use `rmdir` to delete empty directories:
```bash
$ rmdir Personal
# removes an empty folder named Personal
```
### Removing Files and Directories
To remove files:
```bash
$ rm index.html
# deletes the file index.html
```
To remove a directory (and its contents) recursively:
```bash
$ rm -r Desktop/Personal
# deletes the directory 'Personal' inside 'Desktop' and all its contents
```
⚠️ Important:
When using the command line, double-check your path before deleting files — deletions are permanent (they don’t go to Trash).

### Creating Files
Use the `touch` command to create one or more new files:
```bash
$ touch answer.txt
# creates a new file named answer.txt
```
You can create multiple files at once:
```bash
$ touch style.css main.js
# creates two files: style.css and main.js
```

### Displaying Text in Terminal
The `echo` command prints text in the terminal
```bash
$ echo "Who is Morty?"
Who is Morty?
# prints "Who is Morty?" to the terminal
```
You can also write text to a file:
```bash
$ echo "Who is Morty?" > secrets.txt
# creates a file 'secrets.txt' and writes the line inside it
```
To overwrite or add text to an existing file:
```bash
$ echo "Who is Morty?" > ~/logs/2020/1/secrets.txt
# writes the line to the specified file, overwriting its contents
```

### Redirecting File Output
You can redirect text from one file to another using `cat` and redirection operators.

```<``` → input redirection

```>``` → output redirection

Example:
```bash
$ cat a.txt
AAA
# displays content of a.txt

$ cat b.txt
BBB
# displays content of b.txt

$ cat a.txt > b.txt
# copies content of a.txt into b.txt, overwriting its content
```
After this, both files contain:
```bash
AAA
```

### Appending File Content with `cat` and `>>` Operator
In this example, the content of a.txt is copied to the end of b.txt.
The >> operator is used to append data instead of overwriting it.
```bash
$ cat a.txt >> b.txt
# the content of a.txt was copied to b.txt

$ cat b.txt
BBB
AAA
# a.txt content was appended to b.txt without overwriting existing data
```

### Copying Files and Directories with `cp`
The `cp` (copy-paste) command copies a directory or file. After the command, you must specify:
1. The name and path of the source file.
2. The name and path of the destination file.

If the file is copied from or to the current directory, you can omit the path.

Examples:
``` bash
$ cp brothers.html sisters.html
# copied the file brothers.html and named the new copy sisters.html
# both files are in the current folder

$ cp ../docs/brothers.html sisters.html
# specified the path to the source file brothers.html
# the copy is saved in the current folder as sisters.html

$ cp ../docs/brothers.html ../Documents/
# copied brothers.html to the Documents directory
```
To copy an entire directory, use the `-r` flag.

### Moving and Renaming Files and Directories with `mv`
The `mv` (move) command moves a folder or file and works similarly to the `cp` command: you specify the file name and the destination path. Always specify the path to the new location.

Examples:
```bash
$ mv card.txt ~/
# card.txt moved from the current directory to the home directory

$ mv card.txt /home/logs/2020/
# card.txt moved using an absolute path
```
The `mv` command can also rename a file. To do this, provide the new file name as the second argument.
```bash
$ mv my_app.ssh your_app.ssh
# renamed my_app.ssh to your_app.ssh

$ mv /home/logs/2020/card.txt /home/logs/2020/cards.txt
# moved card.txt and renamed it to cards.txt
# absolute path specified
```
If you are already in the directory where you want to move or copy a file, you can omit the source path and just specify the filename.

### Filtering File Content with `grep`
When you have many log files in a directory and only one of them contains an error, you can use the `grep` command (Global Regular Expression Print) to search for specific text or patterns across files.

'grep' is a file search utility that finds and displays lines matching a given expression.
If no file is specified, it searches across all files in the current directory.

Syntax:
```bash
$ grep [Flag(s)] PATTERN [Address]
```
- `PATTERN` → the text you’re looking for (one or more words).
   - Single word: `Frodo`
   - Multiple words: `"Frodo Baggins"` (use quotes)
   
- `[Address]` → the file or directory path.

- `Flag(s)]` → optional arguments that modify the search behavior (e.g., ignore case, recursive search).

### Common Flags for `grep`

| Flag | Meaning                                            | Example                                |
| ---- | -------------------------------------------------- | -------------------------------------- |
| `-R` | Search recursively in all files inside a directory | `grep -R DELETE ~/logs/2020/1`         |
| `-n` | Show the **line number** for each match            | `grep -n DELETE apache_2020-01-01.txt` |
| `-i` | Ignore **case sensitivity** (uppercase/lowercase)  | `grep -i delete apache_2020-01-01.txt` |
| `-B` | Show lines **before** the match                    | `grep -B 2 ERROR system.log`           |
| `-A` | Show lines **after** the match                     | `grep -A 3 ERROR system.log`           |
| `-C` | Show lines **before and after** the match          | `grep -C 1 ERROR system.log`           |
| `-c` | Show only the **count** of matching lines          | `grep -c DELETE system.log`            |

`-R` — Recursive Search:
The `-R` flag tells grep to search through all files and subdirectories.
```bash
$ grep -R DELETE ~/logs/2020/1
```
`-n` — Show Line Numbers:
The `-n` flag displays the line number where each match occurs.
```bash
$ grep -n DELETE apache_2020-01-01.txt
```
#### Example: Search for a String and Display Line Number
You are in your home directory. Go to `~/logs/2020/1`.

Find the line number that contains `"503 3312"` in the file `apache_2020-01-01.txt`.
```bash
$ grep -n "503 3312" apache_2020-01-01.txt
```
#### Output:
```bash
1583
```
✅ The string `"503 3312"` appears on line 1583.

#### Viewing Context Around Matches with grep

In some cases, it’s important to see not only the matching line but also the surrounding lines — for example, to understand what happened before an error in the logs.

To show adjacent lines, use the flags `-B`, `-A`, and `-C`.

| Flag | Meaning        | Description                                                      |
| ---- | -------------- | ---------------------------------------------------------------- |
| `-B` | before-context | Shows the number of lines **before** the matching line           |
| `-A` | after-context  | Shows the number of lines **after** the matching line            |
| `-C` | context        | Shows the number of lines **before and after** the matching line |

Example: 
```bash
$ grep -C 1 DELETE ~/logs/2020/1/apache_2020-01-01.txt
```
This command shows the line containing `DELETE` plus one line above and below it.

#### Counting Matches
Use the `-c` flag to return only the number of lines that match your search term:
Example:
```bash
$ grep -c DELETE ~/logs/2020/1/apache_2020-01-01.txt
```
#### Pattern Matching Symbols

| Symbol | Description                         | Example                                                             |
| ------ | ----------------------------------- | ------------------------------------------------------------------- |
| `[]`   | Match **one of several characters** | `grep -i N[ua]m1 Log1.txt` → matches *Num1*, *Nam1*, *num1*, *nam1* |
| `.`    | Matches **any single character**    | `grep "204 3.96" apache_2020-01-01.txt`                             |
| `^`    | Matches **the beginning** of a line | `grep "^Start" text.txt` → shows lines starting with “Start”        |
| `$`    | Matches **the end** of a line       | `grep "End$" text.txt` → shows lines ending with “End”              |
| `^$`   | Matches **empty lines**             | `grep ^$ text.txt` → finds blank lines                              |

#### Saving `grep` Output to a File
```bash
$ grep ERROR /test1/test2/test_Logs/Log1.txt > errors.txt
```
All lines containing the word “ERROR” are saved into errors.txt.

⚠️ If the file already exists, it will be overwritten.

#### Using Command Templates

Sometimes the term `DELETE` appears with different cases or abbreviations like `DEL`. You can handle this with:

- `-i` → ignores uppercase/lowercase

- `*` (asterisk) → matches any number of characters

Example:
```bash
$ grep -i DEL* ~/logs/2020/1/apache_2020-01-31.txt
```
#### Useful Command Line Tricks

| 🔹 **Command / Shortcut**    | 🧩 **Description**                                        | 💻 **Example / Notes**                                                  |
| ---------------------------- | --------------------------------------------------------- | ----------------------------------------------------------------------- |
| **↑ / ↓ (Arrow keys)**       | Browse through previous commands in your command history. | Press `↑` to recall the last command, `↑↑` twice for the one before it. |
| **Ctrl + A**                 | Move cursor to the **beginning** of the command line.     | Great for quickly editing the start of a long command.                  |
| **Ctrl + E**                 | Move cursor to the **end** of the command line.           | Jump to the end instantly.                                              |
| **Ctrl + U**                 | **Clear** the entire command line.                        | Deletes everything currently typed.                                     |
| **Ctrl + C**                 | **Interrupt** a running process.                          | Use when a command hangs or runs too long.                              |
| **`mv *.txt ~/Desktop`**     | Move all `.txt` files to the Desktop using wildcards.     | Moves every text file from current directory.                           |
| **`cp j*.txt ~`**            | Copy all files starting with “j” and ending in `.txt`.    | Copies `jodorowsky_film.txt`, `jodorowsky_comics.txt`, etc.             |
| **`history`**                | Show your command history.                                | Lists all commands used in current session.                             |
| **`history > commands.log`** | Save command history to a file.                           | Stores full history in `commands.log`.                                  |

</details>

<!-- End: Linux & Command Line Section -->

---

<!-- Start: QA Engineering: SQL as a Data Management Tool -->

<details>
  <summary><h2>QA Engineering: SQL as a Data Management Tool</h2></summary>

SQL (Structured Query Language) is a programming language designed to manage and manipulate data in relational databases.

### Key Concepts 

| Term                                  | Definition                                                                                                     |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Database**                          | A structured repository of information.                                                                        |
| **Entity**                            | A group of objects that share common characteristics.                                                          |
| **Relational Database**               | A type of database where entities are **tables** and objects are **rows**.                                     |
| **DBMS (Database Management System)** | Software that allows you to create, manage, and edit databases and tables.                                     |
| **Table**                             | A collection of rows and columns representing structured data.                                                 |
| **Field**                             | A column in a table that defines a specific attribute and has a name and data type.                            |
| **Record**                            | A row in a table containing data about one object.                                                             |
| **Cell**                              | The intersection of a row and column.                                                                          |
| **Primary Key**                       | A unique field (or group of fields) used to identify records. It must not contain duplicates or `NULL` values. |
| **Query**                             | A structured SQL command that specifies **what data to retrieve** and **how to process it**.                   |

### Comments in SQL
```bash
-- Single-line comment

/* Multi-line
   comment */
```

### Basic SELECT Queries

Select specific columns:
```bash
SELECT column_name_1, column_name_2, column_name_3
FROM table_name;
```
Select all columns:
```bash
SELECT *
FROM table_name;
```
Select with condition:
```bash
SELECT column_name_1, column_name_2
FROM table_name
WHERE condition;
```

### Filtering Data

Select rows where a value is between two values:
```bash
SELECT *
FROM table_name
WHERE field_1 BETWEEN value_1 AND value_2;
```
Select rows where values are in a specific list:
```bash
SELECT *
FROM table_name
WHERE column_name IN ('value_1', 'value_2', 'value_3');
```

### Aggregate Functions
```bash
SELECT 
COUNT(*) AS total_rows,
COUNT(column) AS non_null_rows,
COUNT(DISTINCT column) AS unique_values,
SUM(column) AS total_sum,
AVG(column) AS average_value,
MIN(column) AS min_value,
MAX(column) AS max_value
FROM table_name;
```

### Type Conversion
```bash
SELECT CAST(column_name AS data_type)
-- or
SELECT column_name :: data_type
```

### Grouping and Sorting
Group Data:
```bash
SELECT field_1, field_2, AGGREGATE_FUNCTION(field) AS result
FROM table_name
WHERE condition
GROUP BY field_1, field_2;
```
Order results:
```bash
SELECT field_1, field_2, AGGREGATE_FUNCTION(field) AS result
FROM table_name
WHERE condition
GROUP BY field_1, field_2
ORDER BY field_1 DESC, field_2 ASC
LIMIT n;
```

### Managing Data
Insert data:
```bash
INSERT INTO table_name (column_1, column_2, column_3)
VALUES (value_1, value_2, value_3);
```
Update data:
``` bash
UPDATE table_name
SET column_name = new_value
WHERE condition;
```
Delete data:
```bash
DELETE FROM table_name
WHERE condition;
```

### Summary Table

| Command                             | Description                                          |
| ----------------------------------- | ---------------------------------------------------- |
| `SELECT`                            | Retrieves data from a table.                         |
| `WHERE`                             | Filters data based on a condition.                   |
| `BETWEEN`                           | Selects data between two values.                     |
| `IN`                                | Filters data within a specific list.                 |
| `COUNT`, `SUM`, `AVG`, `MIN`, `MAX` | Aggregate functions to perform calculations.         |
| `GROUP BY`                          | Groups data by one or more fields.                   |
| `ORDER BY`                          | Sorts data ascending (`ASC`) or descending (`DESC`). |
| `INSERT INTO`                       | Adds new data into a table.                          |
| `UPDATE`                            | Modifies existing records.                           |
| `DELETE`                            | Removes records from a table.                        |
| `CAST`                              | Converts a value to a different data type.           |

#### Quick Tip
Use `LIMIT n` to restrict the number of rows returned, especially when testing queries on large datasets.

### Types of Relationships Between Tables
A foreign key (FK) is a column in one table that contains values from another table — it links the two.

| Relationship Type      | Description                                                    | Example                                                                               |
| ---------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **One-to-One (1:1)**   | Each row in one table is linked to exactly one row in another. | `Employee → Payroll_Info` (each employee has one payroll record).                     |
| **One-to-Many (1:N)**  | One row in a table corresponds to multiple rows in another.    | `Author → Books` (an author can write many books, but each book has one author).      |
| **Many-to-Many (N:N)** | Multiple rows in one table relate to multiple rows in another. | Requires a **junction table** combining both primary keys (e.g., `students_courses`). |

### ER Diagrams (Entity-Relationship)
ER diagrams visually represent how tables and their relationships are structured in a database.

Elements:

* Tables: Shown as rectangles divided into two parts:
  * Upper part: table name
  * Lower part: list of fields with keys indicated (`PK` = Primary Key, `FK` = Foreign Key)
* Relationships: Lines connecting tables, with symbols indicating cardinality (1:1, 1:N, N:N).

### Searching for Empty Values
Find records with missing or empty fields using `IS NULL` or `IS NOT NULL`.

```bash
-- Rows with empty values
SELECT *
FROM table_name
WHERE column_name IS NULL;

-- Rows with non-empty values
SELECT *
FROM table_name
WHERE column_name IS NOT NULL;
```

### Searching Data in Tables
#### Comparison Operators

| Operator | Meaning                  |
| -------- | ------------------------ |
| `=`      | Equal to                 |
| `!=`     | Not equal to             |
| `>`      | Greater than             |
| `<`      | Less than                |
| `>=`     | Greater than or equal to |
| `<=`     | Less than or equal to    |

#### Logical Operators

| Operator | Description                        |
| -------- | ---------------------------------- |
| `AND`    | Both conditions must be true       |
| `OR`     | One or both conditions can be true |
| `NOT`    | The condition must be false        |

#### Special Operators

| Operator  | Description                                        |
| --------- | -------------------------------------------------- |
| `BETWEEN` | Selects values within a specific range (inclusive) |
| `IN`      | Filters rows that match any value from a list      |
| `LIKE`    | Searches for a pattern using wildcards             |

### `INNER JOIN`
Returns only the rows that have matching values in both tables (intersection).
```bash
SELECT  
    table1.field_1,
    table1.field_2,
    table2.field_n
FROM table1
INNER JOIN table2 ON table2.field_1 = table1.field_2;
```
Key Points:
* Table order doesn’t affect the result.
* Includes only rows that meet the join condition.
* Be careful with duplicates in 1:N relationships.

### `LEFT JOIN` (Outer Join)
Selects all rows from the left table and the matching rows from the right table.

Non-matching rows from the right table are filled with `NULL`.
```bash
SELECT  
    table1.field_1,
    table1.field_2,
    table2.field_n
FROM table1
LEFT JOIN table2 ON table2.field_1 = table1.field_2;
```
Key Points:
* Keeps all rows from the left table.
* Missing matches from the right table show as NULL.
* Useful for finding records that exist in one table but not another.

### `RIGHT JOIN` (Outer Join)
Selects all rows from the right table and the matching rows from the left table.

Non-matching rows from the left table are filled with `NULL`.
```bash
SELECT  
    table1.field_1,
    table1.field_2,
    table2.field_n
FROM table1
RIGHT JOIN table2 ON table1.field_1 = table2.field_2;
```
Key Points:
* Keeps all rows from the right table.
* Works similarly to `LEFT JOIN`, just mirrored.
* Less common but useful in some scenarios.

### Joining Multiple Tables
You can join more than two tables by chaining multiple `JOIN` statements.
```bash
SELECT  
    table1.field_1,
    table2.field_2,
    table3.field_3
FROM table1
INNER JOIN table2 ON table1.id = table2.table1_id
INNER JOIN table3 ON table2.id = table3.table2_id;
```
#### Things to Remember:
* Each new JOIN builds on the previous one.
* You can mix different join types (INNER, LEFT, RIGHT).
* The order of joins can affect results.
* Always double-check each ON condition.

Example:
```bash
SELECT  
    customers.name,
    orders.order_date,
    products.product_name
FROM customers
INNER JOIN orders ON customers.id = orders.customer_id
INNER JOIN order_items ON orders.id = order_items.order_id
INNER JOIN products ON order_items.product_id = products.id;
```

#### Quick Summary

| Concept                   | Description                             |
| ------------------------- | --------------------------------------- |
| **Foreign Key**           | A column linking two tables.            |
| **ER Diagram**            | Visual map of tables and relationships. |
| **IS NULL / IS NOT NULL** | Filters empty or non-empty data.        |
| **Comparison Operators**  | `=`, `!=`, `>`, `<`, `>=`, `<=`         |
| **Logical Operators**     | `AND`, `OR`, `NOT`                      |
| **JOINs**                 | Combine data from related tables.       |
| **LEFT JOIN**             | Keep all records from the left table.   |
| **RIGHT JOIN**            | Keep all records from the right table.  |
| **Multiple JOINs**        | Combine three or more tables.           |

</details>


<!-- End: QA Engineering: SQL as a Data Management Tool -->

---

<!-- Start: Networking Section -->

<details>
  <summary><h2>QA Engineering: SSH And Networking</h2></summary>

### What is SSH?
SSH (Secure Shell) is a network protocol used to securely access another computer remotely.
It encrypts all communication between client and server.

### Main uses:

Remote access to servers or devices.
Executing commands securely.
Transferring files (`scp`, `sftp`).
Tunneling other network traffic.
### Connect to a server:
```bash
ssh user@server_ip
```
### SSH Keys:
* Private key stays on the client.
* Public key goes on the server.

They provide secure passwordless authentication.
### Is SSH Always Secure?
SSH is very secure when properly configured, but risks include:

* Weak passwords → use SSH keys instead.
* Outdated SSH versions → keep OpenSSH updated.
* Not verifying host fingerprints → risk of MITM attacks.
* Exposed port 22 → use a non-standard port or firewall.
* Stolen private keys → protect with passphrases.
  
| Setup                      | Security Level |
| -------------------------- | -------------- |
| Password login             | Medium         |
| Key-based authentication   | Strong         |
| Outdated/misconfigured SSH | Weak           |


### SSH Ports

* Default SSH port: TCP 22

#### Risks:
* Brute-force attacks → disable password logins.
* Port scanning → change default port or restrict via firewall.
* Outdated versions → keep updated.

#### Best practices:
* Use key-based authentication.
* Change default port.
* Limit IP access.
* Use tools like Fail2Ban or UFW.

### Ports on the Client Side
#### When connecting via SSH:
* The client does not open permanent ports.
* It uses a temporary ephemeral port for outbound connection (e.g., Client:54321 → Server:22).
* The port closes automatically when the session ends.

#### Exceptions:
Ports open locally only when port forwarding is used.

| Mode | Description        | Open Port                         |
| ---- | ------------------ | --------------------------------- |
| `-L` | Local forwarding   | Opens local port (localhost only) |
| `-R` | Remote forwarding  | Opens port on remote server       |
| `-D` | Dynamic forwarding | Opens local SOCKS proxy port      |

### Disconnecting from SSH
To disconnect cleanly:
``` bash
exit
```
If the session freezes:
```bash
~.
```
If the terminal is closed, the SSH session ends immediately.

**Best practice**: always use `exit`.
</details>

<!-- End: Networking Section -->
---

<!-- Start: Cybersecurity Section -->

<details>
  <summary><h2>Cybersecurity Basics</h2></summary>
   
### Overview
Core cybersecurity principles learned through TryHackMe and self-study:

* Understanding security principles, governance, and regulation.
* The Cyber Kill Chain model for analyzing attack stages.
* Fundamental defensive practices (updates, authentication, network segmentation).
</details>

<!-- End: Cybersecurity Section -->

---

<!-- Start: Python Section -->

<details>
  <summary><h2>Python</h2></summary>

## 🐍 Python Cheat Sheet (Courtesy of Jack Rhysider)

This Python cheat sheet was created by **Jack Rhysider**, the host of [**Darknet Diaries**](https://darknetdiaries.com/), and is shared here for **educational and personal learning purposes**.

> 📝 **Credit:** Original creator — [Jack Rhysider](https://darknetdiaries.com/)  
> 📄 **Source:** *Python Cheat Sheet (Darknet Diaries)*  
> ⚖️ **Note:** All rights belong to the original author. This resource is included here solely for study and reference.

[📘 View the Python Cheat Sheet (by Jack Rhysider)](./assets/Python-CheatSheet.pdf)

## 🐍 Python for Test Automation — Introduction to Python

This section covers the fundamentals of Python. 
It provides a beginner-friendly overview to help build the foundation needed for future test automation.

### What is Python?

Python is a high-level, interpreted programming language that is easy to read and write.  
It is widely used for automation, software testing, data analysis, web development, and more.

#### Why Python is used in Test Automation:
- Simple and readable syntax (easy for beginners)
- Large ecosystem of libraries and frameworks for testing (e.g., `pytest`, `unittest`)
- Cross-platform: works on Windows, macOS, Linux

### Running Python Code

You can run Python in different ways:

| Method | Description |
|--------|----------------|
| Python Interpreter | Run Python directly in the terminal or command prompt |
| Python Script File `.py` | Write code in a file and execute it |
| IDE (e.g., PyCharm, VS Code) | Allows writing, running, and debugging Python projects |

#### Example (run in terminal):
```python
python3 hello.py
```
## Python Syntax Basics
### Indentation
Indentation is mandatory in Python. It defines code blocks – not {} brackets like other languages.
```python
if 5 > 3:
    print("Python uses indentation!")
```
### Case Sensitivity
`Variable`, `variable`, and `VARIABLE` are all different.

### Comments
Used to explain code and make it easier to understand.
```python
# This is a single-line comment
```
#### Multi-line comment:
```python
"""
This is a multi-line
comment in Python
"""
```

## Variables in Python
A variable stores data that can change during program execution.
You do not need to declare the type — Python detects it automatically.
```python
name = "Camilo"
age = 25
```
Rules for variable names:
* Must start with a letter or _
* Cannot start with a number
* Cannot contain spaces or symbols like @, $, %
* Case-sensitive

## Basic Data Types

A **data type** defines the kind of value a variable holds.  
Python automatically detects the type of data assigned to a variable — this is called **dynamic typing**.

| Data Type         | Example         | Description           |
| ----------------- | --------------- | --------------------- |
| `int` (integer)   | `10`            | Whole numbers         |
| `float` (decimal) | `10.5`          | Numbers with decimals |
| `str` (string)    | `"Hello"`       | Text                  |
| `bool` (boolean)  | `True`, `False` | Logical values        |

### Example:
```python
age = 25          # int
price = 9.99      # float
name = "Camilo"   # string
is_testing = True # bool
```

## Input & Output
### Print Output
```python
print("Hello, Python!")
```
### User Input
```python
name = input("Enter your name: ")
print("Hello, " + name)
```
Note: input() always returns a string.

## Basic Operators

| Operator | Example  | Description                     |
| -------- | -------- | ------------------------------- |
| `+`      | `2 + 3`  | Addition                        |
| `-`      | `5 - 2`  | Subtraction                     |
| `*`      | `4 * 3`  | Multiplication                  |
| `/`      | `8 / 2`  | Division (always returns float) |
| `//`     | `8 // 2` | Floor division (no decimals)    |
| `%`      | `7 % 3`  | Modulo (remainder)              |
| `**`     | `2 ** 3` | Exponent (power)                |

## Numeric Data Types
Python includes two common number types:

| Type    | Description                 | Example                 |
| ------- | --------------------------- | ----------------------- |
| `int`   | Whole numbers (no decimals) | `10`, `-3`, `0`         |
| `float` | Numbers with decimals       | `10.5`, `0.75`, `-3.14` |

Operations apply differently:
```python
a = 5     # int
b = 2.5   # float

print(a + b)   # 7.5 (result is float)
```

## Strings


</details>

<!-- End: Python Section -->
---

<!-- Start: Future Sections -->

<details>
  <summary><h2>Future Sections</h2></summary>
   
These placeholders will be filled as I continue learning:

* Git & GitHub Workflow

* Networking Deep Dive

* Kali Linux Tools

* Advanced QA Techniques

* Cloud & Virtualization Basics
</details>



