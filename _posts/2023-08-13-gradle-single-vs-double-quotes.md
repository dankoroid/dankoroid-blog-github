---
title: "Gradle - single vs double quotes"
date: 2023-08-13
---

Regarding gradle build files - I've seen both single & double quotes and wondered what was the difference in usage between them - and just stumbled upon [this SO answer](https://stackoverflow.com/a/15172175/13608717):

> The main difference is that double-quoted String literals support String interpolation

For example, if defining dependency - it can be done this way:
```
lombokVersion=1.18.28
"org.projectlombok:lombok:${lombokVersion}"
```
and `lombokVersion` will be replaced with value of this variable

