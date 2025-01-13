### 1. Basic knowledge
##### a. variable
```bash
foo=bar 
# remember not to write foo = bar, because the space will make the shell think "foo" is a function and "=" "bar" are arguments

echo $foo
# '$' is used to refer the value of a variable

```

##### b. the difference between `' '` and `" "`
```bash
echo "Value is $foo"
# result: Value is bar

echo 'Value is $foo'
# result:Value is $foo
# the $foo won't be expanded
```

##### c. operator `$`

```bash
result=$(ls) # save the result of command "ls -l" in the variable 'result'
echo $result 

foo=$(pwd) # pwd <=> print working directory
echo $(foo)
```

```bash
echo $? # print the error code from the previous command
cd $_ # change directory to the argument of last command, the '$_' will be replaced by the arguments from the last command 
```

In a script:
```bash
echo $@ # $@ means all the arguments
echo $# # $# means the number of all the arguments
```

In a script:

```bash
# $1 and $2 are like the "argv" in C
echo $0 # the name of the script
echo $1
echo $2
```


##### d. operator `!!`

```bash
mkdir /mnt/new
# result: permission denied, not having enough access

sudo !!
# the !! will be replaced by the command last time
```
##### e. define a `function`

* create a script:
```bash
vim test.sh # notice the expansion name '.sh'
```

* in vim:
```bash
test() {
  mkdir -p "$1"
  cd "$1"
}
```

* in bash:
```bash
source test.sh # load the content(variables and functions into the shell)
```

##### f. how to call a `funciton`

In the script:
```bash
hello() {
    echo "Hello, $1!"
}

goodbye() {
    echo "Goodbye, $1!"
}

# determine which function to use
if [ "$1" == "hello" ]; then
    hello "$2"  # call hello
elif [ "$1" == "goodbye" ]; then
    goodbye "$2"  # call goodbye
else
    echo "Usage: $0 {hello|goodbye} [name]"
fi
```

In the shell:
```bash
./my_script.sh hello Alice
./my_script.sh goodbye Alice
```

* or:
```bash
source mu_script.sh
```


##### g. `Error Code`

```bash
# in test.txt:
# foobar = 114514
# hello world!

grep foobar test.txt
echo $?

grep hiiiii test.txt
echo $?
```

![[Photos/Pasted image 20241113131130.png]]

* Logic operators like `&&` and `||`
![[Photos/Pasted image 20241113131429.png]]

##### h. the bash output stream

 For example, 
```bash
cat <(ls) <(ls..)
```

This command will get the output from the bash output stream, instead of the stdin.
The results will be like:

![[Photos/Pasted image 20250113095328.png]]

###### i. globbing

*  `*` will match any length of string, while `?` only matches one.
* `{}` will be expanded automatically by shell, allowing us simplify operations. You can use multiple `{}` in the same line.

```bash
ls *.sh # show all the .sh files under current directory
ls text?.txt # show items like text1.txt, text2.txt... 
```

![[Photos/Pasted image 20250113100824.png]]

![[Photos/Pasted image 20250113101030.png]]

![[Photos/Pasted image 20250113101550.png]]

##### j. shebang

The first line in a script is called `shebang`, which tells the shell where the interpreter is to run the script.

For example:
![[Photos/Pasted image 20250113102023.png]]

The first line tells the shell that it should use `/usr/local/bin/python` to interpret the script.
You can also use `env`(a program) to find where the interpreter(here, `python`) is.
```python
#!/usr/bin/env python
```

##### k. use `tldr` or `man` to get explanation and examples of commands

##### l. use `find` to find a file/folder and execute something
For example:
![[Photos/Pasted image 20250113103416.png]]

* `.` means current directory
* `-name` stands for name
* `-path` stands for the path you hope the file should be in
* `mtime -1` means modified date
* `-type` is the type of file you want to find. `d`for directory, `f` for file.

Execute something while finding files:
![[Photos/Pasted image 20250113105606.png]]

* `-exec rm {} \;`: For each file found, the command will execute `rm` (remove) on the file. The `{}` is replaced by the filename of each file found, and `\;` signifies the end of the `-exec` command.
	* - The `-exec` option in `find` requires a delimiter to know where the command you're running should stop. In this case, `\;` signifies the end of the command (`rm {}`).
	- The backslash (`\`) is used to escape the semicolon (`;`) because a semicolon is a special character in the shell and would be interpreted differently without the escape.

Some other ways to search for files
* `fd` is a useful tool which is simple to use. By default, it uses the regular expression.
* `locate` will build up a database index in you file system, so it's much faster.
	* Use `updatedb` to update the database.

##### m. `grep`

```bash
grep foobar temp.txt
# or grep "foobar" temp.txt
grep -R foobar . # search recursively under current directory
```

Some other tools to grep for words
* `rg`:
	For example,
```bash
rg "#include <stdio.h>" -t c -C 5 .
rg -u --files-without-match "^#\!" -t sh .
```
This means:
* Search for `"#include <stdio.h>"`
* In file type `c`
* `-C` provide a context about 5 lines
* Under '.' directory

* `-u` don't ignore hidden files
* `--files-without-match`: files don't match the $\text{regx}$ `^#\!`
(Actually this searches for files without a shebang)

##### n. `history`
This displays the history of your shell commands.
```bash
history 1 # print all history
history # just some part of it
```

Some other ways
* Use `Ctrl + R` to search back-forward commands. Keep using it will browse through the matches.

##### o. `tree` and `nnn` can help you see the structure of the directory