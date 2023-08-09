---
title: "MapStruct - Implicit & Explicit mapping"
date: 2023-08-09
---

If you need to map 2 objects to 1 new object, assuming that all fields from 1 object (let's call it implicit object) need to be in new object, but from 2nd object (let's call it explicit) we need only specific fields - then next trick could be useful:

Define 3 methods in mapper:
1. populateImplicitly - to map implicit fields from 1st object
2. populateExplicitly - to map explicit fields from 2nd object
3. combineOrdered - to combine those methods

```
@Mapper
public interface TestMapper {

    TestMapper INSTANCE = Mappers.getMapper(TestMapper.class);

    default ResultClass combineOrdered(ImplicitClass implicitClass, ExplicitClass explicitClass) {
        ResultClass resultClass = populateImplicitly(implicitClass);
        populateExplicitly(resultClass, explicitClass);
        return resultClass;
    }

    ResultClass populateImplicitly(ImplicitClass implicitClass);

    @BeanMapping(ignoreByDefault = true)
    @Mapping(source = "explicitClass.explicitField", target = "explicitField")
    void populateExplicitly(@MappingTarget ResultClass resultClass, ExplicitClass explicitClass);
}
```

Taken from this [SO answer](https://stackoverflow.com/a/75750645/13608717)
