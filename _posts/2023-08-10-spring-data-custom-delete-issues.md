---
title: "Spring Data - custom delete method issues"
date: 2023-08-10
---

Regarding Spring Data delete custom methods - there are at least 2 major issues with them:
1. **Delete event is not flushed before save**
    <br>[Github issue 1100 - Spring Data JPA](https://github.com/spring-projects/spring-data-jpa/issues/1100)
    <br>This leads to situation:
    ```
    repository.delete(originalEntity);
    // no flush here
    newEntity = repository.save(newEntity);
    // org.springframework.dao.DataIntegrityViolationException: could not execute statement; SQL [n/a]; constraint ["UK_ENTITY_DATA_INDEX_B ON PUBLIC.UNIQUEENTITY(VALUE) VALUES (1, 1)"; SQL statement:
    // insert into UniqueEntity (id, value) values (null, ?) [23505-187]]; nested exception is org.hibernate.exception.ConstraintViolationException: could not execute statement
    ```
    The comments later on lead to sharing that this is JPA provider issue, as it is allowed to delay some operations to improve performance.
    <br>And as Spring Data JPA is built on top of JPA provider - that's not considered an issue in Spring Data JPA

2. **When using custom delete method - actual generated SQL would be greatly different from expected**

    For example, let's take this repo method:
    ```
    void deleteByFieldLessThan(int minValue);
    ```
    I expect next SQL to be generated:
    ```
    delete from table where field < :minValue
    ```
    However, this is was actually generated:
    ```
    select * from table where field < :minValue
    delete from table where id = :id
    delete from table where id = :id
    delete from table where id = :id
    ```
    (yes, there were N delete queries)

    I found relatively OK explanation [here - SO 31346356](https://stackoverflow.com/a/31350749/13608717) - and it states that
    > Entity has to be managed before it can be deleted, so a JPA provider (hibernate in your case) will load (the query you see) the entity first, then issue the delete
    
    And that is the reason for loading - entities are loaded into session - but it could actually be optimized to be executed in 1 native query
