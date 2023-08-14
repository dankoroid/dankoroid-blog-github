------
https://stackoverflow.com/a/63938554

jsonPath can prevent situations like:

expected <10L> actual <10>

by passing result class as argument to jsonPath

------
https://stackoverflow.com/a/51717144

in mockito you can use nullable(Class) instead of any(Class) to also match null values

------
Mockito - if declaring some stubs in @BeforeEach and then write a test that won't call stubbed service at all - exception will arise, telling that "Unnecessary stubbings detected"

This is due to mockito default configuration to not tolerate stubs that are declared and not used - which is a great default, in my opinion

There are next ways to deal with it:
1. Split your test files (if its 50/50 about tests that use and don't use stub)
2. Move @BeforeEach stubbing only to methods that actually require those stubbings - if it is needed in less amount of test methods
3. In test method that does not use stubs - reset stubbed service with use of Mockito.reset(stubbedService) - if you don't want to split test class onto multiple (in case when there is only 1 such test method)
4. use lenient() before declaring stubs in @BeforeEach - but this is worst scenario, I think, as it introduced 'maybe' case into tests, but we'd better have explicit declaration of expectations

------
AssertJ - when using '#usingRecursiveComparison' to compare some objects by reflection and those objects are contained in some collection - don't forget to use '#ignoringCollectionOrder' to ignore collection order if you need only to compare objects and actually don't care about order

------

