### 0. The philosophy behind git

* A folder is a tree, a file is a blob. A commit is a structure including metadata like author, modified date and actual changes etc..
* An object can be a tree, a blob or a commit.
There is a SHA-1 hash map to map the actual content(an object) to a 40 characters long hexadecimal string 
Besides, there is another hash map to map the string mentioned above to the real content stored on disk.

The whole git commit graph is a DAG. A node can have an array of parent nodes.

### 1.  Tracing back

* `git status`: check the status of the current branches

* `git add filename`: add a file to the staging area
	`git add -p`: select parts of the file to be staged, `s` for split etc..

* `git commit -m ""`: commit the content in the staging area

* `git cat-file -p HASH`: see the author, commit message of a commit/file 

* `git log --all  --graph --decorate`: show the git commit graph
	* add `--oneline` for a more concise layout

* `git checkout HASHID`: change the working directory
* `git checkout filename`: set the file to the way it was that the HEAD points to
	* _**NOTICE**_: The `checkout` will actually **change** the content in your working directory, be aware of that.

* `git diff HASH1 HASH2 filename` : compare the difference between the two version of the file

* `git show HASHID` : show the detailed information of the commit with ID


### 2. branching and merging

* `git branch (-vv)`: list all the branches with detail information
* `git branch NEWBRANCH`: create a new branch from current branch
* `git checkout BRANCHNAME`: change to the BRANCH
	* add `-b` to create a new branch and change into it
* `git merge`: merge two branches
* `git mergetool`: use a fancy tool to help resolve merge conflicts
* `git rebase`

### 3. remote

* `git remote`: list the remote repositories
* `git branch --set-upstream-to=origin/master`: set the default remote branch
* `git push`: push the content to the remote repo
* `git fetch <remotename> <url>`: get the content of the remote repo
	* `git pull`: same as `git fetch; git merge`
* `git clone <url> <local path>`: copy the 
	* `git clone --shallow: only the latest snapshot`

* `git blame` : show very detailed information
