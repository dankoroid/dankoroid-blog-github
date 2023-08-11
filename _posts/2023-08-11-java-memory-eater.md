---
title: "Java memory eater example"
date: 2023-08-11
---

Some time ago there was a need to simulate OutOfMemory in application - and thanks to [this Mkyong article](https://mkyong.com/java/how-to-simulate-java-lang-outofmemoryerror-in-java/) I found easy way to do it:

```
public class JavaEatMemory {
  public static void main(String[] args) {
    List<byte[]> list = new ArrayList<>();
    int index = 1;
    while (true) {
      // 1MB each loop, 1 x 1024 x 1024 = 1048576
      byte[] b = new byte[1048576];
      list.add(b);
      Runtime rt = Runtime.getRuntime();
      System.out.printf("[%d] free memory: %s%n", index++, rt.freeMemory());
    }
  }
}
```
