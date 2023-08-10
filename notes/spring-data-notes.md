---
write down about spring data projection loading additional fields when there is list of child rows in projection (check if there is a difference if child collection is raw entity or also a projection)

---
If you need to create some child entity that has parent entity as a field - but you have only parent entity id:

use #getReferenceById instead of #findById

This method will return a Proxy - but will not actually call DB to retrieve the entity!

[Vlad Mihalcea blog post about SpringData #findById AntiPattern](https://vladmihalcea.com/spring-data-jpa-findbyid/)

---
