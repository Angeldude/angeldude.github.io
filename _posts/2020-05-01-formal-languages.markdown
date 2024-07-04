---
layout: post
title: "Lecture 1: Formal Languages"
date: 2020-05-01 14:25:00 -0400
categories: [formal languages]
---

## Formal languages

Mark Tarver created a lisp-like, optionally typed, programming language called Shen. He wrote a book on it and how to use it. He also wrote a book called "Logic, Proof, and Computation". I plan on writing up notes while I work through the book. The book has exercises and eventually will require Shen to work through them. I won't detail all the answers to the exercises but maybe I'll highlight some that I found particulary troubling. Anything in italics is my personal thoughts and insights. My headings will, more or less, match those from the book. There are 20 chapters (known as lectures) and this is the first one, so let's begin!

- A natural language is the language that we humans use in our everyday lives to communicate, such as English, Spanish, Mandarin, etc..

- A formal language is artificially created, less complicated than natural languages, and formed with arbitrary rules. Computer programming languages are a type of formal language.

- Maths and logics are another class of formal languages. (_I’ve always thought of math as being a way to formalize our imagination_)

- Despite popular opinion, maths are simpler and limited in notation than natural languages. (_I suspect math is reviled because we spend most of our lives living/breathing the language we speak_)
Maths and logics will be grouped under **proof systems**.

- A proof system consists of 4 significant features: `syntax`, `semantics`, `proof rules`, and eventually, `heuristics`.
Syntax is the rules to how to formulate a proper sentence with the right symbols in the formal language. Semantics is how we interpret what those sentences/symbols actually mean. Proof rules allow us to determine what is a proof and what isn’t in this particular formal language. Heuristics are found eventually; rules of thumb that allow us to solve problems in the formal language.

# John Backus

- Backus helped create the first high level computer language, **Fortran** and also helped design **Algol 60**.
- In order to specify Algol precisely, Backus developed a way of notating syntax. This is known and **Backus-Naur Form** or **BNF**.
- A simple formal language is shown known as L<sub>0</sub> the rules are:

```
Rule 1. <sentence> := a
Rule 2. <sentence> := a^<sentence>
```

- `:=` means 'composed of'; the `^` means 'concatenation'.
- The first rule can be understood to mean "A \<sentence\> can be composed of a single letter 'a'".
- The second rule reads "A \<sentence\> can be composed of a single letter 'a' concatenated with another \<sentence\>".
- This way of defining the rules is called a **recursive definition**.
- With these rules in place, we can use them to prove `aaa` is a sentence of the language L<sub>0</sub>.

| Expansions | Rule Used |
| :----------:| :----------:|
| \<sentence\>    |    |
| a^\<sentence\> | Rule 2 |
| aa^\<sentence\> | Rule 2 |
| aaa | Rule 1|

- **Parsing** is how we use a grammar for a language to show that some formula is a sentence of that language. This is how computers can understand the code we write to translate into machine instructions. (_It is also why the computer can't just parse any language arbitrarily, it needs the right interpreter/compiler with the proper parsing rules._)

#### Terminology

- A series of rules providing the syntax for a language is called a **grammar**. 
- Any symbol that appears to the right of the `:=` but not to the left of it is called a **terminal**. 
- The symbol `<sentence>` is called the **distinguished symbol**.
- Any rule which has a symbol to the right of the `:=` and to the left of it is called **recursive**.

# Extended BNF

- **Extended BNF** adds to the **BNF** notation by allowing convenient shorthand.
- An example, going back to the L<sub>0</sub> grammar, we can redefine it as such:

```
1. <sentence> := a | a^<sentence>
```

- the `|` is shorthand for an alternative expansion. So it reads "A \<sentence\> can be composed of a single letter 'a' OR a single letter 'a' concatenated with another \<sentence\>"
- Other shorthand notation: `ε` is the empty expansion meaning something can be removed. <sup>`+`</sup> means one or more and <sup>`*`</sup> means zero or more.

#### Exercise 1.6

_So, this exercise, when I first encountered it, scrambled my brain. I could not figure it out._

"Define a BNF grammar for a language L<sub>2</sub> that generates all strings of the form _a<sub>n</sub>b<sub>n</sub>_ where _a<sub>n</sub>_ and _b<sub>n</sub>_ are equal numbers of _as_ and _bs_. Thus _ab_, _aabb_, _aaabbb_, are all sentences of L<sub>2</sub>, but _aaabb_ and _aabbb_ are not."

<details markdown="1">
<summary markdown="span">Solution Spoiler!</summary>

_I think it took me a few days and when the answer finally dawned on me, I felt a bit stupid. But I'm not stupid since I'm the one that figured it out so I guess that's that. All it took was placing the recursive portion in a different location in the definition._

```
Rule 1. <sentence> := a^b
Rule 2. <sentence> := a^<sentence>^b
```

_Or if you prefer in Extended BNF:_

```
1. <sentence> := a^b | a^<sentence>^b
```
</details>

#### Note

- _There is one major thing of importance. One of the exercises defines a grammar for basic arithmetic. What's not mentioned is that this lecture mainly focused on how syntax is built and not semantics, so proving something like `(5 + 3 ) = (8 / 2)` can serve to confuse when one forgets that you're only meant to prove that it is a properly formed sentence based on the rules, not that it's correct or true._

# Beyond BNF

- BNF grammars are considered **type 2** or **context-free**
- A language whose syntax is characterized by context-free grammars is called a **context-free language**
- Natural languages are considered to be **context-sensitive**.
- Parsing natural languages falls under **natural language processing**.
- _I had to get more context (maybe some pun intended) and looked this [up on](https://en.wikipedia.org/wiki/Context-free_grammar) [wikipedia](https://en.wikipedia.org/wiki/Context-sensitive_grammar). I still don't understand what the **context** is but I'm pretty sure I don't need to know most of this. For further reading, the chapter ends with these recommendations: Syntactic Structures by Noam Chomsky, Syntax Anaylsis and Software Tools by Kevin John Gough, and The Theory of Parsing by Alfred Aho and Jeffrey Ullman_

The next lecture is on Semantics and Proof!
