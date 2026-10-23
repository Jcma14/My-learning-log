# Section 3 — General Skills

[← Back to Learning Log](../../README.md)

**Platform:** CyLab Security Academy / picoCTF  
**Module:** Beginner's Guide to the Challenge Library — Section 3  
**Date:** June 2026  
**Skills demonstrated:** `strings`, `grep`, `unzip`, tab completion, web source inspection, robots.txt

#### Overview

The core toolkit for CTF survival: extracting text from binary files, filtering large outputs, tab completion for deep directory trees, inspecting web page source code, and exploiting `robots.txt`. These techniques appear in almost every CTF category.

#### Key Concepts

**Challenge 1 — Wave a Flag / Challenge 4 — Strings It:** Extract readable text from a binary file and filter for the flag.

```bash
strings <file> | grep -i pico
```

This pattern is so common it should be automatic — run it on any unknown file before anything else.

**Challenge 2 — Tab, Tab, Attack:** Navigate a deeply nested directory tree using tab completion. Instead of typing full folder names, type the first few letters and press **Tab** — the terminal fills in the rest.

```bash
unzip Addadshashanammu.zip
strings Add<TAB>/Alm<TAB>/Ash<TAB>/...  # tab-complete each folder
```

**Challenge 3 — Insp3ct0r:** The flag was split across three files of a web page — the HTML, CSS, and JS — hidden in comments.

- HTML comment: `<!-- hidden here -->`
- CSS/JS comment: `/* hidden here */`
- Steps: View Page Source → find part 1 → open linked `.css` and `.js` files → find parts 2 and 3

**Challenge 5 — First Grep:** Search a large plain text file for the flag without reading every line.

```bash
grep -i pico <file>
```

**Challenge 6 — Where are the Robots?:** `robots.txt` tells search engine crawlers which pages not to index — but those "hidden" URLs are visible to anyone. Always check it on web challenges.

```
https://challenge-url/robots.txt
→ Disallow: /8028f.html
https://challenge-url/8028f.html  → flag is here
```

#### Takeaways

- `strings <file> | grep -i pico` is the first command to run on any unknown file in a CTF.
- Tab completion saves enormous time and prevents typos — use it constantly.
- Always check `/robots.txt` on web challenges. The `Disallow` entries are essentially a list of interesting pages the site owner wanted to hide from Google — but not from you.
- View Page Source and check all linked CSS and JS files; flags and sensitive data are often hidden in comments.
