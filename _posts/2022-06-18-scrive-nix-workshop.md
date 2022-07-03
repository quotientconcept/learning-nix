---
title: "Nix Workshop by Scrive"
date: 2022-06-18
permalink: 2022-scrive-nix-workshop
---

## Scrive – Nix Derivation Basics

https://scrive.github.io/nix-workshop/04-derivations/01-derivation-basics.html

It seems like the formatting of the file doesn't matter. The derivation hash is
still at the same hash, and the output is still at the same hash.

I tried deleting the `/nix/store/################################-hello.txt.drv`
and `/nix/store/################################-hello.txt` files, and then
things stopped working (`error: opening file '…': No such file or directory`). I
tried running `nix-store --delete …` but it gave me an error saying that it was
still in use by something. I searched with `nix-store --query --referrers …` and
there was one listed as `censored`. I my nix shell and then it still said that
it was in use but the list of referrers was empty.

Eventually I ran `nix-store --verify --check-contents --repair` and then
`nix-build hello.nix` started working again. But the amount of things in my
store with incorrect checksums that needed to be re-downloaded was truly
worrying. Also I'm using btrfs so my file system was checksumming the files, so
it can't have been bitrot.

For the simple `hello.nix` “package”, I have something like:
```
$ time nix-instantiate hello.nix
[…]
0.25s user
$ time nix-build /nix/store/################################-hello.txt.drv
[…]
0.04s user
$ rm result
$ nix-store --delete /nix/store/################################-hello.txt.drv /nix/store/################################-hello.txt
$ time nix-build hello.nix
[…]
0.25s user
```

### Trying to make my own derivation

Okay I finished `04-derivations/01-derivation-basics`. Am I ready to nixify a
LaTeX document yet? I don't think so but let's try anyway.

Running `nix-instantiate` with a builtins.fetchTarball on a specific nixpkgs is
slow because it has to download that specific nixpkgs before it can evaluate the
rest of the expression.

I need to figure out how to copy sources into the temporary build directory.
Okay I can use the `src` attribute of the `mkDerivatiion` function. Just `src =
./.` works in a pinch, but it's terrible since it includes .git, tempfiles,
result symlink, etc., all of which will result in different derivations if
changed, even though they don't actually affect the output. I guess this is an
argument for using a `src` subdirectory as many projects already do.

Also, I managed to figure out that setting `unpackPhase = "true"` disables the
default behaviour mkDerivation with respect to the `src` attribute.

(Also if there is a makefile then `buildPhase` will default to `"make"` when
absent. \[And `buildphase = "true"` can disable it.\])
