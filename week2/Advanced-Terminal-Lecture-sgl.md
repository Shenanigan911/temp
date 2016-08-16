![alt tag](http://www.refactoru.com/images/graphics/logo.png)
# Advanced Terminal

![alt tag](https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/DEC_VT100_terminal.jpg/220px-DEC_VT100_terminal.jpg)

## Terminal
Single text-based console
![alt tag](https://lh5.ggpht.com/WTLIqG6TOfjr0_G3HQ7nk6h6pmpynuyxYfpDlNsDcDdEzf31QU8E5f1OoRMY3TwK41U=h900)

## Shell
Multiple text-based consoles
![alt tag](http://prefetch.net/images/dvt.png)
Unix/Linux shell history: sh -> csh -> ksh -> bash (Born Again SHell)
## Windows
And the world of computers openned up to the non-Geeks of the world.


> ### Share a new workspace with the students (optional)
> Click on the Share button in the upper right corner of Cloud9
> Uncheck all boxes and one-by-one invite every student to share
>
> Have each student go to the same workspace URL, click on the Collaborate tab on the right side and click on the terminal window that you wish to share
>
> ### Record this lecture
> Save the input/output from the terminal session for notes.
>
> `script terminal.log`
>
> From here on, invite individual students to type the commands in the console window.

## New shell commands
### Echo
Echo prints its arguments to the console

`echo hello`

> What is the difference between an argument and a flag?

An argument is information used by the command.

`echo hello`

A flag modifies the behavior of a command and typically starts with a -.

`echo -n hello     # suppresses the trailing newline character`

### Variables
A variable stores a value which can be used as command line arguments.

```
greeting="hello bash"	# local variable
echo greeting 		    # prints 'greeting'
echo $greeting 		    # prints 'hello bash'
touch $greeting		    # creates files called 'hello' and 'bash'
touch '$greeting'       # creates a file called '$greeting'
touch "$greeting" 	    # creates a file called 'hello bash'
```

### Redirect output
We can use > to redirect STDOUT to a file or use >> to append it to a file from a command.
We can also use < to redirect STDIN from a file to a command.
We can use 2> and 2>> to redirect STDERR like STDOUT (STDOUT is technically 1>).

```
echo $greeting > hello-file         # send STDOUT to a file
valediction=”goodbye bash”	        # (valediction is the opposite of a greeting)
echo $valediction > bye-file	
cat hello-file > conversation-file  # effectively copies hello-file
cat conversation-file
```

> How would you append to the conversation file?

`cat bye-file >> conversation-file  # appends bye-file to conversation-file`

> And here is an example of redirecting STDIN.
`sort < conversation-file           # sorts the contents of conversation-file`

> How would you create an empty file without using touch?  (echo > filename)

```
echo > empty-file-echo  # send an empty line to a new file
touch empty-file-touch  # create a new empty file
```

> Do an `ls -l` on both files.

Wildcards
- * means "everything"
- ? means "any single character"

```
ls -l empty-file-*          # lists all files that begin with empty-file-
cat empty-file-echo
cat empty-file-touch
```

> Why are they different in size? 
> There is an empty line in echo file, but the touch file is completely empty.

## Less is More
> Let's see what's in that long file.

`cat long-file`

> Too much to see.  How do we look at the info? 
> How do we slow it down so we can see it all?

`more` buffers the output of a file.
Use `d` to move forward and `q` to quit.
`more` scrolls forward through the file.

`more long-file`

> Scroll through long-file to line 50.
> `more` can't scroll backwards.  Back in the 80's Mark Nudelman started an open source
> project to create a `more` that could scroll backwards.  It grew and grew in functionality
> until it was much larger and much more functional than `more`.  
>
> Less is More!

`less` buffers the output of a file, but you can move forward, backward and even side-to-side.
Use b  to move back, or use arrow keys to move up, down, left and right.
`less` is the same as `more` but with more!  

`less long-file`

> But what if we aren't looking at a file but want to look at a command's output?

## Pipe
> How do we look at a long directory listing without having to create (and then delete) a file?
> A pipe!

Pipes (`|`) send the STDOUT from one command to the STDIN of another command.

```
ls -l big-dir
ls -l big-dir | less
```

## Permissions
> Look at that output.  What does all of that mean?  Let's look into it.

`cat house-of-straw`

> I can't stand those nasty piggies.  Let's send in the wolf!

```
echo wolf > house-of-straw
cat house-of-straw
cat house-of-sticks
echo wolf > house-of-sticks
```

> Why didn’t that work?

`ls -al house-of-*`

`drwxrwxr-x 2  root  mail  4096  Aug 15  2016  mail`

- first letter is file type: d (directory), - (file) or l (link)
- letter triads are read, write, execute (rwx)
- triads are owner/user, group, other (ugo)
- next comes the file owner and group, then size, then last modified date then filename

> Touch a file.  What happens? 
>
> The last modified date changes.

```
touch house-of-straw
ls -l house-of-straw
```

### chmod (change mode)
`chmod` changes the mode of a file.
- Use `chmod` to add or remove read/write/execute (rwx) permissions to user/group/other/all (ugoa)
- chmod {ugoa}{+/-}{rwx} {files}
- chmod XXX {files}
    - rwx (4+2+1) octal representation of triad (ie. 764 would be user:rwx, group:rw, other:read)

- In Windows and MacOS, the file suffix is used to determine if file is executable.  
- In shell, anything can be executable.
- This is how you control access to and execution of a file.

> When would we change permissions?
>
> Secure Shell (ssh) allows for secure communication over an unsecured network.  
> To do this, it involve encrypting communications using a public key and a private key.
>
> .ssh requires directory to be 711 or 755 (only user can modify files), 
>
> public file to be 644 (anyone can read, but only user can modify)
>
> and the private key to be 600 (only user can read/modify, others can not even see it)

> Let's look back at that house of sticks

`ls -l house-of-sticks`

> We need to add write permissions to the file.

```
chmod u+w house-of-sticks    # add write permissions to the file
ls -l house-of-sticks       # check the new permissions
echo wolf > house-of-sticks # overwrite the file
```

> It worked!  
> By adding write access you could overwrite the file.

`cat house-of-bricks`

> We can't even see what's inside the file!  
> We have no read access.

`ls -l house-of-bricks`

> We need to add read permissions to the file to see what's inside.

```
chmod u+rw house-of-bricks
ls -l house-of-bricks
cat house-of-bricks
```

> *Don’t mess with Grandma!*

> What happens if we make a directory not executable?

```
chmod -x ..
cd ..
```

> We can't move into that directory!  Better fix it...

` chmod +x ..`

> Let's create a simple executable file.

```
cat hi
hi
```

> Why doesn’t it work? 
>
> We need to make it executable.

```
ls -l hi
chmod +x hi
ls -l hi
```

> Run `hi` again. Why doesn’t it work?

> We need to run ./hi because . is not in the PATH.
> PATH is a magic variable that lists all the directories which contain executable files that we might want to run from anywhere.
> We will look at that in a minute.

> Run `./hi`. Why does hi return `ubuntu`?  

> That is another magic variable that stores who you are.
> (Ubuntu is a flavor of Linux and Cloud9 creates a user named ubuntu its environment.)

`echo $USER`

> Where do these magic variables come from? 
> Our environment!

`env`

> See variables like `$PATH` and `$USER` in there?
>
> Why does `hi` return `$USER` but not `$greeting`? 
>
> Because `$greeting` is scoped as a local variable instead of an environment variable.

Environment variables like $PATH and $USER have global scope.

They can be used in any child process of a shell.

You must `export` a local variable in order to make it an environment variable.

```
export greeting=”howdy”     # create an environment variable
```

> Try to run ./hi one more time.

`./hi`

> It worked!

`unset` clears the value of a variable.

`unset greeting              # unsets an environment variable`

> How do we see what variables are defined in the environment?

`env` shows what variables are defined in the environment

`env                         # show environment variables`

> We just want to look for the value of `$USER`.  How do we filter all that output?

## GREP
`grep` (Global Regular Expression/Print) is a command to filter its input based on a regular expression.

` grep secret *          # This will show all lines in all files in the current directory (*) containing the word "secret"`

> You see an error message in the output of you application and need to determine
> where that message is coming from.  How might grep help you?

` grep -r secret *       # this will show all lines in all files in all subdirectories (recursively) containing the word secret`

> How might we use `grep` to filter the output of `env` to find the value of USER.
>
> Hint: How might we use a pipe with `env` and `grep`?

`env | grep USER    # take the output of env and only print lines containing the string "USER"`

> Key env variables include PATH, USER and HOME
>
> Look at PATH.  What are some of the directories that it uses?  What is in /bin for example?
> `ls /bin`

> How would you add . to your `PATH` so you don’t have to type dot in front `hi` to run it? 

```
export PATH=$PATH;.
hi
```

> Would there be a difference between adding a new directory to the beginning of the PATH or the end of the PATH?
>
> Yes!  The PATH is searched from the beginning of the list and the first command found matching the name is executed.

## Shell
The `sh` command creates a new shell which inherits the current environment.
The `quit` command will exit out of the child shell.

> Set a local and a global (environment) variable.

```
myLocalVar="I am local"
echo $myLocalVar
export myGlobalVar="I am global"
echo $myGLobalVar
```

> Then fire up a new shell.

```
sh
echo $myLocalVar
echo $myGLobalVar
quit
```

Don't exit out of your original shell or it will close the terminal!

## Alias
`alias` is a way of creating a shorthand command.
It is often used to abbreviate a command or command with common arguements.

```
alias ws=’pushd $HOME/workspace’
ws      # run the newly defined alias and pushd to the workspace
popd    # return to your original directory
```
EXAMPLE : typo ls to sl, subl='open -a "/Applications/Sublime Text.app'
function can { cp to bin, rm local, echo filename}

> Note that value is defined at the time that alias is created, not at the time the alias is run

`alias g=’git commit -m “quick commit”'`

> How would you create an alias to do a git commit and push?

`alias g=”cd $HOME/workspace; git add .; git commit -m \“quick commit\”;git push origin master;”`

> How would you write it to allow for a custom comment?

Aliases are only interpretted as commands - the first thing on the command line.

```
echo ws     # ws is not interpretted as an alias
ws          # ws is interpretted as an alias
pushd
```

> Can we create an alias to do git pushes with a runtime message?

```
alias g=”cd $HOME/workspace; git add .; git commit -m \“$*\”;git push origin master;”

```

> No!  
> The argument is evaluated when the alias is defined and the runtime arguments are just appended to the expanded alias.
>
> `alias g=”cd $HOME/workspace; git add .; git commit -m \“$*\”;git push origin master;”`
>
> when run as `g this is my comment` becomes
>
> `cd /home/users/ubuntu/workspace; git add .; git commit -m ""; git push origin master this is my comment`

Alias can’t take arguments.  It just expands on the command line.  We need a function to handle arguments.

## Function
A function can take arguments!
```
function gm { 
    cd $HOME/workspace
    git add .
    git commit -m “$*”
    git push origin master
}

gm this is my comment     # adds, commits and pushes with a runtime comment
```

> But we don't want to have to type these aliases and functions in every time we fire up a new shell terminal.

## .bashrc
.bashrc is a file that is run every time you start a new shell.  It initializes the state of the shell.
```
cd
less .bashrc
```

`.bashrc` can invoke other files which are then also processed at the same time.
This is done to organize what could otherwise become a very long, complicated script.
.bashrc vs .bash_profile, MAC vs. Linux

## Find
Find helps us locate files and directories with specific characteristics that we are looking for.

> How would we list all the files and directories from any given directory?

`find .`

> What if we need to find a file with a specific name (and we don't have Google)?

`find . -name "*hidden*"`

> What if we want to find all the secrets hidden inside all those files?

`find . -exec grep secret {} \;`

> That shows us the secrets, but not in which files they are hidden.

`find . -type -exec grep secret \; -exec echo {} \;`

## Sudo
- ADD to CHMOD section
- 

## Editors
In addition to `nano`, the bash shell has many editors.
`nano`, `ex`, `vi`, `vim`, `emacs`
alias to open GUI editor

And don't forget
`man` or `cmd -help`

> Terminate the script command and all the things we did have been captured in terminal.log)
> `exit`



ps
ps -A
sleep 1000&
ps -A
Show a real bash shell, ps -A
ps -A | grep sleep
kill XXXX
sudo sleep 1000&
ps -A | grep sleep
kill XXXX (fail)
sudo kill XXXX
sleep 1000&
ps -A | grep sleep
killAll sleep
ps -A | grep sleep

killall sleep

{mongdb}
ps -A | grep mongod
sudo killall mongod

