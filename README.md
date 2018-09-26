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

# Namespace API
- Api list
    - `clone()`, `unshare()`, `setn()`
    - clone():
        - clone a function into different namespace
        - clone(func , stack_p, flags, arguments to func);
        - related usefule api
            - `uname(&uts_struct)` copy the system details to uts_struct 
            - `sethostname(str, strlen)` set the hostname of the new namespace 
        - `ls -l /proc/pid/ns` shows the namespace links
        - opening a file descriptor of a namespace file will keep the namespace in `proc` dir even though process is end.
    - setns():
        - join an existing namespace to a process
        - `setns(file_descriptor, nstype)` return `-1` if error occurs
    - unshare()
        - leaving a namespace 
        - `unshare(nsflag)` leave the current namespace 

# PID NAMESPACE
- flag for pid namespace is `CLONE_NEWPID`
- while calling `clone()` with `CLONE_NEWPID` then the process PID will be created in another PID namespace created by `procfs`
- to view the PID's the `/proc` dir should be mounted on another dir
- `procfs` is a filesystem for PID namespace abstruction
- never use the ```/proc``` file for new PID namespace. because it confused the root system. or `host' system
- one process can view the other process if it's resides in same namespace or it's the parent of the namespace.
- ## int process
    - the first process in a new namespace works as init process
    - init process became the parent process of the other orphaned process in that namespace 
    - `signals` that can be delivered to init are those for which the process has established a `signal handler`
    - `PID namespace` will also be destroyed when it's `init process terminates`.
    -  it is not possible to `create new processes` in the namespace `(via setns() plus fork())` the lack of an init process is detected during the fork() call, which fails with an `ENOMEM` error
        - if a namespace doesn't have a `init process` namespace then it's not possible to create a process in that namespace
    - The `ps` a command lists all processes accessible via `/proc`
        - to work `ps` `proc` from `procfs` need to mount on `/proc`
    - orphan process created in a different namespace will have the init process as the parent process.

