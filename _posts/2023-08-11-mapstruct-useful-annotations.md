---
title: "Useful Mapstruct annotations"
date: 2023-08-11
---

To default null to some enum value - and throw exception if unexpected value given (mapstruct 1.5+):
```
@ValueMapping(source = MappingConstants.NULL, target = "SUNDAY")
@ValueMapping(source = MappingConstants.ANY_REMAINING, target = MappingConstants.THROW_EXCEPTION)
@EnumMapping(unexpectedValueMappingException = UnsupportedDayOfWeekException.class)
DayOfWeek map(DayOfWeekType dayOfWeekType);
```

[EnumMapping#unexpectedValueMappingException](https://mapstruct.org/documentation/dev/api/org/mapstruct/EnumMapping.html#unexpectedValueMappingException()) is responsible for type of exception to throw when can't map given value to target.
It is required for provided exception to have constructor with String only - that will have next text: "Unexpected enum constant: " + dayOfWeekType

[MappingConstants](https://mapstruct.org/documentation/dev/api/org/mapstruct/MappingConstants.html) has useful constants that define specific flows/values. Here are some of them:
- ANY_REMAINING - represents any source that is not already mapped by either a defined mapping or by means of name based mapping.
- ANY_UNMAPPED - represents any source that is not already mapped by a defined mapping.
- NULL - represents a null source or target.
- THROW_EXCEPTION - this represents any target that will be mapped to an IllegalArgumentException which will be thrown at runtime.
