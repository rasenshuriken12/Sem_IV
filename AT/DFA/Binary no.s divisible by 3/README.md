# DFA for accepting binary numbers divisible by 3.

Trick: Assume we have 101 = value = 5.

- If we get next bit as "1", then, `new value = value * 2 + 1` = 5 * 2 + 1 = 11
- If we get next bit as "O", then, `new value = value * 2 + 0` = 5 * 2 + 0 = 10

So, from the start state q, we go to state Rem_0 (Remainder 0), if we see a 0, and, go to state Rem_1 (Remainder 1), if we
see a 1.

| value | `Rem_0`| `Rem_`1` | `Rem_2 `|
|--|--|--|--|
| *0* | `0` * 2 + *0* = `0` | `1` * 2 + *0* = `2` | `2` * 2 + *0* = `4`/ 3 = `1` |
| *1* | `0` * 2 + *1* = `1` | `1` * 2 + *1* = `3` / 3 = `0` | `2` * 2 + *1* = `5`/ 3 = `2` | 
