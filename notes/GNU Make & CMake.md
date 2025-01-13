[The rules of writing a Makefile][https://seisman.github.io/how-to-write-makefile/rules.html]
### 1. The basic introduction to a `Makefile` document
```Makefile
target ... : prerequisites ...
    recipe
    ...
    ...
```

* Target is an object file or a executable file (.exe) or a label.
* Prerequisites are the targets or files that are essential to build the current target.
* Recipe is the command to execute to build the target (It could be ant shell command).
	The commands in recipe will not be executed unless there are updates in the files/targets listed in the Prerequisites. 
##### An example:
```Makefile
edit : main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o # The target('edit') need these flies(*.o) to build
    cc -o edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o # use the c compiler to link the files(*.o) to build target('edit')

main.o : main.c defs.h
    cc -c main.c
kbd.o : kbd.c defs.h command.h
    cc -c kbd.c
command.o : command.c defs.h command.h
    cc -c command.c
display.o : display.c defs.h buffer.h
    cc -c display.c
insert.o : insert.c defs.h buffer.h
    cc -c insert.c
search.o : search.c defs.h buffer.h
    cc -c search.c
files.o : files.c defs.h buffer.h command.h
    cc -c files.c
utils.o : utils.c defs.h
    cc -c utils.c
clean :
    rm edit main.o kbd.o command.o display.o \
        insert.o search.o files.o utils.o
```

* This file should be saved as `Makefile` or `makefile`.
* When we type command `make` in the shell, the process will start automatically.
##### The Process of `make`

1. `make` will find the file names `Makefile` or `makefile` under current directory
2. If `make` has found the the file, it will find the `target('edit')` and use it as the final target file
3. It continues to check the `*.o` listed _**recursively**_, and if doesn't exist or has been updated, it will update it or rebuild it.
4. Use command 
5. `make clean` to clean up the `*.o` files
###### The variables in `make`

 An example:
```Makefile
objects = objects = main.o kbd.o command.o display.o \
     insert.o search.o files.o utils.o
```

Use `$(objects)` to get the content of it(just like a shell script)

##### The automatic generation

When make sees a `*.o`, it will automatically add the `*.c` into the prerequisite list and will execute the command `cc -c *.c`

##### Another style of `makefile`

```Makefile
objects = main.o kbd.o command.o display.o \
    insert.o search.o files.o utils.o

edit : $(objects)
    cc -o edit $(objects)

$(objects) : defs.h
kbd.o command.o files.o : command.h
display.o insert.o search.o files.o : buffer.h

.PHONY : clean # see it below
clean :
    rm edit $(objects)
```

##### The rule of clearing the directory

```Makefile
.PHONY : clean
clean :
    -rm edit $(objects)
```

Put this part at the end of your `makefile`

About the `.PHONY`:
	This means it's not an actual target, it's just a label(or simply a task or a group of command to be executed)
	The `make` then will not try to find a file named `clean`. Instead, it will execute the command.

`.PHONY` can also be executed recursively:
```Makefile
.PHONY : cleanall cleanobj cleandiff

cleanall : cleanobj cleandiff
    rm program

cleanobj :
    rm *.o

cleandiff :
    rm *.diff
```

### 2. Writing rules

##### An example:
```Makefile
targets : prerequisites
    command
    ...
```

or:
```Makefile
targets : prerequisites ; command
    command
    ...
```

_**Notice**_: There should and must be a `Tab` before the command in example 2

##### Wildcard characters:

1. `*`:
	For example, `*.c` means all files ends with `.c`
	It stands for string of any length.
2. `~`:
	The same as the `~` in shell, which means the home directory of current user.
3. `?`

##### Variables in `make` is actually the `marco` in `C/C++`

For example:
```Makefile
obj = *.o
```

`$(obj)` will not be expanded automatically like `xxx1.o xxx2.o`. It's just `*.o`.

We can use `wildcard` to expand it:
```Makefile
objects := $(wildcard *.o)
```

##### Searching for the files

Using a _**special variable**_ `VPATH`:
	An example:
```Makefile
VPATH = src:../headers
```

This contains two directories divided by the `:` operator: 
`src` and `../headers`
If `VPATH` isn't defined, then `make` will just search in the current directory.

Another solution is using `vpath` which has 3 methods:
```Makefile
vpath <pattern> <directories> # search in the <directories> for files following the <pattern>
vpath <pattern> # clean the searching directories for the files following <pattern>
vpath # clean all the file searching directories
```

About the `<pattern>`: `[^%][a-zA-Z0-9]`
For example, `%.h` means all files ending with `.h`

```
vpath %.h ../headers
```

The `make` will search for all the files ending with `.h` in the directory `../headers` if 

##### Multiple targets

The automatic variable `$@` is the set of all the targets under current rules.
For example:
```Makefile
bigoutput littleoutput : text.g
    generate text.g -$(subst output,,$@) > $@
```

is the same as:
```Makefile
bigoutput : text.g
    generate text.g -big > bigoutput
littleoutput : text.g
    generate text.g -little > littleoutput
```

The `$` in `-$(subst output,,$@)` means to execute a function named `subst` in `Makefile`

##### Static mode

The rules:
```Makefile
<targets ...> : <target-pattern> : <prereq-patterns ...>
    <commands>
    ...
```

* `<targets>` is a list of targets
* `<target-pattern>` is a set of rules that target follows
* `<prereq-patterns>` gives a re-definition of the targets given. For example, `%.o` will be turned to `%.c` if the `target-pattern` and `prereq-pattern` are like `%.o: %.c`


Here's one example:
```Makefile
objects = foo.o bar.o

all: $(objects)

$(objects): %.o: %.c
    $(CC) -c $(CFLAGS) $< -o $@
```

which equals to:
```Makefile
foo.o : foo.c
    $(CC) -c $(CFLAGS) foo.c -o foo.o
bar.o : bar.c
    $(CC) -c $(CFLAGS) bar.c -o bar.o
```

Another example:
```Makefile
files = foo.elc bar.o lose.o

$(filter %.o,$(files)): %.o: %.c
    $(CC) -c $(CFLAGS) $< -o $@
$(filter %.elc,$(files)): %.elc: %.el
    emacs -f batch-byte-compile $<
```

##### Generating the reliability automatically 

_**Most of**_ the compilers allow to find the headers automatically with commands like:
```Shell
cc -M main.c
```

it turns out to be:
```Shell
main.o : main.c defs.h
# suppose that the main.c has #include <defs.h>
```

In `Makefile` you should use `-MM` instead, otherwise the standard library will be included.

The GNU suggests that the compilers should put every automatic generated reliability into a `.d` file. For instance, there should be a file named `hello.d` for `hello.c`.

Here's one example:
```Makefile
%.d: %.c
    @set -e; rm -f $@; \
    $(CC) -M $(CPPFLAGS) $< > $@.$$$$; \
    sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
    rm -f $@.$$$$
```

In our `Makefile` body, we should write:
```Makefile
main.o main.d : main.c defs.h
# add main.d to update it
```


### Writing commands

##### Display a command

Usually, `make` will display the current command on the screen before its execution.


```Makefile

```


##### Executing the commands

Interestingly, the `make` executes command line by line, but the result of the earlier one won't influence the later one.
In order to achieve that, you need to write them in the same line.
For example,
```Makefile
exec:
    cd /home/slh
    pwd
```

This will print the current directory instead of `/home/slh`.

However, this one do so:
```Makefile
exec:
    cd /home/slh; pwd
```

##### The error of commands

To ignore the return value/error code, add `-` for "ignore" before the command.

```Makefile
clean:
    -rm -f *.o
```

or add `-i` or `--ignore-errors` in the `make` command like: `make -i xxx`. This will be effective globally.

Another one is `-k` or `--keep-going`, which will kill the execution of the current rule but will keep on executing the next one.

##### Using `Makefile` recursively

When working on a large project, there might be so many levels of folders. Writing a `Makefile` in different level is a great idea. For example, we have a sub-directory named "subdir":
```Makefile
subsystem:
    cd subdir && $(MAKE)
```
which is the same as:
```Makefile
subsystem:
    $(MAKE) -C subdir
```

_**But**_ the first version will go into the subdirectory, and you need to return back manually like `cd ..`, while the second one won't.

The `$(MAKE)` variable will execute the $\text{make}$ command.