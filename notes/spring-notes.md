------
Spring SPEL - use second property if first is missing:

@Value("#{'${com.mycompany.propertygroup.propertyname:${oldconvention.propertyname:}}'}")

Find out situation when it could be useful

------
