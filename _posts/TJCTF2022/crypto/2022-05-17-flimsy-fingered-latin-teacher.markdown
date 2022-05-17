---
title:  "TJCTF 2022 -- crypto/flimsy-fingered-latin-teacher"
date:   2022-05-17 02:00:00 -0400
categories: TJCTF2022 crypto
tags: TJCTF2022 crypto
# classes: wide
---
Looks like gibberish...

# Prompt
![Challenge Prompt][img-prompt]

# Solution
So the given text `ykvyg}pp[djp,rtpelru[pdoyopm|` looks like gibberish, but the challenge name and prompt give us a hint; it might be a keyboard layout related problem.

## Manually
We know that the flags have the format of `tjctf{some_message}`, starting with `tjctf{` and ending with `}`. The given text ends with a `|`. Looking at the a 'qwerty' keyboard layout, that's one key to the right of the `}` key. Hmm...

We can get the flag by shifting every character one key to the right on the keyboard.

## Automatically
Submit the provided text into a keyboard shift cipher solver on [dcode.fr][dcode]. This gives us all the possible results when shifting on the keyboard with different keyboard layouts:

![DCODE.fr output][img-dcode-output]

## Flag
The flag is: `tjctf{oopshomerowkeyposition}`

[img-prompt]: /assets/img/tjctf2022/flimsy-fingered-latin-teacher/0-prompt.png
[dcode]: https://www.dcode.fr/keyboard-shift-cipher
[img-dcode-output]: /assets/img/tjctf2022/flimsy-fingered-latin-teacher/1-dcode-output.png
