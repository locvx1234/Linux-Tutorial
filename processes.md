###Linux processes
A **process** is simply an instance of one or more related tasks (**threads**) executing on the same machine. It is not the same as a program or a command; a single program may actually start several processes simultaneously. Some processes are independent of each other and others are related. A failure of one process may or may not affect the others running on the system. Processes use many system resources, such as memory, CPU cycles, and peripheral devices such as printers and displays. The operating system (especially the kernel) is responsible for allocating a proper share of these resources to each process and ensuring overall optimum utilization.

A terminal window, is a process that runs as long as needed. It allows users to execute programs and access resources in an interactive environment. You can also run programs in the background, which means they become detached from the shell. Processes can be of different types according to the task being performed. 

|Type|Description|
|--------|---------|
|Interactive |Need to be started by a user, either at a command line or through a graphical interface such as an icon or a menu selection.|
|Batch |Automatic processes which are scheduled from and then disconnected from the terminal. These tasks are queued and work on a FIFO (First In, First Out) basis.|
|Daemons|Server processes that run continuously. Many are launched during system startup and then wait for a user or system request indicating that their service is required.|
|Threads|Lightweight processes. These are tasks that run under the umbrella of a main process, sharing memory and other resources, but are scheduled and run by the system on an individual basis.|
|Kernel Threads|Kernel tasks that users neither start nor terminate and have little control over. These may perform actions like moving a thread from one CPU to another, or making sure input/output operations to disk are completed.|

When a process is in the **running state**, it means it is either currently executing instructions on a CPU, or is waiting for a share (or time slice) so it can run. A critical kernel routine called the **scheduler** constantly shifts processes in and out of the CPU, sharing time according to relative priority, how much time is needed and how much has already been granted to a task. All processes in this state reside on a run queue and on a computer with multiple CPUs there is a run queue on each. Sometimes processes go into the **sleep** state, generally when they are waiting for something to happen before they can resume, perhaps for the user to type something. In this condition a process is sitting in a wait queue. There are some other less frequent process states, especially when a process is terminating. Sometimes a child process completes but its parent process has not asked about its state. Such a process is said to be in a **zombie** state; it is not really alive but still shows up in the system's list of processes.

At any given time there are always multiple processes being executed. The operating system keeps track of them by assigning each a unique process ID or **PID** number. The PID is used to track process state, cpu usage, memory use, precisely where resources are located in memory, and other characteristics. New PIDs are usually assigned in ascending order as processes are born. Thus PID 1 denotes the **init** process (initialization process), and succeeding processes are gradually assigned higher numbers.

Many users can access a system simultaneously, and each user can run multiple processes. The operating system identifies the user who starts the process by the Real User ID or **RUID** assigned to the user. The user who determines the access rights for the users is identified by the Effective UID or **EUID**. The **EUID** may or may not be the same as the **RUID**. Users can be categorized into various groups. Each group is identified by the **RGID**. The access rights of the group are determined by the **EGID**. Each user can be a member of one or more groups. Most of the time we ignore these details and just talk about the **UID**.

At any given time, many processes are running on the system. However, a **CPU** can actually accommodate only one task at a time, just like a car can have only one driver at a time. Some processes are more important than others so Linux allows you to set and manipulate process priority. Higher priority processes are granted more time on the processor. The **priority** for a process can be set by specifying a nice value, or **niceness**, for the process. The lower the nice value, the higher the priority. Low values are assigned to important processes, while high values are assigned to processes that can wait longer. A process with a high nice value simply allows other processes to be executed first. In Linux, a nice value of -20 represents the highest priority and 19 represents the lowest. You can also assign a real-time priority to time-sensitive tasks, such as controlling machines or collecting incoming data. This is just a very high priority and is not to be confused with what is called hard real time which is conceptually different, and has more to do with making sure a job gets completed within a very well-defined time window.

###Running processes
The ``ps`` command provides information about currently running processes, keyed by **PID**. If you want a repetitive update of this status, you can use the ``top`` command or commonly installed variants such as ``htop`` or ``atop`` from the command line. The ``ps`` command has many options for specifying exactly which tasks to examine, what information to display about them, and precisely what output format should be used.

Without options ``ps`` will display all processes running under the current shell. You can use the `` ps -u`` to display information of processes for a specified username. The command ``ps -ef`` displays all the processes in the system in full detail. The command ``ps -eLf`` goes one step further and displays one line of information for every thread (a process can contain multiple threads).

```
# ps -u adriano
  PID TTY          TIME CMD
  847 ?        00:00:00 sshd
  848 pts/2    00:00:00 bash
 1070 ?        00:00:00 sshd
 1071 pts/3    00:00:00 bash
 6475 pts/3    00:00:00 top
```
