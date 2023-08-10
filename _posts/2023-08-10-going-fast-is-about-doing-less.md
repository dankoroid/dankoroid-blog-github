---
title: "Going fast is about going less"
date: 2023-08-10
---

Watched this YT video - [Going fast is about going less](https://www.youtube.com/watch?v=5rb0vvJ7NCY) on channel of [leddoo](https://www.youtube.com/@leddoo).

This video is about optimizing solution of finding shortest way to get to some state in a system with defined rules.

Optimization path from this video was great:

First - start with most brute-forcy algorithm but be sure that is definitely works
<br>After that - add some caching
<br>After that - explore which state should be skipped (in multiple iterations)
<br>And in the end, when a lot of states are actually skipped - caching was not that useful, so removing it improved algorithm time, which is crazy

My main points from this video:
1. Try to do less work in your algorithms
2. Always doubt optimizations to really check that they optimize algorithm and not slow it down
