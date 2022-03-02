# A tutorial introduction to Git
Adapted from: https://git-scm.com/docs/gittutorial
## Description
 This tutorial explains how to import a new project into Git, make changes to it, and share changes with other developers.

It is a good idea to introduce yourself to Git with your name and public email address before doing any operation. 

The easiest way to do so is:

```
$ git config --global user.name "Your Name Comes Here"
$ git config --global user.email you@yourdomain.example.com
```

## Starting a new project
If you have files in your current location you can place them under version control using the command:

```
git init
```

Git should reply

```
Initialized empty Git repository in .git/
```

You’ve now initialized the working directory—​you may notice a new directory created, named ".git".

Next, tell Git to take a snapshot of the contents of all files under the current directory (note the .), with git add:

```
$ git add .
```
This snapshot is now stored in a temporary staging area which Git calls the "index". You can permanently store the contents of the index in the repository with git commit:

```
git commit -m "Add a message here!"
```
You’ve now stored the first version of your project in Git!

## Making Changes
Modify some files, then add their updated contents to the index:

```
$ git add file1 file2 file3
```
You are now ready to commit. You can see what is about to be committed using git diff with the --cached option:
```
$ git diff --cached file1 file2 file3
```

Without --cached, git diff will show you any changes that you’ve made but not yet added to the index.

You can also get a brief summary of the situation with git status:

```
$ git status
```

Example output:
```
On branch master
Changes to be committed:
Your branch is up to date with 'origin/master'.
  (use "git restore --staged <file>..." to unstage)

	modified:   file1
	modified:   file2
	modified:   file3
```

If you need to make any further adjustments, do so now, and then add any newly modified content to the index.

Finally, commit your changes with:

```
git commit -m "Add a message here!"
```
Alternatively, instead of running git add beforehand, you can use

```
$ git commit -a
```
which will automatically notice any modified (but not new) files, add them to the index, and commit, all in one step.

## Git tracks content not files
Many revision control systems provide an add command that tells the system to start tracking changes to a new file. Git’s add command does something simpler and more powerful: git add is used both for new and newly modified files, and in both cases it takes a snapshot of the given files and stages that content in the index, ready for inclusion in the next commit.

## Viewing project history
At any point you can view the history of your changes using

```
$ git log
```
If you also want to see complete diffs at each step, use

```
$ git log -p
```

Often the overview of the change is useful to get a feel of each step

```
$ git log --stat --summary
```

## Managing branches
A single Git repository can maintain multiple branches of development. To create a new branch named "experimental", use
```
$ git branch experimental
```
If you now run
```
$ git branch
```
you’ll get a list of all existing branches:

```
  experimental
* master
```

The "experimental" branch is the one you just created, and the "master" branch is a default branch that was created for you automatically. The asterisk marks the branch you are currently on; type

```
$ git switch experimental
```

to switch to the experimental branch. Now edit a file, commit the change, and switch back to the master branch:

```
(edit file)
$ git commit -a
$ git switch master
```

Check that the change you made is no longer visible, since it was made on the experimental branch and you’re back on the master branch.

You can make a different change on the master branch:

```
(edit file)
$ git commit -a
```

at this point the two branches have diverged, with different changes made in each. To merge the changes made in experimental into master, run

```
$ git merge experimental
```

If the changes don’t conflict, you’re done. If there are conflicts, markers will be left in the problematic files showing the conflict;

```
$ git diff
```
will show this. Once you’ve edited the files to resolve the conflicts,
```
$ git commit -a -m "Message here!"
```

At this point you could delete the experimental branch with

```
$ git branch -d experimental
```
This command ensures that the changes in the experimental branch are already in the current branch.

If you develop on a branch crazy-idea, then regret it, you can always delete the branch with
```
$ git branch -D crazy-idea
```
Branches are cheap and easy, so this is a good way to try something out.

## Using Git for collaboration
Suppose that a colleague has started a new project with a Git repository available on GitHub at https://github.com/IoC-Sunderland/my_new_app.git

You want to collaborate so start by cloning the repo to your machine:
```
$ git clone https://github.com/IoC-Sunderland/my_new_app.git my_new_app
```
This clones the repo into a new folder called ***my_new_app***

The clone is on an equal footing with the original project, possessing its own copy of the original project’s history.

```
(edit files)
$ git commit -a -m "I changed some stuff!"
(repeat as necessary)
```
When ready, you can tell other developers to pull changes from the repository at https://github.com/IoC-Sunderland/my_new_app.git.

```
$ git pull https://github.com/IoC-Sunderland/my_new_app.git master
```

This merges the changes from the "master" branch (more recently referred to as "main") into the other developer's current branch. If that developer has made her own changes in the meantime, they may need to manually fix any conflicts.

The "pull" command thus performs two operations: it fetches changes from a remote branch, then merges them into the current branch.

Note that in general, the developer would want their local changes committed before initiating this "pull".

 If your work conflicts with what they did since their histories forked, they will use their working tree and the index to resolve conflicts, and existing local changes will interfere with the conflict resolution process (Git will still perform the fetch but will refuse to merge — they will have to get rid of their local changes in some way and pull again when this happens).

The devloper can peek at what you did without merging first, using the "fetch" command; this allows them to inspect what you did, using a special symbol "FETCH_HEAD", in order to determine if you have done anything worth pulling, like this:

```
$ git fetch https://github.com/IoC-Sunderland/my_new_app.git main
$ git log -p HEAD..FETCH_HEAD
```

## Exploring history
Git history is represented as a series of interrelated commits. We have already seen that the git log command can list those commits. Note that first line of each git log entry also gives a name for the commit:
```
$ git log
commit 35a963ecb7d436f7583eab622e921940547e62b5 (HEAD -> main
, origin/main, origin/HEAD)
Author: Institute of Coding (Sunderland) <gavin.mcclary@sunderland.ac.uk>
Date:   Wed Mar 2 15:15:43 2022 +0000

    Create main.py

commit b1e4ac3452b7c3ec0302709db214a911adbbc878
Author: Institute of Coding (Sunderland) <gavin.mcclary@sunderland.ac.uk>
Date:   Wed Mar 2 15:15:09 2022 +0000

    Create file1
```
We can give this name to git show to see the details about this commit.
```
$ git show b1e4ac3452b7c3ec0302709db214a911adbbc878
```
But there are other ways to refer to commits. You can use any initial part of the name that is long enough to uniquely identify the commit:
```
$ git show b1e4ac34	# the first few characters of the name are usually enough

$ git show HEAD		# the tip of the current branch
```

The git grep command can search for strings in any version of your project, so

```
$ git grep "print" b1e4ac34
```
searches for all occurrences of "hello" in commit b1e4ac34

If you leave out the commit name/sha, git grep will search any of the files it manages in your current directory. So
```
$ git grep git grep "print"
```
is a quick way to search just the files that are tracked by Git.
