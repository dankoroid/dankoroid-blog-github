---
title: "H2 integration tests - reset ID column"
date: 2023-08-12
---
During development of some story & covering it with IT tests (including DB) stumbled upon strange issue:
> primary key violation during execution of test method business logic

Reason it was strange is combination of next factors:
1. There were 3 same methods with same DB setup & data - and only one of them failed (failure was on step that is common for all test methods)
2. It tried to insert id that was already existing in DB

After some investigation found out the issue:
Between test methods execution DB tables are cleaned up (all data deleted) - but identity sequences in H2 were not affected by this.

For example, take next situation:
- You have a DB data with some package with id = 10
- You have 3 test methods, each of them creates 4 more packages (12 in total)
- Test that will be executed last (and will create package with id 10) will fail, because id 10 was already inserted by DB data

This only happens in scope of 1 test class, because all DB structure (including tables) are deleted between execution of test classes.

**Fix:**

After/before test method execution have next SQL executed:
```
alter table table_name alter column id restart with 1;
```
