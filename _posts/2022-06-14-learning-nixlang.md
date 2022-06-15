---
title: "Learning nixlang"
date: 2022-06-14
permalink: 2022-learning-nixlang
---

I came across this really neat interactive series about nixlang called [A tour
of Nix](https://nixcloud.io/tour/), so I decided to start there.

### Exercise 8
There are a couple of extensions listed:

> What happens if you create an infinite recursion call to min?

Stack overflow.

> Now, instead of calling your `min` and `max` implementation, use `lib.min`
> and `lib.max`.
>
> The question is actually: what has to be added in order to use lib?

I don't understand how anyone learning for the first time is supposed to know
that the solution requires writing
```nix
with import <nixpkgs> { };
```
at the start. It also doesn't explain what `<…>`, `import`, or `with import`
means.

### Exercise 15
I managed to guess exercise15.ex7 after a few attempts. Reading it as `! (attr ?
a)` I figured the `set ? id` notation means “true iff `set` contains `id`”, and
`! expr` means “false iff `expr` is true”.

### Exercise 16
I didn't get the first part of this one. I didn't realise `name` and `value`
would be treated specially by `listToAttrs`.

### Exercise 17
After completing this and then looking at the solution I learnt the usefulness
of the `with` keyword.

### Exercise 18
It seems to me that `ex03=func ({} // x // z);` is a cleaner solution. Then it
is presumably always the case that `{} // set == set`, so it should be
simplified to just `func (x // z)`.

### Exercise 20
I thought `toList` on a string would result in a list of strings of individual
characters, but no.

### Exercise 22
Ahh, here `with`, `import` and `inherit` are explained.

Although `<…>` is still not explained.

I don't get why `with …; …` uses the semicolon syntax, it looks wrong to me. I
think it should be `in` as well.

### Exercise 23
The solution to this exercise is really badly written and does a terrible job at
explaining this. I think what it's saying is that when you use the `with`
keyword, it might be that you're importing something and you don't know what
names it binds, and you wouldn't want it to shadow any names you're explicitly
defining.

### Exercise 24
`ex05 = isFunction (x:x);` worked fine for me, despite the solution being
different (parsed as a url, which `isString`). Additionally, `ex11` didn't work
without brackets, but the question didn't mention anything about this. The note
is also confusing – `()` is not a valid function, so to me parentheses are
always just representing precedence.

### Exercise 30
Okay it's kinda disgusting how juxtaposition may or may not be function
application depending on whether or not you are inside a list. That is truly
awful syntax design.

### Done
Okay I'm done. Next up is [Nix by example: Part
1](https://medium.com/@MrJamesFisher/nix-by-example-a0063a1a4c55) (that's the
only part there is), followed by Nix Pills, which I believe used to be hosted on  
http://lethalman.blogspot.de/2014/07/nix-pill-1-why-you-should-give-it-try.html
but is now more up-to-date at  
https://nixos.org/guides/nix-pills/why-you-should-give-it-a-try.html
