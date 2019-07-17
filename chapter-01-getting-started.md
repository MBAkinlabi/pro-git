# Chapter 1: Getting Started
- Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates.
```bash   
git config
```
- The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information.
```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```
You need to do this only once if you pass the `--global` option. 

- If you want to override this with a different name or email address for specific projects, you can run the command without the --global option when you’re in that project.
- You can configure the default text editor that will be used when Git needs you to type in a message. If not configured, Git uses your system’s default editor.
- If you want to use a different text editor, such as Emacs, you can do the following:
```bash
$ git config --global core.editor emacs
```
- On a Windows system, if you want to use a different text editor, you must specify the full path to its executable file.
- If you are on a 32-bit Windows system, or you have a 64-bit editor on a 64-bit system, you’ll type something like this:
```bash
$ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe'
-multiInst -nosession"
```
- If you have a 32-bit editor on a 64-bit system, the program will be installed in C:\Program Files(x86):
```bash
$ git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe'
    -multiInst -nosession"
```
- If you want to check your configuration settings, you can use the git config --list command to list all the settings Git can find at that point:
```bash
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```
- You can also check what Git thinks a specific key’s value is by typing git config <key>:
```bash
$ git config user.name
John Doe
```
- There are two ways to get help in Git:
```bash
$ git help <verb>
$ man git-<verb>
```
- For example, you can get the manpage help for the git config command by running:
```bash
$ git help config
```
- If you don’t need the full-blown manpage help, but just need a quick refresher on the available options for a Git command, you can ask for the more concise “help” output with the -h or--help options, as in:
```bash
$ git add -h
usage: git add [<options>] [--] <pathspec>...
    -n, --dry-run dry run
    -v, --verbose be verbose
    -i, --interactive interactive picking
    -p, --patch select hunks interactively
    -e, --edit edit current diff and apply
    -f, --force allow adding otherwise ignored files
    -u, --update update tracked files
    -N, --intent-to-add record only the fact that the path will be added later
    -A, --all add changes from all tracked and untracked files
    --ignore-removal ignore paths removed in the working tree (same as --no-all)
    --refresh don\'t add, only refresh the index
    --ignore-errors just skip files which cannot be added because of errors
    --ignore-missing check if - even missing - files are ignored in dry run
    --chmod <(+/-)x> override the executable bit of the listed files
```