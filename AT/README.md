# Automata Theory

### What is an Automaton

- An abstract machine that reads input step-by-step 
- changes state according to predefined rules to decide something.

1. What is Automata Theory?

Automata theory is the study of abstract computing devices, or "machines," that follow predetermined rules to process inputs and produce outputs. These models help us understand what can be computed, how efficiently, and with what resources.

2. The Hierarchy of Automata (Chomsky Hierarchy)

Automata form a hierarchy based on computational power:

```
Most Powerful → Least Powerful
Turing Machines
    ↓
Linear Bounded Automata
    ↓
Pushdown Automata
    ↓
Finite Automata
```

3. Finite Automata (FA) - The Foundation

Simplest automata with finite memory. They read input string symbol by symbol and change states based on current state and input.

Types:

1. DFA (Deterministic Finite Automaton)
   - Exactly one transition per input symbol from each state
   - No ε (empty string) transitions
   - Example: Traffic light controller
2. NFA (Nondeterministic Finite Automaton)
   - Can have multiple transitions for same input
   - Can have ε-transitions
   - Key Theorem: Every NFA can be converted to an equivalent DFA

> Closure Representation in TOC
1. L+: "Positive Closure" - a set of all strings except Null or ε-strings.

2. L*: "Kleene Closure" - occurrence of certain alphabets from zero to the infinite number of times for given language alphabets. In which ε-string is also included.

L* = εL+

Formal Definition (DFA):

A DFA is a 5-tuple: (Q, Σ, δ, q₀, F)

- Q: Finite set of states
- Σ: Finite input alphabet
- δ: Transition function Q × Σ → Q
- q₀: Start state
- F: Set of accepting/final states

Example:

DFA accepting strings ending with "01" over {0,1}:

```
States: q₀, q₁, q₂
Start: q₀
Accepting: q₂

Transitions:
q₀ --0--> q₁
q₀ --1--> q₀
q₁ --0--> q₁
q₁ --1--> q₂
q₂ --0--> q₁
q₂ --1--> q₀
```

4. Regular Expressions and Languages

What they describe:

Finite automata recognize regular languages, which can be described by regular expressions.

Regular Expression Operations:

· Union: (a|b) matches a OR b
· Concatenation: (ab) matches a followed by b
· Kleene Star: a* matches zero or more a's

Example:

· (0|1)*01 = Strings ending with 01
· a(a|b)*b = Strings starting with a, ending with b

Key Equivalence (Kleene's Theorem):

1. Every regular expression has an equivalent NFA
2. Every NFA has an equivalent regular expression
3. DFAs, NFAs, and regular expressions all describe regular languages

---

5. Pushdown Automata (PDA)

Why we need them:

Finite automata can't handle nested structures (like parentheses). PDAs add a stack for memory.

What they recognize:

Context-free languages (more powerful than regular languages)

Example languages PDAs recognize:

· Balanced parentheses: ((()))()
· Palindromes: aba, abba
· {0ⁿ1ⁿ | n ≥ 0} (equal 0s and 1s)

Formal Definition:

A PDA is a 6-tuple: (Q, Σ, Γ, δ, q₀, F)

· Γ: Stack alphabet
· δ: Q × (Σ∪{ε}) × (Γ∪{ε}) → Q × Γ*

---

6. Turing Machines (TM) - The Ultimate Model

The "Gold Standard" of Computation:

Alan Turing's 1936 model that defines what is computable.

Components:

1. Infinite tape (memory)
2. Read/write head
3. Finite control (states)
4. Instruction table (transition function)

What they can do:

· Recognize recursively enumerable languages
· Simulate any algorithm
· Church-Turing Thesis: "Anything computable is computable by a Turing machine"

The Halting Problem:

· Undecidable: No algorithm can determine if an arbitrary program will halt
· First proof of limitations of computation

---

7. Key Concepts & Applications

Automata in Practice:

1. Lexical Analysis (compilers): Regular expressions → DFA
2. Text Search: grep uses finite automata
3. Syntax Parsing: PDAs for programming language syntax
4. Software Verification: Model checking using automata

Important Theorems:

· Pumping Lemma: Proves languages are NOT regular/context-free
· Myhill-Nerode Theorem: Determines minimal DFA size
· Rice's Theorem: Most questions about program behavior are undecidable

---

8. Recommended Resources

Books:

· Introduction to the Theory of Computation by Michael Sipser (Gold standard)
· Automata and Computability by Dexter Kozen
· Introduction to Automata Theory, Languages, and Computation by Hopcroft, Motwani, Ullman

Visual Tools:

· JFLAP (Java Formal Languages & Automata Package)
· Automata Simulator (online)
· Turing Machine Simulators

Online Courses:

· MIT OpenCourseWare (6.045J/18.400J)
· Coursera: "Automata Theory" by Stanford


