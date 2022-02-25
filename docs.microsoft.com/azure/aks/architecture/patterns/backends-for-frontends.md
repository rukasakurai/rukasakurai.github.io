https://docs.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends

My interpretation is that this basically usually means having each type of front-end application (e.g., mobile application, desktop application) call separate HTTP APIs for each type of UI. The assumption is that there will be different requirements for the backend APIs depending on the type of UI. 

I would personally, start without considering this pattern, unless there is a concreate need to leverage it.