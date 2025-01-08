### 1. Basic commands

##### a.  `cd`
```bash
cd .  # change directory to current
cd .. # change directory to father directory

cd ~ # '~' will take you back to the home directory, like "/home/shenlehan"
cd - # '-' will take you to the directory you visited last time

cd / # back to the root directory
```

##### b. `ls`
```bash
ls    # list all the files under current directory
ls /  # list all the content under the root, you can add something to get the certain directory
ls -l # show the detailed information

# [d][rwx][rwx][rwx]
# [d] means this is a directory
# [rwx]:
# r means readable
# w means writable
# x means excutable
# if is not r/w/x then it's -
# the first [rwx] group is the permissions set for the owner of the file
# the second [rwx] group is the permissions set for the group of the file
# the third [rwx] group is the permissions set for the others of the file
```

![[Photos/Pasted image 20241112134049.png]]

* The meaning of `"rwx"` for a directory:
	* `r` means "Are you allowed to see which files are inside the directory?"
	* `w` means "Are you allowed to rename, create, or remove files within that directory?"
		* if you have permissions to a files but don't to its directory, then you can't delete it, but only to empty it
	* `x` means "Are you allowed to enter/search the directory"
		* if you want to access a certain file, you need to have the `x` permissions for _**all**_ its parent directory 


##### c. dealing with files

```bash
pwd   # print working directory
mkdir "path" # create a new directory named 'path'
mkdir hello\ world # '\ ' is a space

echo $PATH # echo will print what is given, and PATH is the environment variables, and $ means to get its value
# shell will search in the path in PATH to find where the commands are

which echo # this will print where function "echo" is 

mv old_path_name new_path_name # you can use mv to rename a file or to move a file
cp old_path_name new_path_name # copy a file
rm FILE_NAME # remove a file, which is defaultly not recursive

rm -r DIR_NAME # remove all file under DIR_NAME to clean the directory
rmdir DIR_NAME # remove a empty directory named DIR_NAME, use this with the one above

mkdir "A NEW DIR" # create a new directory
```

##### d. `man`
```bash
man FUNC_NAME # print the mannual of a function
```

### 2. `istream` and `ostream`

##### a.  `echo` & `cat`

``` bash
echo 'hello world'
# print hello world in the terminal
echo 'hello world' > hello.txt
# result: hello world in hello.txt

cat hello.txt
# print the content of hello.txt

cat < hello.txt
# print the content of hello.txt, the same as the one above

cat < hello.txt > hello_1.txt
# print the content of hello.txt into hello_1.txt, overriding the original content

cat < hello.txt >> hello_1.txt # append but not override
```

##### b. the pipe `|`

```bash
ls -l | tail -n2 # print the last two lines of the content of "ls -l"
ls -l | tall -n1 > test.txt
```

##### e. `tee`

```bash
echo 'hello' | tee hello.txt # to see the content on the screen and to print it in to a file
```

![[Photos/Pasted image 20241112142740.png]]
### 3.the super user  `root` & `sudo`

![[Photos/Pasted image 20241112141852.png]]

* The `/$` means you are now operating as a normal user
* The `/#` means you are now the root user

* What to do if I want to execute something as a sudo?
	For example:
```bash
sudo echo 500 > brightness
```
Shell is opening the program "brightness", not sudo.
So it has no access to it.

An option is this:
```bash
# sudo echo 500 > brightness 
```
Here the `#` means to operate as root.

Another option is this:
```bash
sudo su # become the root
```

