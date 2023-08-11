---
title: "My takes from Martin Fowler Event-Driven Architecture talk"
date: 2023-08-12
---
Few takes from Martin Fowler talk [The Many Meanings of Event-Driven Architecture • Martin Fowler • GOTO 2017](https://youtu.be/STKCRSUsyP0):

1. When introducing events
<br>Pros - decouple sender and receiver 
<br>Cons - no sense of what is going on in system as a whole

    Personally - mentioned con is quite huge.
    <br>I've worked in environment where your app produces data to some topic - that could later be abandoned by team that initially listened to events - and also some other teams could start consuming events without you knowing about them.
    <br>In such environment it could be really tricky to find out proper teams to contact about any changes to the topic

3. Events vs Commands
<br>Events - more like notifications about changes with no other expectations 
<br>Commands - have some expectations
