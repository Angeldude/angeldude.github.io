---
layout: post
title: "Getting Idris2 Setup on Windows 10"
date: 2024-06-21 22:46:00 -0400
categories: programming languages
---

I couldn't find any information on this in detail so I had to endure this through trial and error. Here is how I got Idris2 up and running on Windows 10.

### Pre-requisites 

Are there any? I was able to install Idris2 on a machine with only 4GB of RAM and, sure, it took 4+ hours to get a current `(0.7.0)` build, but it worked!

Idris2 can be built using Chez Scheme or Racket. I chose Chez Scheme (10.0.0) so I don't know how to go the Racket route. Get it [here](https://github.com/cisco/ChezScheme/releases/latest)

You will also need to install MSYS2 on your [system](https://www.msys2.org/)
(I am also assuming your machine is 64 bit...Why wouldn't it be?)

Finally, you'll need the Idris2 sources. I used the github [repository](https://github.com/idris-lang/Idris2)

Most of the instructions I followed can be found [here](https://idris2.readthedocs.io/en/latest/tutorial/windows.html)

I used the mingw64 terminal, if you want to use the ucrt terminal, just know that the gcc you'll have to install is differently named.

### Here is the tricky part

Chez Scheme did not give me the option of where to install itself so it defaulted into "Program Files". The problem is that idris2 does not do well with directories with spaces in the name. The solution was to create a symlink! I created a symlink of the entire chez scheme directory into another directory path. Make sure there are no spaces in any of the directory names! 

Note: I created the symlink through the mingw64 terminal, I have not tried this with creating a symlink using powershell but I would expect it to work in a similar manner.

The next step is to add the symlinked directory into your environment variables. Again, I put them in the `.bashrc` file in the mingw64 terminal but I imagine it would work the same if you did it through the windows settings, as long as the terminal can see the path, you're good.

### Build Idris2

Finally, we can build idris2 from the source! Now, you may have a good enought system to build it right off the bat but I went the longer route: I git checked out to one of the first git tags: `v0.2.2`
I did this because bootstrapping it from `v0.7.0` with 4GB of RAM would've took forever. When I bootstrapped it from `v0.2.2`, it took a few minutes. Once I have a bootstrapped idris2, I could move on to the next latest version and simply `make && make install`

I did this until I got to `v0.7.0`. Even though this wasn't bootstrapped, this version took 4 hours to build.

The cleanup consists of adding the idris2 path to my windows environment variables and now I can call it up from powershell!
