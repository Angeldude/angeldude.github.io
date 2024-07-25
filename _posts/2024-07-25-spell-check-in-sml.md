---
layout: post
title: "Spell Check in SML?"
date: 2024-07-25 18:34:00 -0400
categories: [data structures, programming, software design, learning]
---

## Designing a CLI Spell Checker in SML

What a leap! Excusing my last post about system design, the one before that was about a simple example in _The Little MLer_ and now, here I am, crying, wondering where I went wrong in life. Sorry, I meant, I am now working on designing a *simple* CLI spell checker written in SML as an exercise. It is a work in progress.

# Project Book

To begin, after I wrote my software design book, I found a [site](https://projectbook.code.brettchalupa.com/) about project ideas to work on for learning purposes. Brett Chalupa started the site as a way to give people ideas for something to build while learning a language (or just practicing) and not have it be Yet Another ToDo List&trade;.

The site is split into categories, the first one is CLIs and I saw the spell checker one. I thought "This sounds easy, why would this be a non-trivial project?" OOOOOOOOOooooooohhhhh was I in trouble! I began by writing out preliminary notes.

I should be able to run the program from the command line, pass in a file path as the second argument, and out it spits lines detailing my typos if it finds any. It is simple and free enough to add features later if I wanted such as being able to pass in options or maybe use a different dictionary? But let me not get ahead of myself.

I chose SML because I wanted to get better at functional programming and I know I can get a standalone binary if I use MLton (or some trickery using SML/NJ). And if I really wanted (I do) I can transcribe it into Haskell.

The program would take one input: the file path. I would have to find the file, read its contents (and possibly hold them in memory) and then check each word against a dictionary. So a dictionary would look like... hmm. I suppose I need another file with words to check against.

I would have to load the file, read its contents (memory?) and then... I figured it'd be stored as a string so I would have to convert it into something else. A list of words perhaps. So we break up the string into a list of words and then when we read the file to check's contents, we also do the same and then iterate through one list and check each word by iterating through another list.

I wrote the code for this part.

```
(* match string with path to file; read its contents and store in var *)
(* issues: how to print each line without using another map function; punctuation; hyphenation *)

val contents: string = "Flower patch";
val dictionary: string list = ["flower", "apple"];

fun words (str: string): string list = String.tokens Char.isSpace str;
fun lower (str: string): string = implode(map Char.toLower (explode str));
fun typo (x: string) = List.exists (fn word => (lower x) = (lower word)) dictionary;

fun typos (x: string) = if typo(x) then "" else x;
fun errors (content: string list) = map typos content;
fun get_typos (ls: string list): string list = 
	if List.all (fn x => x = "") ls 
	then []
	else List.filter (fn x => not (x = "")) ls;

(* get_typos(errors (words contents)) *)
```

The `contents` variable is my sample string as well as the dictionary. I would work out the actual reading of files later. For the most part, this works! The last comment shows the functions to call to get a list that only contains the words that have typos. The top comments highlight some issues I have to work out. One thing not noted is I had been going back and forth about whether to check against case or not and I'm still undecided but for now I just lowercase all the words before they get checked.

Of course, there's a serious issue here. Iterating through a list from inside another list? Not good. Also, the hyphenation and punctuation issues were weighing on my mind so I decided to find out how other spell checkers were made. It turns out it is quite hard finding out this information!

# The Struggle

With the way search engines work now and the plethora of garbage sites that try to sell you things, it was quite difficult to navigate. There was a major spell checker called [Hunspell](https://hunspell.github.io/) but that is a huge application written in C++, I have no idea how long it would've taken me to peruse the code for that. Then I learned of [Aspell](http://aspell.net/) and also Ispell. I went with [Ispell](https://www.cs.hmc.edu/~geoff/ispell.html) and downloaded the code for that. It is written in C but it is much smaller than Hunspell and I went to work.

I can't say I understood most of it but I got the gist and almost kicked myself for not realizing the answer. So Ispell loads a dictionary from a file into a hash table! Of course, it is so simple! I looked into hash tables in SML and turns out SML/NJ has a library that implements them but then I found a [Stack Overflow post](https://stackoverflow.com/a/19842954) that suggested that since hash tables are a stateful structure, you'd be better off with something more functional friendly like a Red Black tree. Guess what? SML has those too!

But first, I wanted to fully understand these data structures so I'm currently implementing one in Ruby and playing with it in the REPL to understand how they will work.

# A Thicker Plot

Let's try to understand why a RBTree is a better option than a hash table. A hash table works as a kind of dictionary, or phone book (if you can remember those) where the keys are paired with a value:

```
{
  "key": value,
  "another_key": another_value
}
```

That is just a visual helper, modeled after JSON, as to what a hashmap is doing, don't take it to mean it actually looks like that. In order to get to the value, I need the key. In my case, the key IS the value, so what would I put in place for the value? More importantly, the hash table has a set size which means if I wanted to grow the dictionary with more words, that would be a problem that needs addressing. There's also worst case performance being O(n), caching issues, and possibly key collisions! 😱

A binary search tree, on the other hand, is made up of nodes, two at most in each level. The node itself contains the key which also happens to be the value! A node is inserted based on a rule: if the the value is less than the root, then it goes on the left, otherwise it goes on the right. There's a problem here. If my word list is in alphabetical order then when I add the words to the tree, the root will be the first 'a' word and then each new node will be added to the right. This makes it look like less of a tree and more of a list which was the kind of structure we were trying to avoid!

A Red-Black tree is meant to be a self-balancing tree with special properties that keep it balanced. I won't go into those because the more I learned about the RBTree the more uneasy I felt. Something was nagging at me about this data structure but I couldn't put my finger on it. I did some more investigating and found out something new.

# Trie Harder

A trie (pronounced 'try' even though it comes from the word 'retrieval') is THE data structure I've been looking for. It is widely used as a DS for strings. My main issue with the RBTree was that each node was storing an exact string and something about that felt very inefficient to me. The trie, however, works a little differently. It is a tree that, instead of storing the string in a node, it makes each node a character of that string. That way, you can build off many words if mapped properly.

```
       / \  
       c d
	  /   \
	 a     i
	/\      \
   t  r      p
       \     
		t 
```

That is a rough approximation of what the tree looks like. You can see that you can create the words "cat","car","cart","dip". With an alphabetical list of words, the mapping to this data structure works really well! In fact, this data structure is what allows for autocomplete and autocorrect and spelling suggestions to work. If I wanted to enhance my spell checker, I would not have any problem making it suggest words that were misspelled.

I implemented a rough trie in ruby and played with it and it is fantastic! There is writings on making tries in a functional language including SML and thankfully there is at least two implementations of a trie data structure in SML.

# To be continued...

I've made tremendous amount of progress in what was seemingly a simple idea. I've yet to write any new code down but I'm ending the post here. I will be putting this up on github and I will do another write up when I (hopefully) complete the project. 

[Spell checker in SML](https://github.com/Angeldude/Spell_Check_SML)
