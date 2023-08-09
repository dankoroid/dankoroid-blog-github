---
title: "DBUnit XML datasets - DB table population"
date: 2023-08-09
---

There is a tricky default behavior with DBUnit datasets (at least xml ones) - related to "column sensing".

For example, if dataset is defined in next way - `user_email` will not be populated into DB
```
<user id="1" />
<user id="2" user_email="me@home.com" />
```

And logs would have such messages - which are not quite intuitive:
> 2023-08-08 15:41:10,102 WARN  [] [Test worker] FlatXmlProducer: Extra columns (user_email) on line 3 for table user (global line number is 2). Those columns will be ignored.
>  <br>Please add the extra columns to line 1, or use a DTD to make sure the value of those columns are populated or specify 'columnSensing=true' for your FlatXmlProducer.
>  <br>See FAQ for more details.

It is default behavior of DBUnit to use first encountered row to define the columns of table.
If there will be any other data for same table but with additional columns - those columns would be ignored.

What [DBUnit documentation](https://www.dbunit.org/faq.html#differentcolumnnumber) says about it:
> DbUnit uses the first tag for a table to define the columns to be populated. If the following records for this table contain extra columns, these ones will therefore not be populated. To solve this, either define all the columns of the table in the first row (using NULL values for the empty columns), or use a DTD to define the metadata of the tables you use.

> Since DBUnit 2.3.0 there is a functionality called "column sensing" which basically reads in the whole XML into a buffer and dynamically adds new columns as they appear.

Solutions:
- Add such fields to all data inserts
- Rearrange rows
- Set com.github.database.rider.core.api.configuration.DBUnit#columnSensing to true
