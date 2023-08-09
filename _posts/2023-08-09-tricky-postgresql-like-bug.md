---
title: "Tricky bug that seemed to be PostgreSQL related"
date: 2023-08-09
---

Nearly half a year ago I ran into tricky issue during bug investigation:

Sometimes, during calls to repository method like 'findFirstByField(String field)' - underlying DB (PostgreSQL) would fail with:

> org.postgresql.util.PSQLException: ERROR: invalid byte sequence for encoding "UTF8": 0x00

Reason of this DB exception - value used for search contained null byte (yeah, quite easy to see by error message).

As per code - value that was passed to repository method what expected to be valid UUID, taken from [CorrelationId Kafka Header](https://github.com/spring-projects/spring-kafka/blob/main/spring-kafka/src/main/java/org/springframework/kafka/support/KafkaHeaders.java#L133) of messages that app consumed from specific topic.
```
public static final String CORRELATION_ID = PREFIX + "correlationId";
```

This header was used to pass some business value (which was UUID) between 2 services - and consumer would do lookup operation in database by this UUID.

But recently there was added a new producer - which did not know about the rules of correlationId usage that were defined by existing services.
So, this producer did not populated the correlation id kafka header - as it was not required - but catch here is that if correlation id was not provided - it would be populated by [framework's default correlation id strategy](https://github.com/spring-projects/spring-kafka/blob/main/spring-kafka/src/main/java/org/springframework/kafka/requestreply/ReplyingKafkaTemplate.java#L489)

```
private static <K, V> CorrelationKey defaultCorrelationIdStrategy(
		@SuppressWarnings("unused") ProducerRecord<K, V> record) {

	UUID uuid = UUID.randomUUID();
	byte[] bytes = new byte[16]; // NOSONAR magic #
	ByteBuffer bb = ByteBuffer.wrap(bytes);
	bb.putLong(uuid.getMostSignificantBits());
	bb.putLong(uuid.getLeastSignificantBits());
	return new CorrelationKey(bytes);
}
```

Result of that is just a byte array - but consumer tried to convert given byte array to UTF-8 string - and as given input was always random - it could sometimes generate string with null bytes that is [not allowed by PostgreSQL - 4.1.2.2. String Constants With C-Style Escapes](https://www.postgresql.org/docs/current/sql-syntax-lexical.html#SQL-SYNTAX-STRINGS-ESCAPE):
> It is your responsibility that the byte sequences you create, especially when using the octal or hexadecimal escapes, compose valid characters in the server character set encoding.
> 
> The character with the code zero cannot be in a string constant.
