# Python Mini-Exercises: Emoticon Replacer & List Unpacking

A short guide to two beginner‑friendly Python tasks we worked on:

1) Converting specific words in a sentence to **emoticons**
2) Unpacking a list into **first**, **middle**, and **last** parts

---

## Requirements

- Python 3.8+ (standard library only)

---

## 1) Emoticon Replacer (`emotify`)

### Problem
Given a sentence, replace the words **smile**, **grin**, **sad**, and **mad** with their corresponding emoticons:

| word  | emoticon |
|------:|:--------|
| smile | `:)`    |
| grin  | `:D`    |
| sad   | `:((`   |
| mad   | `>:(`   |

The replacement should be **case‑insensitive** and should leave any other words unchanged.

### Simple Solution (no regex)

```python
EMOTICONS = {
    "smile": ":)",
    "grin":  ":D",
    "sad":   ":((",
    "mad":   ">:("
}

def emotify(text: str) -> str:
    # Split on whitespace, replace known words, keep others as-is, and re-join.
    return " ".join(EMOTICONS.get(word.lower(), word) for word in text.split())
```

#### How it works
- `text.split()` breaks the input into tokens on whitespace.
- For each `word`, `word.lower()` normalizes case so `"GRIN"` matches `"grin"`.
- `EMOTICONS.get(key, default)` returns the emoticon if found; otherwise it returns the original `word`.
- `" ".join(...)` glues the transformed tokens back together with single spaces.

#### Examples
```python
print(emotify("Make me smile"))     # Make me :)
print(emotify("I am MAD"))          # I am >:(
print(emotify("Please GRIN now"))   # Please :D now
```

#### Notes / Limitations
- Punctuation sticks to words (e.g., `mad!` stays `mad!`).  
  If you want punctuation handled, a small regex version can help:

```python
import re

EMOTICONS = {"smile": ":)", "grin": ":D", "sad": ":((", "mad": ">:("}

def emotify_regex(text: str) -> str:
    pattern = r"\b(smile|grin|sad|mad)\b"
    return re.sub(pattern, lambda m: EMOTICONS[m.group(0).lower()], text, flags=re.IGNORECASE)
```

---

## 2) Unpacking a List into `first`, `middle`, `last`

### Problem
Given a list like `[1, 2, 3, 4, 5, 6]`, assign:
- `first = 1`
- `middle = [2, 3, 4, 5]`
- `last = 6`

### Cleanest Solution (extended unpacking)

```python
lst = [1, 2, 3, 4, 5, 6]
first, *middle, last = lst

print("first:", first)    # 1
print("middle:", middle)  # [2, 3, 4, 5]
print("last:", last)      # 6
```

### Slice-Based Alternative

```python
lst = [1, 2, 3, 4, 5, 6]
first  = lst[0]
middle = lst[1:-1]
last   = lst[-1]
```

### Notes / Limitations
- Requires **at least 2 elements** for the unpacking form.  
  For `[10, 20]`, you get `first=10`, `middle=[]`, `last=20`.
- `middle` is a **list** in both versions.  
- Very large inputs duplicate the `middle` portion in memory (normal for slicing/unpacking).

---

## Quick Test Script

```python
# --- Emoticon tests ---
print(emotify("Make me smile"))       # Make me :)
print(emotify("I am MAD"))            # I am >:(
print(emotify("Please GRIN now"))     # Please :D now

# --- Unpacking tests ---
for seq in ([1], [1, 2], [1, 2, 3, 4, 5, 6]):
    if len(seq) >= 2:
        f, *m, l = seq
        print(f"seq={seq} -> first={f}, middle={m}, last={l}")
    else:
        print(f"seq={seq} -> too short to unpack into first/middle/last")
```

---

## Common Pitfalls We Fixed
- **Case mismatch in the dictionary keys** for `emotify` (use lowercase keys if you lowercase input).
- Off‑by‑one index when using the slice approach (`lst[0]` is the first element).

---

## License
Do whatever you like for learning and demos. 🙂
