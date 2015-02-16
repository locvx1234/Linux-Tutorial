###Standard File Streams
When commands are executed, by default there are three standard file streams or descriptors always open for use:

1. standard input or **stdin**
2. standard output or **stdout**
3. standard error or **stderr**

Usually, **stdin** is your keyboard, **stdout** and **stderr** are printed on your terminal; often **stderr** is redirected to an error logging file. The **stdin** is often supplied by directing input to come from a file or from the output of a previous command through a pipe. The **stdout** is also often redirected into a file. Since **stderr** is where error messages are written, often nothing will go there.

In Linux, all open files are represented internally by what are called file descriptors. Simply put, these are represented by numbers starting at zero. The **stdin** is file descriptor 0, **stdout** is file descriptor 1, and **stderr** is file descriptor 2. Typically, if other files are opened in addition to these three, which are opened by default, they will start at file descriptor 3 and increase from there.

We can redirect the three standard filestreams so that we can get input from either a file or another command instead of from our keyboard, and we can write output and errors to files or send them as input for subsequent commands. For example, having a program *called do_something* that reads from **stdin** and writes to **stdout** and **stderr**, we can change its input source:
```
$ do_something < input-file
```
If you want to send the output to a file, use the this as in:
```
$ do_something > output-file
```
We can pipe the output of one command or program into another as its input.
```
$ command1 | command2 | command3
```
The above represents what we often call a _pipeline_ and allows linux to combine the actions of several commands into one. 
