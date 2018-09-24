# Namespace overview 
- wrap up the global system in an isolated environment
- process within the namespace have their own isolated instance of global resources.
- helps to easy implementation of containers
- List of namespaces
    1. `Mount Namespace`: isolate the filesystem mount point for processes
        1. syscall mount() and unmout()
    2. `UTS Namespace`: isolate the linux hostname and nodename
        1. syscall: `uname()`, `sethostname()`, `setnodename()`, 
        2. name came from the `struct` passed in `uname()` `utsname`
        3. UTS -> Unix timesharing system
    3. `IPC Namespace`: isolate inter-process communication resources namely system V IPC objects and POSIX queues. 
        1. IPC namespace has its own set of System V IPC identifiers and its own POSIX message queue filesystem.
    4. `PID Namespace`: isolate process ids. multiple namespace can have same process id.
    5. `Network Namespace`: isolate network related resources 
        1. Ip table
        2. routing table
    6. `User Namespace`: Isolete user and group id. multiple process can have same user with same id. 
        1. An unprivileged user can used as root privileged user inside a namespace. 