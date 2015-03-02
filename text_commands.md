###Text commands
Linux provides utilities for file and text manipulation:

1. Display contents using ``cat`` and ``echo``. 
2. Edit file contents using ``sed`` and ``awk``.
3. Search for patterns using ``grep``.

###Display contents
The ``cat`` is short for concatenate and is often used to read and print files as well as for simply viewing file contents.

The ``tac`` command prints the lines of a file in reverse order.
```
$ cat > myfile.txt
Mario Rossi
Antonio Esposito
Michele Laforca
Ctrl-D
$ cat myfile.txt
Mario Rossi
Antonio Esposito
Michele Laforca
$
$ tac myfile.txt
Michele Laforca
Antonio Esposito
Mario Rossi
```
The ``echo`` simply displays text.
```
$ echo myfile.txt
myfile.txt
]$echo HOME
HOME
$ echo $HOME
/home/ec2-user
```
###Edit file content
The command ``sed`` is a powerful text processing tool. Its name is an abbreviation for stream editor. It filters text as well as perform substitutions in data streams. Data from an input source/file (or stream) is taken and moved to a working space. The entire list of operations/modifications is applied over the data in the working space and the final contents are moved to the standard output space (or stream).
```
$ sed s/Mario/Saverio/ myfile.txt
Saverio Rossi
Antonio Esposito
Michele Laforca
$ cat myfile.txt
Mario Rossi
Antonio Esposito
Michele Laforca
$ sed s/Mario/Saverio/ myfile.txt >  myfile2.txt
$ cat myfile2.txt
Saverio Rossi
Antonio Esposito
Michele Laforca
```

