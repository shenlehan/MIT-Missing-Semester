### 1. normal mode
* `:q`: quit
	* `qa`: quit all the windows
* `:w` : write, saving the file
* `:help :COMMAND` : display the help
* `:sp`: split the window

* `w`: move through the words
* `b`: move to the beginning of the word. Keep pressing it to go to word before
* `e`: go to the end of the word
* `hjkl` for left, down, up, right 
* `o`: insert a newline under the cursor
* `O`: insert a newline above the cursor

* `x`: cut the character
* `d` means delete, but it will also copy the content deleted:
	* `dd`: delete the whole line
	* `dw`: delete the word
* `y` means yank(copy):
	* `yy`: copy the whole line
	* `yw`: copy the word
* `c` means change:
	* `cw`: change word, which means delete the word and enter insert mode

* decorators: `i` means inside, `a` means all
* `ci'`: change the content inside `''`
* `ci"`: change the content inside '""'
	* there are also `ci(`,`ci[`...

### 2.  visual mode

In normal mode:
* `v`: enter the visual mode to select words/characters
* `V`: enter the visual mode and select a whole line
* `Ctrl+v`: enter the visual block mode to select the character block