---
layout: post
title: "Software Design Crisis"
date: 2024-07-17 15:38:00 -0400
categories: [philosophy, pedagogy, software design]
---

I have been thinking about software design a lot lately, mostly about how poorly it is taught and the (perceived) lack of useful resources. Coming from a musical background, I was always used to reading music by the, so called, 'masters' and gleaning insights from their masterful use of harmonies or melodic movement, or their use of rhythm in a thoughtful way. This practice allowed us to then be able to apply the same concepts or principles in our musical writing. There is a huge lack of this in programming.

When I used to teach, a lot of beginner's problems with learning programming, myself included oh those many years ago, was once we had the 'tools' taught to us, we didn't know how to apply them. Sure, I understand what a loop does; Yes, I know how to write a function; Of course I know what a variable is, but how does knowing any of that translate into knowing how to build a: compiler, web server, JSON parser, web page, web app, database, programming language, etc...?

# Software Design

Enter software design: these words _should_ be the next step in the learning journey but it is often met with ridicule or incredulity: "Don't read a book, just build something! Write code, break things, learn from your mistakes! You don't learn how to code from reading!" This is absolutely wild thinking to me. We often repeat the mantra "Don't reinvent the wheel" and yet we continuously tell beginners to do just that. Math textbooks often have tons of exercises meant for the student to be able to practice the concepts they learned. The reason being is simple: These are all solved problems, we want to ramp up the student's knowledge to get to a point where they can work on the hard, yet-unsolved problems. In programming, it seems backwards. How does one know what parts a server needs? What makes it robust and safe or secure? We're often told to roll our own to learn how they work but never use what we build for production, there's already some library that is battle-tested that is better for that anyway.

# Reading Code

Even so, rolling our own still requires knowledge that a beginner wouldn't know about. Dr. Greg Wilson has made steps in the right direction. He notes that he once taught a course in software design but the books that existed during that time were too theoretical with a lack of examples that students could relate to. Thanks to him, he has several books out that seem to follow the ideas that I've been thinking about. _Beautiful Code_, edited by Wilson and Andy Oram, show cases various examples of code that is focused on a particular problem and how it was solved. This reminds me back to the reading of 'the masters' to gain insight. Furthermore, Dr. Wilson has since produced two more books, _The Architecture of Open Source Applications Volumes 1 & 2_, which further explores real world code examples of programs and their code. As a bonus, the AOSA page has other books in similar fashion too! Dr. Greg Wilson's most recent books are _Software Design by Example_, one for JavaScript and one for Python, and it truly makes me happy to see this.

I cannot continue on this subject without mentionining Diomidis Spinellis. He has two books, _Code Reading_ and _Code Quality_, that are in the vein of "read some great code to gain insight" such as the source of the BSD platform!

# Conclusion

All of these books serve to help bridge the gap from knowing programming to knowing how to program applications. I hope to see more of these kinds of books in the future and less of the mindset that feels the need to tell people to just "build", whatever that may mean.

One final note: All those books? They are released under a creative commons license, meaning you can read them for free online (not including _Beautiful Code_ or the Spinellis books)!

[Software Design by Example Python](https://third-bit.com/sdxpy/)

[Software Design by Example Javascript](https://third-bit.com/sdxjs/)

[The Architecture of Open Source Applications (and more!)](https://aosabook.org/)