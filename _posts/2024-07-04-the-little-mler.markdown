---
layout: post
title: "A Type of Problem"
date: 2024-07-04 12:00:00 -0400
categories: [functional programming, ML, SML]
---

# The Little MLer

I recently purchased _The Little MLer_ by Matthias Felleisen and Daniel P. Friedman, and I came across an interesting scenario. It is on Chapter 2 on pattern matching.

## The Setup

We are presented with some datatypes:

```
datatype α shish = 
   Bottom of α
 | Onion of α shish
 | Lamb of α shish
 | Tomato of α shish
 
datatype rod = 
   Dagger
 | Fork
 | Sword
 
datatype plate = 
   Gold_plate
 | Silver_plate
 | Brass_plate
```

Then we are presented with a function `is_veggie : α shish -> bool` that checks if any `α shish` object is veggie or not by returning `false` for any shish that contains `Lamb` and `true` for the rest. Here is its definition:

```
fun is_veggie(Bottom(x)) = true
  | is_veggie(Onion(x)) = is_veggie(x)
  | is_veggie(Lamb(x)) = false
  | is_veggie(Tomato(x)) = is_veggie(x)
```

So now we can check if any of our `α shish`es  are vegetarian or not!

```
is_veggie(Onion(Bottom(Gold_plate))) (* true *)
is_veggie(Onion(Lamb(Bottom(Fork)))) (* false *)

is_veggie(Onion(Bottom(Lamb(Bottom(Fork))))) (* true *)
```

## The Problem

Wait a minute, that last one doesn't seem right. Because the type of `α shish` includes the data constructor `Bottom of α`, it allows a `Bottom` to be any type of value, including another `α shish`! Because of this, I can bypass the `is_veggie` definition of returning `false` whenever it attempts to pattern match against a `Lamb` constructor by passing it in through the `Bottom` constructor.

This is not ideal, so how can we solve this scenario? It is here where I must explain that I am not well versed in ML of any type so it took a bit of soul searching (i.e. searching the internet and asking around).

At first, the concept of modules seemed like the way to go. Modules are similar to Haskell typeclasses (at least that's what most people say and there was even a [paper written on this](https://link.springer.com/chapter/10.1007/978-3-540-89330-1_14).

After some time reading through the paper and experimenting, I learned that this route would be overkill and, besides, it also depends on the implementation of ML you are using.

## The solution

Eventually, I landed on this solution:

```
datatype bottoms = Rod of rod | Plate of plate
``` 

Then, changed the defintion of `α shish` slightly:

```
datatype α shish = 
   Bottom of bottoms
 | Onion of α shish
 | Lamb of α shish
 | Tomato of α shish
```

And there you have it! Sort of. I'm not sure if there's a better way but this is the best I could do for now. This change restricts what types the `Bottom` constructor will allow but it's easy to add more types to `bottoms` if you want to expand the list of types. The other issue is constructing the types also changes to:

```
is_veggie(Onion(Bottom(Plate Gold_plate))) (* true *)
is_veggie(Onion(Lamb(Bottom(Rod Fork)))) (* false *)

is_veggie(Onion(Bottom(Lamb(Bottom(Rod Fork))))) (* This does not typecheck and fails to compile!*)
```
But, as you can see, it prevents us from creating the ill-typed expression in the end! Finally, this also ignores the fact that later in the chapter, a `what_bottom : α shish -> α` function is introduced and my solution makes the need for this function unnecessary because the answer will always be of type `bottoms`.

## Conclusion

I'm not trying to say this book is bad for showing us this flawed example. This merely highlights some of the pitfalls when it comes to programming in general and the difficulty involved in writing and designing any piece of code and not meant as an indictment against the book.

The Little <Blank> series of books are often polarizing, there are those who love them or those that can't stand them. I personally find the dialogue format charming and I love food so I don't hate the food examples either. But if you can look past all that, there are some insights that can be found and I look forward to continuing with this book.
