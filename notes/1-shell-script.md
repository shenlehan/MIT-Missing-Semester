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
```

```bash
echo $? # print the error code from the previous command
cd $_ # change directory to the argument of last command, the '$_' will be replaced by the arguments from the last comm and 
```

In a script:
```bash
echo $@ # $@ means all the arguments
echo $# # $# means the number of all the arguments
```

In a script:

```bash
# $1 and $2 are like the "argv" in C
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

