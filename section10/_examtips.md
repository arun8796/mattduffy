=== Simple Workflow Service (SWF)
* workers are programs that interact with amazon SWF to get tasks, process received tasks, and return the tasks
* decider is a program that controls the coordination of tasks, i.e. their ordering, concurency, and scheduling according to the app logic
 * ensures that a task is assigned only once and is never duplicated (as opposed to SQS)

* Domains are the container of the types of tasks.  Domains isolate a set of types, excutions, and task lists from others within the same account
 * create domains in the aws console or with the API RegisterDomain via JSON formatted parameters
 * maximum workflow can be 1 year (measured in seconds)

