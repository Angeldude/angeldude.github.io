---
layout: post
title: "Lecture 2: Semantics and Proof Pt 1"
date: 2024-06-21 13:03:00 -0400
categories: [formal languages]
---

## Semantics and Proof

*BTW, It's been 2 years since my last post (2024 edit: much longer still). I'm getting back into things and I will also add that I have a firm grip on what **context free** and **context sensitive** means! It is the difference between knowing what the 'It' refers to in the beginning of this sentence.*

# Model Theory

- Formal semantics is the study of the allocation of meaning to the sentences of a language.
- Programming languages are for directing computers, maths and logics are for making assertions.
- proof and meaning belong to math and logic and why they are categorized as proof systems.

L<sub>4</sub> specification:

```
<sentence> := <arithexpr> = <arithexpr>
<arithexpr> := (^<arithexpr><op><arithexpr>^) | <variable> | <number>
<op> := + | - | * | /
<variable> := w | x | y | z | <variable>^'
<number> := <digit> | <digit>^<number>
<digit> := 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

A proof system's symbols can be divided into two camps: **constants** and **variables**. Constants have fixed and understood meanings. In the formal language above, the `<op>` and `<digit>` definitions contain well known mathematical symbols. We are to assume the meaning we have ascribed to them in the real world.

Using this knowledge, a sentence of `4 + 4 = 8` we, knowing basic arithemetic, can say this is a true sentence. Something like `3 + 5 = 23` would be false. If a constant is replaced with a variable, such as `w + 3 = 4`, then this is called an **open sentence**. These are neither true nor false. We can ask if there are solutions to these that can make them true.

- An **interpretation** is provided to an open sentence when we fix a meaning to the variables. The interpretation must be of the right kind, otherwise sentences such as `3 + apple = ß` would be created.
- When the variables in a sentence are interpreted into constants, we have a **closed sentence**.
- When an interpretation makes an open sentence true, that is a **model** of the sentence.
- A **countermodel** is when the interpretation makes an open sentence false.
- At least one model in a sentence makes it **satisfiable**.
  - An open sentence is **valid** when every interpretation is a model.
  - An open sentence is **countervalid** when every interpretation is a countermodel.
- [**Truth conditions** or **model theoretic**](https://en.wikipedia.org/wiki/Proof-theoretic_semantics) determine what counts as an interpretation and what conditions that interpretation makes the sentence true.

## Proof Theory

Instead of models and interpretations, there is an alternative of providing a series of rules and axioms which state how an assertion is to be proved. **Logical positivists** believed that explaining how to prove an assertion would also explain the meaning of that assertion. They took this seriously and identified meaning of natural language assertions this way.

In a proof system, this is an easier pill to swallow. A proof in mathematics is not simply an assertion but a "reasoned monologue about the relations between a conclusion to be proved and the assumptions necessary to prove it."

A proof usually follows a pattern of "Theorem: if such-and-such then so-and-so" then the proof. The fundamental object of a proof is not an assertion but a pair made up of a list of assumptions *(such-and-such)* and what is to be proved *(so-and-so)*. This is called a **sequent**.

# Gerhard Gentzen
- Gerhard Gentzen equated sequents with objects of proof
- He developed sequent calculus during WWII
- Died of malnutrition in 1945
- In sequent based proof system, provability is defined by **sequent rules**
- The form is: 

<pre>Where Δ is a list of assumptions and <i>p</i> the conclusion and where n ≥ 0;
a sequent of the form Δ, <i>p</i> is provable if the sequents S<sub>0</sub>....S<sub>n</sub> are provable.</pre>

This is Part 1 because it has been sitting on my computer for ages (including my original update) and I just want to post. I may get back into this series but up until this point, it's fine.
