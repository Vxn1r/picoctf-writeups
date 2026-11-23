# Undo

**Category:** General Skills  
**Difficulty:** Easy  
**Author:** Yahaya Meddy  
**Event:** picoCTF 2026

---

## Challenge

> Can you reverse a series of Linux text transformations to recover the original flag?

Pretty straightforward description. You connect to an instance and it starts throwing transformed versions of the flag at you, one layer at a time. Your job is to undo each transformation using normal Linux commands.

---

## Solution

First, launch the instance and connect:

```bash
nc <host> <port>
```

It greets you and shows something like:

```
=== Welcome to the Text Transformations Challenge! ===
Your goal: step by step, recover the original flag.
At each step, you'll see the transformed flag and a hint.
Enter the correct Linux command to reverse the last transformation.
```

Then it starts dropping the steps.

### Step 1 — Base64

Current flag looks like a long string of letters and numbers.  
Hint says: **Base64 encoded the string.**

Easy one. Just decode it:

```bash
base64 -d
```

It accepts the command and moves to the next step. Nice.

### Step 2 — Reverse the text

Now the string is backwards.  
Hint: **Reversed the text.**

There's a simple command for this:

```bash
rev
```

Worked. Moving on.

### Step 3 — Underscores vs Dashes

Hint: **Replaced underscores with dashes.**

So they took `_` and turned them into `-`. To undo it we just swap them back:

```bash
tr '-' '_'
```

`tr` is super useful for this kind of character translation.

### Step 4 — Curly braces vs Parentheses

Hint: **Replaced curly braces with parentheses.**

Same idea:

```bash
tr '()' '{}'
```

Now it actually starts looking like a real flag (almost).

### Step 5 — ROT13

Last one. Hint: **Applied ROT13 to letters.**

ROT13 just shifts every letter 13 places in the alphabet. The cool part is that applying it twice brings you back to the original, so the same command both encrypts and decrypts:

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

And boom — flag drops.

---

## Flag

```
picoCTF{Revers1ng_t3xt_Tr4nsf0rm@t10ns_xxxxxxxx}
```

*(Replace with your actual flag — every instance has a slightly different ending)*

---

## What I learned

- `base64 -d` for decoding
- `rev` to reverse strings
- `tr` is really powerful for character replacement / translation
- ROT13 is its own inverse (same command works both ways)

Pretty good intro challenge. Forces you to actually use the tools instead of just reading about them.

---

*Solved on picoCTF 2026*
