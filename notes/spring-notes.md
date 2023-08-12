------
Spring SPEL - use second property if first is missing:

@Value("#{'${com.mycompany.propertygroup.propertyname:${oldconvention.propertyname:}}'}")

Find out situation when it could be useful

------
[24 Feb 2023]
Please be careful with usage of @Async without specifying task executor name.

If intent was to use some specific task executor - please always specify expected executor name. 
As if no executor is specified - it is possible that actual task executor could be unintentionally changed later on.

Reason - new task executor that can be added later and Spring won't be able to resolve initially intended task executor.

When there is only 1 task executor defined in app - it will be provided to all @Async methods/classes.
However if there are multiple task executors defined in app and @Async does not specify which specific it is expected to use - Spring can't choose 1 of multiple task executors (as no criteria given) - and will just silently create a new task executor.

Showing on example:
1. Configuration defines ThreadPoolTaskExecutor (named "exec-1") - and this is the only task executor in application
2. Some method (named "method-exec-1") wishes to use exactly this executor and annotation @Async in applied
3. As "exec1" is the only task executor in app - it will be used for method "method-exec-1"
4. Some new ThreadPoolTaskExecutor (named "exec-2") is created in configuration (for any particular reason)
5. When Spring tried to define which task executor to use for method "method-exec-1" is stumbles upon 2 suitable executors
    In such situation Spring will not choose one of them - but will just create a new SimpleAsyncTaskExecutor and provide it to "method-exec-1"

------
