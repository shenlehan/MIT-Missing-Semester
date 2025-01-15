### 1. some commands in command line

* signals:
	* `SIGINT`: interrupt --`Ctrl+C`
	* `SIGQUIT`: quit the task -- `Ctrl+\`
	* `SIGSTOP`: stop and hang in the background -- `Ctrl+Z`
	* see the rest by `man signal`
* 
```bash
nohup sleep 2000 &
```
* `&` tells the shell to run it to the background

* `jobs`: see all the tasks
![[Photos/Pasted image 20250115142120.png]]

* use `bg %TASKID` to continue the task on the list above 
![[Photos/Pasted image 20250115142517.png]]

* use `fg` to recover a process to the foreground and reattach it to the standard output

* use `kill` to not only kill a process but send a `SIGNAL`
![[Photos/Pasted image 20250115142626.png]]

* `nohup` will make the process ignore the `HUP` signal(but `KILL` is still useful)


### 2. terminal multiplexer

The main idea is that a _**session**_ includes _**windows**_ and a _**window**_ includes _**panes**_.
![[Photos/Pasted image 20250115145212.png]]
##### Sessions
* `tmux`: to start a new session
* `tmux -ls` : to see all the sessions
* `Ctrl+b` then `d`: to exit the new session
* `Ctrl+D`: end a session
* `tmux a` :to reattach the session
* `tmux new -t NAME` :create a session named `NAME`
* `tmux a -t NAME`: to reattach to a session named `NAME`

##### Windows
* `Ctrl+b` then `c`(create): create a new windows
* `Ctrl+b` then `'p'`(previous): go to the previous windows
* `Ctrl+b` then `'n'`(next): go to the next windows
* `Ctrl+b` then `ID` : jump to the windows with `ID`
* `Ctrl+b` then ',': rename the window

##### Panes
* `Ctrl+b` then `"`: split the current window and create a new pane horizontally
* `Ctrl+b` then `%`: split the current window and create a new pane vertically
* `Ctrl+b` then `arrow keys`:  navigate around the panes
* `Ctrl+b` then `space`: change the current layout for the panes
* `Ctrl+b` then `z`: to zoom the current pane(zoom in for one `z`, zoom out for two)
* `Ctrl+b` the `x`: end the pane
	* When all the panes are closed, use `Ctrl+b` then `x` again to close the window

### 3. dotfiles

* `alias x="something"`: create a new mapping 
	For example, `alias mv="mv -i"`

---------------------------unfinished at 43:22---------------------------