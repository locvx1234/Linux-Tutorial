###Standard File Streams
When commands are executed, by default there are three standard file streams or descriptors always open for use:

1. standard input or _stdin_
2. standard output or *stdout*
3. standard error or *stderr*

Usually, *stdin* is your keyboard, *stdout* and *stderr* are printed on your terminal; often *stderr* is redirected to an error logging file. The *stdin* is often supplied by directing input to come from a file or from the output of a previous command through a pipe. The *stdout* is also often redirected into a file. Since *stderr* is where error messages are written, often nothing will go there.

In Linux, all open files are represented internally by what are called file descriptors. Simply put, these are represented by numbers starting at zero. The *stdin* is file descriptor 0, *stdout* is file descriptor 1, and *stderr* is file descriptor 2. Typically, if other files are opened in addition to these three, which are opened by default, they will start at file descriptor 3 and increase from there.

```
```
