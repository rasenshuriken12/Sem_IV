# DFA for accepting binary numbers divisible by 3.

Trick: Assume we have 101 = value = 5.

- If we get next bit as "1", then, `new value = value * 2 + 1` = 5 * 2 + 1 = 11
- If we get next bit as "0", then, `new value = value * 2 + 0` = 5 * 2 + 0 = 10

```
δ(r, d) = (2*r + d) mod 3.

Accepting states: {R_0}

Non-accepting states: {R_1, R_2, R_3}
```

So, from the start state q, we go to state R_0 (Remainder 0), if we see a 0, and, go to state R_1 (Remainder 1), if we
see a 1.

| Value | `R_0`| `R_1` | `R_2 `|
|--|--|--|--|
| *0* | `0` * 2 + *0* = `0` | `1` * 2 + *0* = `2` | `2` * 2 + *0* = `4`/ 3 = `1` |
| *1* | `0` * 2 + *1* = `1` | `1` * 2 + *1* = `3` / 3 = `0` | `2` * 2 + *1* = `5`/ 3 = `2` | 

**Transition table (simplified):**

| Value | R_0 | R_1 | R_2 |
|-------|-------|-------|-------|
| *0*   | 0 | 2 | 1 |
| *1*   | 1 | 0 | 2 |

---

# DFA accepting decimal numbers divisible by 4.
```
δ(r, d) = (10*r + d) mod 4   // r ∈ {0, 1, 2, 3} and d ∈ {0, 1, … , 9}

Accepting states: {R_0}

Non-accepting states: {R_1, R_2, R_3}
```

| Value | `R_0` | `R_1` | `R_2` | `R_3` |
|-------|-------|-------|-------|-------|
| *0*   | `0` * 10 + *0* = 0 → 0 | `1` * 10 + *0* = 10 → 2 | `2` * 10 + *0* = 20 → 0 | `3` * 10 + *0* = 30 → 2 |
| *1*   | `0` * 10 + *1* = 1 → 1 | `1` * 10 + *1* = 11 → 3 | `2` * 10 + *1* = 21 → 1 | `3` * 10 + *1* = 31 → 3 |
| *2*   | `0` * 10 + *2* = 2 → 2 | `1` * 10 + *2* = 12 → 0 | `2` * 10 + *2* = 22 → 2 | `3` * 10 + *2* = 32 → 0 |
| *3*   | `0` * 10 + *3* = 3 → 3 | `1` * 10 + *3* = 13 → 1 | `2` * 10 + *3* = 23 → 3 | `3` * 10 + *3* = 33 → 1 |
| | | | |
| *4*   | `0` * 10 + *4* = 4 → 0 | `1` * 10 + *4* = 14 → 2 | `2` * 10 + *4* = 24 → 0 | `3` * 10 + *4* = 34 → 2 |
| *5*   | `0` * 10 + *5* = 5 → 1 | `1` * 10 + *5* = 15 → 3 | `2` * 10 + *5* = 25 → 1 | `3` * 10 + *5* = 35 → 3 |
| *6*   | `0` * 10 + *6* = 6 → 2 | `1` * 10 + *6* = 16 → 0 | `2` * 10 + *6* = 26 → 2 | `3` * 10 + *6* = 36 → 0 |
| *7*   | `0` * 10 + *7* = 7 → 3 | `1` * 10 + *7* = 17 → 1 | `2` * 10 + *7* = 27 → 3 | `3` * 10 + *7* = 37 → 1 |
| *8*   | `0` * 10 + *8* = 8 → 0 | `1` * 10 + *8* = 18 → 2 | `2` * 10 + *8* = 28 → 0 | `3` * 10 + *8* = 38 → 2 |
| *9*   | `0` * 10 + *9* = 9 → 1 | `1` * 10 + *9* = 19 → 3 | `2` * 10 + *9* = 29 → 1 | `3` * 10 + *9* = 39 → 3 |

> [!NOTE]
> - Just check from value *0* to *3* and `R_0` to `R_3`

**Transition table (simplified):**

- Refine the non-accepting group

| Value | R_0 | R_1 | R_2 | R_3 |
|-------|-------|-------|-------|-------|
| *0,4,8* | 0 | 2 | 0 | 2 |
| *1,5,9* | 1 | 3 | 1 | 3 |
| *2,6*   | 2 | 0 | 2 | 0 |
| *3,7*   | 3 | 1 | 3 | 1 |

- Since, columns R_1 & R_3 are same, we merge them, R_1 = R_3 = R_13.

| Value | R_0 | R_13 | R_2 |
|-------|-------|-------|-------|
| *0,4,8* | 0 | 2 | 0 |
| *1,5,9* | 13 | 13 | 13 |
| *2,6*   | 2 | 0 | 2 |
| *3,7*   | 13 | 13 | 13 |

- Since, rows 2 & 4 are same, we merge them.

| Value | R_0 | R_13 | R_2 |
|-------|-------|-------|-------|
| *0,4,8* | 0 | 2 | 0 |
| *1,3,5,7,9* | 13 | 13 | 13 |
| *2,6*   | 2 | 0 | 2 |

- columns R_0 & R_2 are same, but R_0 is an accepting state R_2 is a non-accepting state, so we can't merge them. You can only merge states if they are indistinguishable- same acceptance status (both accepting OR both non-accepting)


**DFA Details:**
- **States:** R_0, R_1, R_2, R_3 (remainders when divided by 4)
- **Start state:** R_0 (before reading any digits, the "number so far" is 0)
- **Accepting state:** R_0 (numbers divisible by 4 end with remainder 0)
- **Transition function:** δ(current state, digit) = (10 × current state + digit) mod 4

---

# DFA for ending with 00 or 11

The basic states ending with 0 and ending with 1 are required. Additionally, we will have states ending with 00 and 11. Start with a table

**Transition table:**

| S | q0 | q1 |
|--|--|--|
| q0 | q00 | q1 |
| q1 | q0 | q11 |
| q00 | q00 | q1 |
| q11 | q0 | q11 |

---

# DFA for ending with aba

## The Pattern Being Tracked

This DFA appears to be tracking **strings ending with specific substrings**. This is actually implementing the **Knuth-Morris-Pratt (KMP) pattern matching automaton** for pattern "aba"! The subscripts track the **longest prefix of the pattern** that is a suffix of the string read so far.

Target Pattern: **aba**

| State | Meaning (suffix of "aba" seen so far) |
|-------|---------------------------------------|
| qa | Last character was 'a' (not necessarily part of pattern yet) |
| qb | Last character was 'b' |
| qab | Last two characters were "ab" |
| qaba | Last three characters were "aba" (full pattern found!) |

### Transition Logic Examples:

**From qa (ends with 'a'):**
- Read 'a' → now ends with "aa" → longest suffix of "aba"? Only the last 'a' matches → back to **qa**
- Read 'b' → now ends with "ab" → "ab" is a prefix of "aba"! → go to **qab**

**From qb (ends with 'b'):**
- Read 'a' → now ends with "ba" → longest suffix of "aba"? Only the last 'a' matches (since "ba" isn't a prefix) → go to **qa**
- Read 'b' → now ends with "bb" → longest suffix of "aba"? None (just empty string conceptually) → wait, but here it stays **qb**? Actually "bb" has no suffix matching pattern start, but last char is 'b' so stay qb.

**From qab (ends with "ab"):**
- Read 'a' → now ends with "aba" → full pattern! → go to **qaba**
- Read 'b' → now ends with "abb" → longest suffix matching start of "aba"? Last char is 'b', so qb? But check: "abb" last char 'b' means qb, yes.

**From qaba (ends with "aba" - full pattern found):**
- Read 'a' → now ends with "abaa" → longest suffix matching start of "aba"? Last three chars "baa" no, last two "aa" no, last char 'a' → so **qa**
- Read 'b' → now ends with "abab" → longest suffix matching start? "ab" is a prefix! → go to **qab**

| State | a | b |
|-------|---|---|
| S | qa | qb |
| qa | qa | qab |
| qb | qa | qb |
| qab | qaba | qb |
| qaba | qa | qab |

