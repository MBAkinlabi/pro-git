# Chapter 2: Git Basics

## Getting a Git Repository

### Initializing a Repository in an Existing Directory

- If you have a project directory that is currently not under version control and you want to start controlling it with Git, you first need to go to that project’s directory.
- And type this to initialize Git:
```bash
$ git init
```
- This creates a new subdirectory named .git that contains all of your necessary repository files — a Git repository skeleton. At this point, nothing in your project is tracked yet.
- If you want to start tracking specific files, you can accomplish that with a few `git add` commands that specify the files you want to track, followed by a `git commit`:
```bash
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```
- We'll talk more about the above shell commands later.

### Cloning an Existing Repository

- If you want to get a copy of an existing Git repository — for example, a project you’d like to contribute to — the command you need is `git clone`.
- You clone a repository with `git clone <url>`. For example, if you want to clone the Git linkable library called `libgit2`, you can do so like this:
```bash
$ git clone https://github.com/libgit2/libgit2
```
- That creates a directory named `libgit2`, initializes a `.git` directory inside it, pulls down all the data for that repository, and checks out a working copy of the latest version.
- If you want to clone the repository into a directory named something other than libgit2, you can specify that as the next command-line option:
```bash
$ git clone https://github.com/libgit2/libgit2 mylibgit
```
- That command does the same thing as the previous one, but the target directory is called `mylibgit`.

## Recording Changes to the Repository

### Checking the Status of Your Files

- Remember that each file in your working directory can be in one of two states: tracked or untracked. Tracked files are files that were in the last snapshot; they can be unmodified, modified,or staged. In short, tracked files are files that Git knows about
- The main tool you use to determine which files are in which state is the `git status` command. If you run this command directly after a clone, you should see something like this:
```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```
- Let’s say you add a new file to your project, a simple README file. If the file didn’t exist before, and you run `git status`, you see your untracked file like so:
```bash
$ echo 'My Project' > README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:
    (use "git add <file>..." to include in what will be committed)
    
    README

nothing added to commit but untracked files present (use "git add" to track)
```
- You can see that your new README file is untracked, because it’s under the “Untracked files” heading in your status output.

### Tracking New Files

- In order to begin tracking a new file, you use the command `git add`. To begin tracking the README file, you can run this: (**NOTE: TO ADD ALL FILES USE THE `git add .` command to add all files. And learn more from the How to Use Git and GitHub course)**
```bash
$ git add README
```
- If you run your `git status` command again, you can see that your README file is now tracked and staged to be committed:
```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)
    
    new file: README
```
- You can tell that it’s staged because it’s under the “Changes to be committed” heading
- The `git add` command takes a pathname for either a file or a directory; if it’s a directory, the command adds all the files in that directory recursively.

### Staging Modified Files

- If you change a previously tracked file called CONTRIBUTING.md and then run your `git status` command again, you get something that looks like this:
```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)
    
    new file: README

Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

    modified: CONTRIBUTING.md
```
### Short Status

- While the git status output is pretty comprehensive, it’s also quite wordy. If you run `git status -s` or `git status --short` you get a far more simplified output from the command:
```bash
$ git status -s
    M README
MM Rakefile
A lib/git.rb
M lib/simplegit.rb
?? LICENSE.txt
```
### Ignoring Files

- Often, you’ll have a class of files that you don’t want Git to automatically add or even show you as being untracked.
- These are generally automatically generated files such as log files or files produced by your build system. In such cases, you can create a file listing patterns to match them named `.gitignore`. Here is an example `.gitignore` file:
- **The code that was supposed to appear here isn't clear. It doesn't explain much information. You can learn more about .gitignore in the How to Use Git and GitHub Course that is inside the Notion Desktop which you learned on YouTube.**
- GitHub maintains a fairly comprehensive list of good `.gitignore` file examples for dozens of projects and languages at [https://github.com/github/gitignore](https://github.com/github/gitignore) if you want a starting point for your project.
- **NOTE:** In the simple case, a repository might have a single `.gitignore` file in its root directory, which applies recursively to the entire repository. However, it is also possible to have additional `.gitignore` files in subdirectories. The rules in these nested `.gitignore` files apply only to the files under the directory where they are located. **(The Linux kernel source repository has 206 `.gitignore` files.)**

### Viewing Your Staged and Unstaged Changes

- If the git status command is too vague for you — you want to know exactly what you changed, not just which files were changed — you can use the `git diff` command.
- `git diff` shows you the exact lines added and removed — the patch, as it were.
- To see what you’ve changed but not yet staged, type `git diff` with no other arguments:
```bash
$ git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8ebb991..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -65,7 +65,8 @@ branch directly, things can get messy.
    Please include a nice description of your changes when you submit your PR;
    if we have to read the whole diff to figure out why you\'re contributing
    in the first place, you\'re less likely to get feedback and have your change
    -merged in.
+merged in. Also, split your changes into comprehensive chunks if your patch is
+longer than a dozen lines.

If you are starting to work on a particular area, feel free to submit a PR
that highlights your work in progress (and note in the PR title that it\'s
```
- That command compares what is in your working directory with what is in your staging area. The result tells you the changes you’ve made that you haven’t yet staged.
- If you want to see what you’ve staged that will go into your next commit, you can use `git diff--staged`. This command compares your staged changes to your last commit:
```bash
$ git diff --staged
diff --git a/README b/README
new file mode 100644
index 0000000..03902a1
--- /dev/null
+++ b/README
@@ -0,0 +1 @@
+My Project
```
- Use git `diff --cached` to see what you’ve staged so far `(--staged` and `--cached` are synonyms):

### Committing Your Changes

- Now that your staging area is set up the way you want it, you can commit your changes.
- Any files you have created or modified that you haven’t run `git add` on since you edited them — won’t go into this commit.
- The simplest way to commit is to type `git commit`:
```bash
$ git commit
```
- Doing so launches your editor of choice.
```bash
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Your branch is up-to-date with 'origin/master'.
#
# Changes to be committed:
    # new file: README
    # modified: CONTRIBUTING.md
# 
~ 
~ 
~
".git/COMMIT_EDITMSG" 9L, 283C
    
```
- You can see that the default commit message contains the latest output of the `git status` command commented out and one empty line on top. You can remove these comments and type your commit message, or you can leave them there to help you remember what you’re committing.
- Alternatively, you can type your commit message inline with the `commit` command by specifying it after a `-m` flag, like this:
```bash
$ git commit -m "Story 182: Fix benchmarks for speed"
[master 463dc4f] Story 182: Fix benchmarks for speed
 2 files changed, 2 insertions(+)
 create mode 100644 README
```
### Skipping The Staging Area

- If you want to skip the staging area, Git provides a simple shortcut. Adding the `-a` option to the `git commit` command makes Git automatically stage every file that is already tracked before doing the commit, letting you skip the `git add` part:
```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    
    modified: CONTRIBUTING.md

no changes added to commit (use "git add" and/or "git commit -a")
$ git commit -a -m 'added new benchmarks'
[master 83e38c7] added new benchmarks
 1 file changed, 5 insertions(+), 0 deletions(-)
```
- Notice how you don’t have to run `git add` on the CONTRIBUTING.md file in this case before you commit. That’s because the `-a` flag includes all changed files. **This is convenient, but be careful; sometimes this flag will cause you to include unwanted changes.**

### Removing Files

- The `git rm` command does that, and also removes the file from your working directory so you don’t see it as an untracked file the next time around.
- If you simply remove the file from your working directory, it shows up under the “Changes notstaged for commit” (that is, unstaged) area of your `git status` output:
```bash
$ rm PROJECTS.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    
    deleted: PROJECTS.md

no changes added to commit (use "git add" and/or "git commit -a")
```
- Then, if you run `git rm`, it stages the file’s removal:
```bash
$ git rm PROJECTS.md
rm 'PROJECTS.md'
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    
    deleted: PROJECTS.md
```
- The next time you commit, the file will be gone and no longer tracked. If you modified the file and added it to the staging area already, you must force the removal with the `-f` option. This is a safety feature to prevent accidental removal of data that hasn’t yet been recorded in a snapshot and that can’t be recovered from Git.
- Another useful thing you may want to do is to keep the file in your working tree but remove it from your staging area.
- This is particularly useful if you forgot to add something to your `.gitignore` file and accidentally staged it, like a large log file or a bunch of .a compiled files. To do this, use the `--cached` option:
```bash
$ git rm --cached README
```
- You can pass files, directories, and file-glob patterns to the `git rm` command. That means you can do things such as:
```bash
$ git rm log/\*.log
```
- Note the backslash (`\`) in front of the `*.` This is necessary because Git does its own file name expansion in addition to your shell’s filename expansion. This command removes all files that have the `.log` extension in the `log/` directory. Or, you can do something like this:
```bash
$ git rm \*~
```
- This command removes all files whose names end with a `~`

### Moving Files

- Git has a `mv` command. If you want to rename a file in Git, you can run something like:
```bash
$ git mv file_from file_to
```
- In fact, if you run something like this and look at the status, you’ll see that Git considers it a renamed file:
```bash
$ git mv README.md README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    
    renamed: README.md -> README
```
- This is equivalent to running something like this:
```bash
$ mv README.md README
$ git rm README.md
$ git add README
```
## Viewing the Commit History

- After you have created several commits, or if you have cloned a repository with an existing commit history, you’ll probably want to look back to see what has happened.
- The most basic and powerful tool to do this is the `git log` command.
- These examples use a very simple project called “simplegit”. To get the project, run:
```bash
$ git clone https://github.com/schacon/simplegit-progit
```

- When you run `git log` in this project, you should get output that looks something like this:
```bash
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date: Mon Mar 17 21:52:11 2008 -0700

    changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date: Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date: Sat Mar 15 10:31:28 2008 -0700

    first commit
```
One of the more helpful options is `-p` or `--patch`, which shows the difference (the patch output) introduced in each commit. You can also limit the number of log entries displayed, such as using `-2` to show only the last two entries.
```bash
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date: Mon Mar 17 21:52:11 2008 -0700
    
    changed the version number

diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
    s.platform = Gem::Platform::RUBY
    s.name = "simplegit"
-   s.version = "0.1.0"
+   s.version = "0.1.1"
    s.author = "Scott Chacon"
    s.email = "schacon@gee-mail.com"
    s.summary = "A simple gem for using Git in Ruby code."

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date: Sat Mar 15 16:40:33 2008 -0700
    
    removed unnecessary test

diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit
    end

 end
-
-if $0 == __FILE__
- git = SimpleGit.new
- puts git.show
-end
```
- If you want to see some abbreviated stats for each commit, you can use the `--stat` option:
```bash
$ git log --stat
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date: Mon Mar 17 21:52:11 2008 -0700
    
    changed the version number
 Rakefile | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date: Sat Mar 15 16:40:33 2008 -0700
    
    removed unnecessary test

 lib/simplegit.rb | 5 -----
 1 file changed, 5 deletions(-)

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date: Sat Mar 15 10:31:28 2008 -0700
    
    first commit

 README | 6 ++++++
 Rakefile | 23 +++++++++++++++++++++++
 lib/simplegit.rb | 25 +++++++++++++++++++++++++
 3 files changed, 54 insertions(+)
```
- As you can see, the `--stat` option prints below each commit entry a list of modified files, how many files were changed, and how many lines in those files were added and removed. It also puts a summary of the information at the end.
- Another really useful option is `--pretty`. This option changes the log output to formats other than the default.
- The `oneline` option prints each commit on a single line, which is useful if you’re looking at a lot of commits. In addition, the `short`, `full`, and `fuller` options show the output in roughly the same format but with less or more information, respectively:
```bash
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 changed the version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
```
- The most interesting option is format, which allows you to specify your own log output format. This is especially useful when you’re generating output for machine parsing — because you specify the format explicitly, you know it won’t change with updates to Git:
```bash
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : changed the version number
085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
a11bef0 - Scott Chacon, 6 years ago : first commit
```
- Useful options for `git log --pretty=format` lists some of the more useful options that format takes.

#### Table 1. Useful options for git log --pretty=format
| Option      | Description of Output                               |
|------------ | --------------------                                |
| %H          | Commit hash                                         |
| %h          | Abbreviated commit hash                             |
| %T          | Tree hash                                           |
| %t          | Abbreviated tree hash                               |
| %P          | Parent hashes                                       |
| %p          | Abbreviated parent hashes                           |
| %an         | Author name                                         |
| %ae         | Author email                                        |
| %ad         | Author date (format respects the --date=option)     |
| %ar         | Author date, relative                               |
| %cn         | Committer name                                      |
| %ce         | Committer email                                     |
| %cd         | Committer date                                      |
| %cr         | Committer date, relative                            |
| %s          | Subject                                             |

- The `oneline` and `format` options are particularly useful with another `log` option called `--graph`. This option adds a nice little ASCII graph showing your branch and merge history:
```bash
$ git log --pretty=format:"%h %s" --graph
* 2d3acf9 ignore errors from SIGCHLD on trap
* 5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
|\
| * 420eac9 Added a method for getting the current branch.
* | 30e367c timeout code and tests
* | 5a09431 add timeout protection to grit
* | e1193f8 support for heads with slashes in them
|/
* d6016bc require time for xmlschema
* 11d191e Merge branch 'defunkt' into local
```
- Those are only some simple output-formatting options to `git log` — there are many more. Common options to `git log` lists the options we’ve covered so far, as well as some other common formatting options that may be useful, along with how they change the output of the log command.

#### Table 2. Common options to git log

| Option               | Description
|--------------------- |---------------------------------------------------------------------------------------------|
| -p                   | Show the patch introduced with each commit.                                                 |
| --stat               | Show statistics for files modified in each commit.                                          |
| --shortstat          | Display only the changed/insertions/deletions line from the --stat command.                 |
| --name-only          | Show the list of files modified after the commit information.                               |
| --name-status        | Show the list of files affected with added/modified/deleted information as well.            |
| --abbrev-commit      | Show only the first few characters of the SHA-1 checksum instead of all 40.                 |
| --relative-date      | Display the date in a relative format (for example, “2 weeks ago”) instead of using the full|  |                      | date format.                                                                                |  
| --graph              | Display an ASCII graph of the branch and merge history beside the log output.               |
| --pretty             | Show commits in an alternate format. Options include oneline, short, full, fuller, and      |  |                      | format (where you specify your own format).                                                 |
| --oneline            | Shorthand for --pretty=oneline --abbrev-commit used together.                               |

### Limiting Log Output

- In addition to output-formatting options, git log takes a number of useful limiting options — that is, options that let you show only a subset of commits.
- You’ve seen one such option already — the `-2` option, which displays only the last two commits. In fact, you can do `-<n>`, where `n` is any integer to show the last `n` commits. In reality, you’re unlikely to use that often, because Git by default pipes all output through a pager so you see only one page of log output at a time.
- However, the time-limiting options such as `--since` and `--until` are very useful. For example, this command gets the list of commits made in the last two weeks:
```bash
$ git log --since=2.weeks
```
- This command works with lots of formats — you can specify a specific date like `"2008-01-15"`, or a relative date such as `"2 years 1 day 3 minutes ago"`.

#### Table 3. Options to limit the output of git log

| Option                | Description
|---------------------- | -----------------------------------------------------------   |
| -<n>                  | Show only the last n commits                                  |
| --since, --after      | Limit the commits to those made after the specified date.     |
| --until, --before     | Limit the commits to those made before the specified date.    |
| --author              | Only show commits in which the author entry matches the       |
|                       | specified string.                                             |
| --committer           | Only show commits in which the committer entry matches the    |
|                       | specified string.                                             |
| --grep                | Only show commits with a commit message containing the string |
| -S                    | Only show commits adding or removing code matching the string |
```bash
$ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \
    --before="2008-11-01" --no-merges -- t/
5610e3b - Fix testcase failure when extended attributes are in use
acd3b9e - Enhance hold_lock_file_for_{update,append}() API
f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
51a94af - Fix "checkout --track -b newbranch" on detached HEAD
b0ad11e - pull: allow "git pull origin $something:$current_branch" into an unborn
branch
```
- Depending on the workflow used in your repository, it’s possible that a sizable percentage of the commits in your log history are just merge commits, which typically aren’t very informative. To prevent the display of merge commits cluttering up your log history, simply add the log option `--no-merges`.

## Undoing Things

- One of the common undos takes place when you commit too early and possibly forget to add some files, or you mess up your commit message.
- If you want to redo that commit, make the additional changes you forgot, stage them, and commit again using the --amend option:
```bash
$ git commit --amend
```
- If you commit and then realize you forgot to stage the changes in a file you wantedto add to this commit, you can do something like this:
```bash
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```
### Unstaging a Staged File

- Let’s say you’ve changed two files and want to commit them as two separate changes, but you accidentally type `git add *` and stage them both.
- How can you unstage one of the two? The `git status` command reminds you:
```bash
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    
    renamed: README.md -> README
    modified: CONTRIBUTING.md
```
- Right below the “Changes to be committed” text, it says use `git reset HEAD <file>`... to unstage. So,let’s use that advice to unstage the `CONTRIBUTING.md` file:
```bash
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed: README.md -> README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    
    modified: CONTRIBUTING.md
```
- The command is a bit strange, but it works. The CONTRIBUTING.md file is modified but once again unstaged.

### Unmodifying a Modified File

- What if you realize that you don’t want to keep your changes to the CONTRIBUTING.md file? How can you easily unmodify it?
- Luckily, `git status` tells you how to do that, too. In the last example output, the unstaged area looks like this:
```bash
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    
    modified: CONTRIBUTING.md
```
- It tells you pretty explicitly how to discard the changes you’ve made. Let’s do what it says:
```bash
$ git checkout -- CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    
    renamed: README.md -> README
```
- **IMPORTANT:** It’s important to understand that `git checkout -- <file>` is a dangerous command. Any changes you made to that file are gone — Git just copiedCanother file over it. Don’t ever use this command unless you absolutely know that you don’t want the file.

## Working with Remotes

- To see which remote servers you have configured, you can run the `git remote` command. It lists the shortnames of each remote handle you’ve specified. If you’ve cloned your repository, you should at least see `origin` — that is the default name Git gives to the server you cloned from:
```bash
$ git clone https://github.com/schacon/ticgit
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.
$ cd ticgit
$ git remote
origin
```
- You can also specify `-v`, which shows you the URLs that Git has stored for the shortname to be used when reading and writing to that remote:
```bash
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
```
- If you have more than one remote, the command lists them all. For example, a repository with multiple remotes for working with several collaborators might look something like this.
```bash
$ cd grit
$ git remote -v
bakkdoor https://github.com/bakkdoor/grit (fetch)
bakkdoor https://github.com/bakkdoor/grit (push)
cho45 https://github.com/cho45/grit (fetch)
cho45 https://github.com/cho45/grit (push)
defunkt https://github.com/defunkt/grit (fetch)
defunkt https://github.com/defunkt/grit (push)
koke git://github.com/koke/grit.git (fetch)
koke git://github.com/koke/grit.git (push)
origin git@github.com:mojombo/grit.git (fetch)
origin git@github.com:mojombo/grit.git (push)
```
### Adding Remote Repositories

- We’ve mentioned and given some demonstrations of how the `git clone` command implicitly adds the `origin` remote for you. Here’s how to add a new remote explicitly. To add a new remote Git repository as a shortname you can reference easily, run `git remote add <shortname> <url>`:
```bash
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
pb https://github.com/paulboone/ticgit (fetch)
pb https://github.com/paulboone/ticgit (push)
```
- if you want to fetch all the information that Paul has but that you don’t yet have in your repository, you can run `git fetch pb`:
```bash
$ git fetch pb
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 43 (delta 10), reused 31 (delta 5)
Unpacking objects: 100% (43/43), done.
From https://github.com/paulboone/ticgit
  * [new branch] master -> pb/master
  * [new branch] ticgit -> pb/ticgit
```
- Paul’s master branch is now accessible locally as `pb/master` — you can merge it into one of your branches, or you can check out a local branch at that point if you want to inspect it.

### Fetching and Pulling from Your Remotes

- Get data from your remote projects, you can run:
```bash
$ git fetch <remote>
```
- The command goes out to that remote project and pulls down all the data from that remote project that you don’t have yet.
- So, `git fetch origin` fetches any new work that has been pushed to that server since you cloned (or last fetched from) it.
- It’s important to note that the `git fetch` command only downloads the data to your local repository — it doesn’t automatically merge it with any of your work or modify what you’re currently working on. You have to merge it manually into your work when you’re ready.
- You can use the `git pull` command to automatically fetch and then merge that remote branch into your current branch.

### Pushing to Your Remotes

- When you have your project at a point that you want to share, you have to push it upstream. The command for this is simple: `git push <remote> <branch>`.
```bash
$ git push origin master
```
- **This command works only if you cloned from a server to which you have write access and if nobody has pushed in the meantime. If you and someone else clone at the same time and they push upstream and then you push upstream, your push will rightly be rejected. You’ll have to fetch their work first and incorporate it into yours before you’ll be allowed to push.**
- If you want to see more information about a particular remote, you can use the `git remote show <remote>` command. If you run this command with a particular shortname, such as origin, you get something like this:
```bash
$ git remote show origin
* remote origin
    Fetch URL: https://github.com/schacon/ticgit
    Push URL: https://github.com/schacon/ticgit
    HEAD branch: master
    Remote branches:
     master tracked
     dev-branch tracked
    Local branch configured for 'git pull':
     master merges with remote master
    Local ref configured for 'git push':
     master pushes to master (up to date)
```
### Renaming and Removing Remotes

- If you want to rename pb to paul, you can do so with `git remote rename`:
```bash
$ git remote rename pb paul
$ git remote
origin
paul
```
- This changes all your remote-tracking branch names, too. What used to be referenced at `pb/master` is now at `paul/master`.
- If you want to remove a remote for some reason, you can either use `git remote remove` or `git remote rm`:
```bash
$ git remote remove paul
$ git remote
origin
```
- Once you delete the reference to a remote this way, all remote-tracking branches and configuration settings associated with that remote are also deleted.

## Tagging

- Git has the ability to tag specific points in history as being important. **Typically people use this functionality to mark release points (v1.0, and so on).** In this section, you’ll learn how to list the available tags, how to create new tags, and what the different types of tags are.

### Listing Your Tags

- Listing the available tags in Git is straightforward. Just type `git tag` (with optional `-l` or `--list`):
```bash
$ git tag
v0.1
v1.3
```
- **This command lists the tags in alphabetical order; the order in which they appear has no real importance.**
- You can also search for tags that match a particular pattern. The Git source repo, for instance, contains more than 500 tags. If you’re only interested in looking at the 1.8.5 series, you can run this:
```bash
$ git tag -l "v1.8.5*"
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
v1.8.5-rc2
v1.8.5-rc3
v1.8.5.1
v1.8.5.2
v1.8.5.3
v1.8.5.4
v1.8.5.5
```
### Creating Tags

- Git supports two types of tags: ***lightweight*** and ***annotated***
- ***A lightweight tag*** is very much like a branch that doesn’t change — it’s just a pointer to a specific commit.
- ***Annotated tags***, however, are stored as full objects in the Git database. They’re checksummed; contain the tagger name, email, and date; have a tagging message; and can be signed and verified with GNU Privacy Guard (GPG). It’s generally recommended that you create annotated tags so you can have all this information; but if you want a temporary tag or for some reason don’t want to keep the other information, lightweight tags are available too.

### Annotated Tags

- Creating an annotated tag in Git is simple. The easiest way is to specify `-a` when you run the tag command. The `-m` specifies a tagging message, which is stored with the tag.
```bash
$ git tag -a v1.4 -m "my version 1.4"
$ git tag
v0.1
v1.3
v1.4
```
- You can see the tag data along with the commit that was tagged by using the `git show` command:
```bash
$ git show v1.4
tag v1.4
Tagger: Ben Straub <ben@straub.cc>
Date: Sat May 3 20:19:12 2014 -0700

my version 1.4

commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date: Mon Mar 17 21:52:11 2008 -0700

    changed the version number
```
### Lightweight Tags

- Another way to tag commits is with a lightweight tag. This is basically the commit checksum stored in a file — no other information is kept. To create a lightweight tag, don’t supply any of the `-a`, `-s`, or `-m` options, just provide a tag name:
```bash
$ git tag v1.4-lw
$ git tag
v0.1
v1.3
v1.4
v1.4-lw
v1.5
```
- This time, if you run `git show` on the tag, you don’t see the extra tag information. The command just shows the commit:
```bash
$ git show v1.4-lw
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date: Mon Mar 17 21:52:11 2008 -0700

    changed the version number
```
### Tagging Later

- You can also tag commits after you’ve moved past them. Suppose your commit history looks like this:
```bash
$ git log --pretty=oneline
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
4682c3261057305bdd616e23b64b0857d832627b added a todo file
166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme
```
- Now, suppose you forgot to tag the project at v1.2, which was at the “updated rakefile” commit. You can add it after the fact. To tag that commit, you specify the commit checksum (or part of it) at the end of the command:
```bash
$ git tag -a v1.2 9fceb02
```
### Sharing Tags

- By default, the `git push` command doesn’t transfer tags to remote servers. You will have to explicitly push tags to a shared server after you have created them. This process is just like sharing remote branches — you can run `git push origin <tagname>`.
```bash
$ git push origin v1.5
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
Total 14 (delta 3), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
  * [new tag] v1.5 -> v1.5
```
- If you have a lot of tags that you want to push up at once, you can also use the `--tags` option to the `git push` command. This will transfer all of your tags to the remote server that are not already there.
```bash
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
* [new tag] v1.4 -> v1.4
  * [new tag] v1.4-lw -> v1.4-lw
```
- Now, when someone else clones or pulls from your repository, they will get all your tags as well.

### Checking out Tags

- If you want to view the versions of files a tag is pointing to, you can do a git checkout, though this puts your repository in “detached HEAD” state, which has some ill side effects:
```bash
$ git checkout 2.0.0
Note: checking out '2.0.0'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.
If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

    git checkout -b <new-branch>

HEAD is now at 99ada87... Merge pull request #89 from schacon/appendix-final

$ git checkout 2.0-beta-0.1
Previous HEAD position was 99ada87... Merge pull request #89 from schacon/appendix-final
HEAD is now at df3f601... add atlas.json and cover image
```
- In “detached HEAD” state, if you make changes and then create a commit, the tag will stay the same, but your new commit won’t belong to any branch and will be unreachable, except for by the exact commit hash. Thus, if you need to make changes — say you’re fixing a bug on an older version, for instance — you will generally want to create a branch:
```bash
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```
- If you do this and make a commit, your version2 branch will be slightly different than your v2.0.0 tag since it will move forward with your new changes, so do be careful.

## Git Aliases

- If you don’t want to type the entire text of each of the Git commands, you can easily set up an alias for each command using `git config`. Here are a couple of examples you may want to set up:
```bash
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```
- This means that, for example, instead of typing `git commit`, you just need to type `git ci`.
- This technique can also be very useful in creating commands that you think should exist. For example, to correct the usability problem you encountered with unstaging a file, you can add your own unstage alias to Git:
```bash
$ git config --global alias.unstage 'reset HEAD --'
```
- This makes the following two commands equivalent:
```bash
$ git unstage fileA
$ git reset HEAD -- fileA
```
- This seems a bit clearer. It’s also common to add a `last` command, like this:
```bash
$ git config --global alias.last 'log -1 HEAD'
```
- This way, you can see the last commit easily:
```bash
$ git last
commit 66938dae3329c7aebe598c2246a8e6af90d04646
Author: Josh Goebel <dreamer3@example.com>
Date: Tue Aug 26 19:48:51 2008 +0800
    
    test for current head

Signed-off-by: Scott Chacon <schacon@example.com>
```
- As you can tell, Git simply replaces the new command with whatever you alias it for. However, maybe you want to run an external command, rather than a Git subcommand. In that case, you start the command with a `!` character. This is useful if you write your own tools that work with a Git repository. We can demonstrate by aliasing `git visual` to run `gitk`:
```bash
$ git config --global alias.visual '!gitk'
```