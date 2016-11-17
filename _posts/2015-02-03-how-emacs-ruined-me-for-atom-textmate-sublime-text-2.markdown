---
layout: post
title: "How emacs ruined me for Atom, TextMate, Sublime Text 2 (and every other GUI based tool)"
date: 2015-02-03 15:14:00 -500
categories: editors coding
---

I’ll start by saying I still consider myself an Emacs newb. These days I spend the majority of my day coding and loving every minute of it. For reference purposes, I span across Ruby, PHP and Javascript (angular, etc.) for my projects. I’m also a bit of a text editor junkie who has tried just about everything I can find including:

- [Sublime Text 2](http://www.sublimetext.com/2)
- [Atom](http://atom.io)
- [TextMate](http://macromates.com)
- [Coda](https://panic.com/coda/)
- [Eclipse](https://eclipse.org) (including various flavors)
- [Brackets](http://brackets.io)

They all had various things to like about them (Sublime was super fast, TextMate has a great plug-in environment, Coda was the best for linking to external server directories and Atom is just really cool in theory) but they all led me to some frustration, often for their own, individual reasons.

Meanwhile, [Ryan McGeary](https://twitter.com/rmm5t) was in the next office for the past year working with Emacs, telling me I should give it a try. I tried…and quit, slows me down to learn. Tried again…and quit, slows me down. Finally, third time was the charm and now I simply can’t go back to the other editors.

Why? Is it the unbelievable number of features? No. Is it the programmable customizations? Nope. Is it that it can be run from a terminal? If anything, that was a negative, at first, for me…so Nope.

The reason I can’t go back is the key sequences for editing…With a little help from Ryan, I re-mapped my Caps Lock key (because programming doesn’t really use Caps Lock) and I learned that the same key sequences work on Mac’s in general but not in most editors without a plug-in. How about some examples:

### Go to the beginning of line:

- Emacs: Ctrl-A
- Everyone Else: Command-Left Arrow

Sounds silly to worry about the difference right? Try going to the beginning of the line without lifting your hands. Can’t do it with Command-Left Arrow.

### Go to the end of line:

- Emacs: Ctrl-E (for end)
- Everyone Else: Command-Right Arrow

Same issue…so much quicker to Ctrl-E than move to the right arrow.

### Go to the next line:

- Emacs: Ctrl-N (for next)
- Everyone Else: Down Arrow

### Go to the next character:

- Emacs: Ctrl-F (for forward)
- Everyone Else: Right Arrow

### Go to the previous character:

- Emacs: Ctrl-B (for backward)
- Everyone Else: Left Arrow

### Highlight Text:

- Emacs: Ctrl-Spacebar (starts the marking area), use cursor keys mentioned and everything in between is highlighted
- Everyone Else: Shift-Arrow-Keys

If every time you just need to move the cursor you have to pick up your hand and find the arrow keys, that adds up.  Muscle memory makes it now that I fly through those sequences and use them everywhere I can on the Mac.

Now, on my first day using Emacs, I didn’t even know how to open a file. So two sequences became my new best friend:

**Ctrl-X, then Ctrl-F:** Open a file anywhere on the file system (and it turns out on other servers as well.

**Ctrl-C, then P, then P:** The P is for “Projectile”, the plug-in that automatically turns an folder with a .git folder into a project. It allows me to determine a project on my file system, then choose a file within it.

After that it’s just **Ctrl-C P F** to open another project file without having to specify a directory or project. It just assumes I want a file in the same project.

How about all of the files I have open in that project? **Ctrl-C P B** (for buffer), start typing the file name (or any sequenced letters) and the fuzzy search shows you the matches automatically.

For me, a truly great tool allows you be productive pretty quickly in the shallow end of the pool. Then, you can keep wading into deeper and deeper functionality as your comfort grows. Admittedly, Emacs’ shallow end is about five feet deep but once you get in, it’s like swimming with fins…Way faster, way more efficient and way more fun.

I tried going back to Atom just because a GUI still occasionally calls to me. What I discovered is that I really don’t need clickable icons anymore…my keys never leave the home row…
