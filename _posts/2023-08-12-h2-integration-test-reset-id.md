---
title: "H2 integration tests - reset ID column"
date: 2023-08-12
---

When using H2 for integration testing together with some library that allows to check database state after test with some dataset (extracted to separate file):

It can be useful to reset H2 table ids between test methods - so those expected datasets could contains ids close to values that id starts from.

Reason for that - when there are multiple repository tests in 1 class - table is not recreated between runs, only cleared.

This leads to situation that id keeps increments between test methods - potentially leading to id constraint violation

Script to restart id in H2:
```
alter table table_name alter column id restart with 1;
```
