---
title: "Provide custom types for H2"
date: 2023-08-12
---

When using h2 for integration testing together with some library that allows to check database by writing down DB state in separate file (for example - [Database Rider](https://github.com/database-rider/database-rider); [DbUnit](https://www.dbunit.org/)) - there could be a situation when H2 does not support (at least directly, looking at you, json) some column type - it would require to write custom factory to provide those fields manually.

For example, in PostgreSQL there are types `timestamp with time zone`, `json`

Sample code:

```
public class ApplicationNameH2DataTypeFactory extends H2DataTypeFactory {
  @Override
  public DataType createDataType(
      int sqlType, String sqlTypeName, String tableName, String columnName)
      throws DataTypeException {
    if (sqlTypeName.equals("TIMESTAMP WITH TIME ZONE")) {
      return DataType.TIMESTAMP;
    } else if (sqlTypeName.equals("JSON")) {
      return new JsonDataType();
    }
    return super.createDataType(sqlType, sqlTypeName, tableName, columnName);
  }

  public static class JsonDataType extends AbstractDataType {

    public JsonDataType() {
      super("JSON", Types.OTHER, String.class, false);
    }

    @Override
    public Object typeCast(Object obj) {
      return obj.toString();
    }

    @Override
    public Object getSqlValue(int column, ResultSet resultSet) throws SQLException {
      return resultSet.getString(column);
    }

    @Override
    public void setSqlValue(Object value, int column, PreparedStatement statement)
        throws SQLException {
      // we need to use type that is defined in H2 classes
      // otherwise it will be cast to JAVA_OBJECT and fail on insert
      ValueJson valueJson = ValueJson.fromJson(value.toString());
      statement.setObject(column, valueJson);
    }
  }
}
```

Answering your question - why did json require separate class?
Because DbUnit does not have this type.

Also, please be aware, ValueJson is from `org.h2.value` package.
You need to be careful, because if instead of creating ValueJson PGobject would be used (as in this [Database Rider issue comment](https://github.com/database-rider/database-rider/issues/102#issuecomment-511241871)) - DbUnit will fail to recognize this as json type but will assume that provided type is JAVA_OBJECT type.
