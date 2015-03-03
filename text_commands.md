###Text commands
Linux provides utilities for file and text manipulation:

1. Display contents using ``cat`` and ``echo``. 
2. Edit file contents using ``sed`` and ``awk``.
3. Search for patterns using ``grep``.

###Display contents
The ``cat`` is short for concatenate and is often used to read and print files as well as for simply viewing file contents, while the ``tac`` command prints the lines of a file in reverse order.
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
$ sed -i s/Mario/Saverio/ myfile.txt
$ cat myfile.txt
Saverio Rossi
Antonio Esposito
Michele Laforca
```
For example, to convert 01/02/… to JAN/FEB/…
```
sed -e 's/01/JAN/' -e 's/02/FEB/' -e 's/03/MAR/' -e 's/04/APR/' -e 's/05/MAY/' \ 
-e 's/06/JUN/' -e 's/07/JUL/' -e 's/08/AUG/' -e 's/09/SEP/' -e 's/10/OCT/' \
-e 's/11/NOV/' -e 's/12/DEC/'
```
The ``awk`` command is used to extract and then print specific contents of a file and is often used to construct reports. It is a powerful utility and interpreted programming language, used to manipulate data files, retrieving, and processing text.
It works well with fields (containing a single piece of data, essentially a column) and records (a collection of fields, essentially a line in a file).

```
$ awk '{ print $0 }' myfile.txt
Saverio Rossi
Antonio Esposito
Michele Laforca
$ awk '{ print $1 }' myfile.txt
Saverio
Antonio
Michele
$ awk '{ print $2 }' myfile.txt
Rossi
Esposito
Laforca
```
Please, check the man pages for the ``awk`` and ``sed`` commands for futher details.

###File manipulation
The ``sort`` command is used to rearrange the lines of a text file either in ascending or descending order, according to a sort key.
```
# cat myfile.txt
Mario Rossi
Antonio Esposito
Michele Laforca
# sort myfile.txt
Antonio Esposito
Mario Rossi
Michele Laforca
# sort -r myfile.txt
Michele Laforca
Mario Rossi
Antonio Esposito
```
The ``uniq`` is used to remove duplicate lines in a text file and is useful for simplifying text display. It requires that the duplicate entries to be removed are consecutive.

```
# cat myfile.txt
Mario Rossi
Antonio Esposito
Michele Laforca
Antonio Esposito
# sort myfile.txt | uniq
Antonio Esposito
Mario Rossi
Michele Laforca
# sort myfile.txt | uniq -c
      2 Antonio Esposito
      1 Mario Rossi
      1 Michele Laforca
```

The ``paste`` command is used to combine fields from different files

```
# cat names.txt
Mario Rossi
Antonio Esposito
Michele Laforca
Antonio Esposito
[root@caldera01 ~]# cat ages.txt
34
46
29
46
[root@caldera01 ~]# paste names.txt ages.txt
Mario Rossi     34
Antonio Esposito        46
Michele Laforca 29
Antonio Esposito        46
```
