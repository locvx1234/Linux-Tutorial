###Text commands
Linux provides utilities for file and text manipulation:

1. Display contents using ``cat`` and ``echo``. 
2. Edit file contents using ``sed`` and ``awk``.
3. Search for patterns using ``grep``.

###Display contents
The ``cat`` is short for concatenate and is often used to read and print files as well as for simply viewing file contents. The ``tac`` command prints the lines of a file in reverse order.
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


