---
title: "Java teeing collector"
date: 2023-08-10
---

Recently found out cool Java collector method that was added in java12 - [Collectors.teeing](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/stream/Collectors.html#teeing(java.util.stream.Collector,java.util.stream.Collector,java.util.function.BiFunction))

Main purpose - to do 2 collectors on 1 stream and then combine results of those 2 collectors in some way.

For example, if needed to split list of some items in 2 based on condition - teeing could help with that:
```
Pair<List<Item>, List<Item>> = items.stream().collect(Collectors.teeing(
    Collectors.filtering(item -> item.price() >= 10, Collectors.toList()),
    Collectors.filtering(item -> item.price() < 10, Collectors.toList()),
    Pair::of
));
```
If needed to find employees with min & max salaries:
```
HashMap<String, Employee> result = employeeList.stream().collect(Collectors.teeing(
    Collectors.maxBy(Comparator.comparing(Employee::getSalary)),
    Collectors.minBy(Comparator.comparing(Employee::getSalary)),
    (e1, e2) -> {
        HashMap<String, Employee> map = new HashMap();
        map.put("MAX", e1.get());
        map.put("MIN", e2.get());
        return map;
    }
));
```
Finding average of values:
```
double average = Stream.of(1, 2, 3, 4, 5).collect(Collectors.teeing(
    Collectors.summingDouble(i -> i), 
    Collectors.counting(),
    (sum, count) -> sum / count)
);
```

Personally for me - it is most useful to split collection of elements based on boolean condition (old/new items, for example).
