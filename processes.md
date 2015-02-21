###Linux processes
A **process** is simply an instance of one or more related tasks (**threads**) executing on the same machine. It is not the same as a program or a command; a single program may actually start several processes simultaneously. Some processes are independent of each other and others are related. A failure of one process may or may not affect the others running on the system. Processes use many system resources, such as memory, CPU cycles, and peripheral devices such as printers and displays. The operating system (especially the kernel) is responsible for allocating a proper share of these resources to each process and ensuring overall optimum utilization.

A terminal window, is a process that runs as long as needed. It allows users to execute programs and access resources in an interactive environment. You can also run programs in the background, which means they become detached from the shell. Processes can be of different types according to the task being performed. 

|Type|Description|Example|
|--------|---------|-----|
|Interactive |Need to be started by a user, either at a command line or through a graphical interface such as an icon or a menu selection.|bash, firefox, top|
|Batch |Automatic processes which are scheduled from and then disconnected from the terminal. These tasks are queued and work on a FIFO (First In, First Out) basis.|updatedb|
|Daemons|Server processes that run continuously. Many are launched during system startup and then wait for a user or system request indicating that their service is required.|httpd, xinetd, sshd|
|Threads|Lightweight processes. These are tasks that run under the umbrella of a main process, sharing memory and other resources, but are scheduled and run by the system on an individual basis.|firefox|
|Kernel Threads|Kernel tasks that users neither start nor terminate and have little control over. These may perform actions like moving a thread from one CPU to another, or making sure input/output operations to disk are completed.|kswapd0, migration, ksoftirqd|

