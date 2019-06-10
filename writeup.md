---
layout: default
---

## Note: Use a variant of the following command to ssh into a level :

`$ ssh -p 2220 -L 1234:localhost:22 bandit.labs.overthewire.org -l bandit**`

Statements beginning with a "$" symbol are commands.

# Level 26 --> Level 27

_Good job getting a shell! Now hurry and grab the password for bandit27!_

While still in the shell opened to get to level 26, we will specify bash as the shell for vim.Type :set shell=/bin/bash and then :sh

This should open the bandit26 bash shell. To get the password for level27, run the following command:

`$  ./bandit27-do cat /etc/bandit_pass/bandit27`

The output is: 3ba3118a22e93127a4ed485be72ef5ea

# Level 27 --> Level 28

_There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo. The password for the user bandit27-git is the same as for the user bandit27.Clone the repository and find the password for the next level_

The instructions for this level are fairly clear. I couldn’t clone the repository in the home directory, so I switched to the /tmp directory and created a new directory as follows:

`$ mkdir /tmp/moretmp`

`$ cd /tmp/moretmp`

Now, we clone the repository as follows and use the password obtained for level27:

`$ git clone ssh://bandit27-git@localhost/home/bandit27-git/repo`

Now change directory into the cloned repository called repo. A README exists in the directory containing the password to level 28.

`$ cat README`

`The password to the next level is: 0ef186ac70e04ea33b4c1853d2526fa2`

# Level 28 --> Level 29

_There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo. The password for the user bandit28-git is the same as for the user bandit28.
Clone the repository and find the password for the next level._

Same as before, we cd into tmp and create a new working directory.

`$ mkdir /tmp/level28`

`$ cd /tmp/level28`

Clone the git repository and use the password for level28:

`$ git clone ssh://bandit28-git@localhost/home/bandit28-git/repo`

The password is redacted from the README so we’ll have to do some digging. Type “git log” to see the log of changes made to the repository. This reveals the following:
![Screenshot](./assets/level28-29.png)

Let’s checkout the branch before the information leak was fixed:

`$ git checkout 186a1038cc54d1358d42d468cdc8e3cc28a93fcb`

Now, the README containes the password for the next level: bbc96594b4e001778eee9975372716b2

# Level 29 --> Level 30

_There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo. The password for the user bandit29-git is the same as for the user bandit29.
Clone the repository and find the password for the next level._

Clone the repo in a /tmp directory as before:

`$ git clone ssh://bandit29-git@localhost/home/bandit29-git/repo`

Looking at the README reveals that no passwords were revealed in producted. So, they probably exist in development.
Lets read the packed-refs file in the .git directory

`$ cat .git/packed-refs`

` pack-refs with: peeled fully-peeled`

`33ce2e95d9c5d6fb0a40e5ee9a2926903646b4e3 refs/remotes/origin/dev`

`84abedc104bbc0c65cb9eb74eb1d3057753e70f8 refs/remotes/origin/master`

`2af54c57b2cb29a72e8f3e84a9e60c019c252b75 refs/remotes/origin/sploits-dev`

Now, checkout the dev branch

`$ git checkout 33ce2e95d9c5d6fb0a40e5ee9a2926903646b4e3`

Now, the README contains the password for the next level: 5b90576bedb2cc04c86a9e924ce42faf

# Level 30 --> Level 31

_There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo. The password for the user bandit30-git is the same as for the user bandit30.
Clone the repository and find the password for the next level._

Clone the repo in a /tmp directory as before:

`$ git clone ssh://bandit30-git@localhost/home/bandit30-git/repo`

The README is of no help this time around. So, let’s look at the packed-refs again:

`$ cat .git/packed-refs`

`pack-refs with: peeled fully-peeled`

`3aa4c239f729b07deb99a52f125893e162daac9e refs/remotes/origin/master`

`f17132340e8ee6c159e0a4a6bc6f80e1da3b1aea refs/tags/secret`

Trying to checkout the secret branch will result in a fatal error. So, we use the git show command to look at the metadata of the commit:

`$git show f17132340e8ee6c159e0a4a6bc6f80e1da3b1aea`

47e603bb428404d265f59c42920d81e5

This is the password for the next level :)

# Level 31 --> Level 32

_There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo. The password for the user bandit31-git is the same as for the user bandit31.
Clone the repository and find the password for the next level._

Clone the repo in a /tmp directory as before:

`$ git clone ssh://bandit31-git@localhost/home/bandit31-git/repo`

The README suggests that the password will be found when we push a file to the master branch containing the given text. So, lets create a file called key.txt with vim. The file should only contain the text “May I come in?”

If you attempt to check the status of the branch, the changes we made do not appear to be reflected. This can be checked using the git status command:

`$git status`

`On branch master`

`Your branch is up-to-date with 'origin/master'.`

`nothing to commit, working tree clean`

One possible explaination is that this file extention (.txt) has been enlisted in .gitignore file. Checking the file reveals this is true:

`$ cat .gitignore`

`*.txt`

Simply removing that line from the file will do the trick.

Now, lets commit our changes and push to the master branch with the following commands (output excluded here):

`$ git add .`

`$ git commit -m “adding file”`

`$ git push`

The output text from the last command contains the password to the next level: 56a9bf19c63d650ce78e6ec0354ee45e

# Level 32 --> Level 33

_After all this git stuff its time for another escape. Good luck!_

This level seems to be running a binary that converts all input commands to uppercase, rendering them useless. The hint for this level suggests looking at the man page of “sh”. Since any commands containing alphabets won’t be useful here, we pay attention to the section on special parameters.
We can invoke the bash shell using $0, where 0 “expands to the name of the shell or shell script” (source: linux man pages).

`>> $0`

`$ cat /etc/bandit_pass/bandit33`

c9c3199ddf4121b10cf581a98d51caee


