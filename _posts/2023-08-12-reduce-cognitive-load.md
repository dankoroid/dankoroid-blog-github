---
title: "Reduce cognitive load of your code"
date: 2023-08-12
---

My thoughts on [Death by design patterns, or On the cognitive load of the abstractions in the source code](https://alentred.medium.com/cognitive-load-of-abstractions-in-the-source-code-20a889a106d4) article written by [Alen Tred](https://alentred.medium.com/) together with some [comments on HackerNews](https://news.ycombinator.com/item?id=36118093):

What does it mean to have clean code, good abstraction?

To answer this - let's review term named cognitive load.
> Cognitive load, in simple terms, describes amount of your memory needs for some actions. Higher it is - higher is the cognitive load.

Returning to question about clean code & good abstraction - both are good when they reduce cognitive load.

And some quotes on this topic:
- [Alen Tred](https://alentred.medium.com/cognitive-load-of-abstractions-in-the-source-code-20a889a106d4):
  > So, before introducing abstractions to a new project I now try to decide if future readers would be able to confidently make changes without jumping all over the source code tree
- [ngngngng](https://news.ycombinator.com/item?id=36118284)
  > One of the strangest ironies of my career is that the smartest developers often write the worst code. Their perfect memories enable them to effortlessly work with infinite layers of abstraction and write the most clever solutions imaginable to any problem.
- [	pschuegr](https://news.ycombinator.com/item?id=36119629)
  > One of the single best pieces of programming advice I ever received when I was young was my TL saying "I don't like this, it's too clever"
  
  > 1) Less code is better code. The only code with no bugs is no code. After you get something working, remove as much code as you can.
  > 2) Simple code is better code. Don't waste time making your code complex for the sake of being efficient until you've used it and proven that it's a bottleneck with a profiler. Otherwise, just do the simplest thing you can think of that will work.
- [epolanski](https://news.ycombinator.com/item?id=36118544)
  > Good rule of thumb is to write code the least skilled or experienced person can understand on its own without guidance
