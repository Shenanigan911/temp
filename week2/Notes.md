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



## New shell commands
### Echo
Echo prints its arguments to the console

`echo hello`


An argument is information used by the command.

`echo hello`

A flag modifies the behavior of a command and always starts with a -.

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
We can use > to redirect STDOUT to a file or use >> to append it to a file.

```
echo $greeting > hello-file # send STDOUT to a file
valediction=”goodbye bash”	# valediction is the opposite of a greeting
echo $valediction > bye-file	
cat hello-file > conversation-file  // effectively copies hello-file
cat conversation-file
```

`cat bye-file >> conversation-file // appends bye-file to conversation-file`

```
echo > empty-file-echo  # send an empty line to a new file
touch empty-file-touch  # create a new empty file
```

```
ls -l empty-file*
cat empty-file-echo
cat empty-file-touch
```

## Less is More

`ls -l dir-big`

`ls -l dir-big > listing`

`more` buffers the output of a command.
Use `d` to move forward and `q` to quit.
`more` only moves forward through the file.

`more listing`

`less` buffers the output of a command, but you can move forwar, backward and even side-to-side.
Use b  to move back, or use arrow keys to move up, down, left and right.
`less` is the same as `more` but with more!  
*Less is more!*

`less listing`

## Pipe

Pipes (`|`) send the STDOUT from one command to the STDIN of another command.

```
ls dir-big
ls dir-big | less
```

## Permissions

`cat house-of-straw`

```
echo wolf > house-of-straw
cat house-of-straw
cat house-of-sticks
echo wolf > house-of-sticks
```

`ls -al house-of-*`

`drwxrwxr-x 2  root  mail  4096  Aug 15  2016  mail`
- first letter is file type: d (directory), - (file) or l (link)
- letter trios are read, write, execute
- trios are owner/user, group, other (everyone  else)
- next comes the file owner and group, then size, then last modified date then filename

```
touch house-of-straw
ls -l house-of-straw
```

### chmod (change mode)
`chmod` changes the mode of a file.
- chmod {+/-}{rwx} {files}
- Use `chmod` to add or remove read/write/execute permissions
- In Windows and MacOS, the file suffix is used to determine if file is executable.  In shell, anything can be executable.
- This is how you control access to and execution of a file.

`ls -l house-of-sticks`

```
chmod +w house-of-sticks    # add write permissions to the file
ls -l house-of-sticks       # check the new permissions
echo wolf > house-of-sticks # overwrite the file
```

`cat house-of-bricks`

`ls -l house-of-bricks`

```
chmod +rw house-of-bricks
ls -l house-of-bricks
cat house-of-bricks
```

```
chmod -x ..
cd ..
```

` chmod +x ..`

```
cat hi
hi
```


```
ls -l hi
chmod +x hi
ls -l hi
```





`echo $USER`


`env`


Environment variables like $PATH and $USER have global scope.

They can be used in any child process of a shell.

You must `export` a local variable in order to make it an environment variable.

```
export greeting=”howdy”     # create an environment variable
env                         # show environment variables
```


`./hi`


And you can clear the value of a variable with `unset`.

`unset greeting              # unsets an environment variable`


`grep` (Global Regular Expression/Print) is a command to filter its input based on a regular expression.

`env | grep USER    # take the output of env and only print lines containing the string "USER"`



```
export PATH=$PATH;.
hi
```

## Shell
The `sh` command creates a new shell which inherits the current environment.
The `quit` command will exit out of the child shell.


```
myLocalVar="I am local"
echo $myLocalVar
export myGlobalVar="I am global"
echo $myGLobalVar
```


```
sh
echo $myLocalVar
echo $myGLobalVar
quit
```

Don't exit out of your original shell or it will close the terminal!

## Pushd/Popd

`cd ~/playground/here`


`cd ~/playground/there`


```
cd ~/playground/here
cd ~/playground there
```


`pushd` is a LIFO (Last In/First Out) stack of directories that you can use to move back and forth between two or more directories.

```
pushd ~/playground/here # push here onto the directory stack
pushd                   # swaps the first and second directories in the stack
pushd                   # and swap the first two again
popd                    # pops the first directory off the stack and returns you to the next directory
```


```
pushd $HOME/workspace
pushd $HOME/playground
pushd /
pushd
pushd
popd
popd
```

## Alias
`alias` is a way of creating a shorthand command.
It is often used to abbreviate a command or command with common arguements.

```
alias ws=’pushd $HOME/workspace’
ws      # run the newly defined alias and pushd to the workspace
popd    # return to your original directory
```


`alias g=’git commit -m “quick commit”'`


`alias g=”cd $HOME/workspace; git add .; git commit -m \“quick commit\”;git push origin master;”`


Aliases are only interpretted as commands - the first thing on the command line.

```
echo ws     # ws is not interpretted as an alias
ws          # ws is interpretted as an alias
pushd
```


```
alias g=”cd $HOME/workspace; git add .; git commit -m \“$*\”;git push origin master;”

```


Alias can’t take arguments.  It just expands on the command line.  We need a function!

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


## .bashrc
.bashrc is a file that is run every time you start a new shell.  It initializes the state of the shell.
```
cd
less .bashrc
```

`.bashrc` can invoke other files which are then also processed at the same time.
This is done to organize what could otherwise become a very long, complicated script.

## Editors
In addition to `nano`, the bash shell has many editors.
`nano`, `ex`, `vi`, `vim`, `emacs`

And don't forget
`man` or `cmd -help`



