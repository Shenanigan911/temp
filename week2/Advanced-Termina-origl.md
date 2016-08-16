# Advanced Terminal

## Purpose
The purpose of this lecture is to make students feel more comfortable and productive at the command-line. By the end of this lecture, students should have a basic understanding of the following concepts:

- The `.bash_profile` file, what it's for, when it's run, etc.
- How to create variables, and the difference between local and exported variables.
- The difference between single and double quotes in BASH.
- Some intermediate BASH commands that particularly useful
    - `echo`
    - `alias`
    - `less`
    - `man`
    - `grep`
- How to compose more complex commands using shell redirection (>, >>, <, and |)

### Commands

`echo`
- prints stuff on the command line

```bash
    $ echo "hello world" # simple command to print text on the screen
```

`.bash_profile`
- executes every time you open a new terminal
- good place for customizing your terminal environment

```bash
    $ cat ~/.bash_profile
```

`alias`
- set up one command to run another command

```bash
    $ alias lsa="ls -a"
    $ alias subl="open -a '/Applications/Sublime Text 2.app'"
```

### Variables
```bash
    $ greeting="hello bash" # local var:
    $ echo greeting # prints 'greeting'
    $ echo $greeting # prints 'hello bash'
    $ touch $greeting # creates files called 'hello' and 'bash'
    $ touch '$greeting' # creates a file called '$greeting'
    $ touch "$greeting" # creates a file called 'hello bash'
    $ export port=3000 # environment variable

    $ # exported variables are available to scripts, e.g. use process.env to access exported variables in node.js scripts.
    $ # put it in your .bash_profile to make it permanent
```

`function` - like alias, but better!

`$1` is the first argument passed into the function. `$2` is the second argument, etc.

```bash
    function can {
        cp -r "$1" ~/.Trash
        rm -r "$1"
    }
```

Use `$` to insert other expressions

```bash
    echo ls # prints 'ls'
    echo $(ls) # prints the contents of the current folder
```

### Automatic Backup

This function copies files to a folder called 'backup', and then renames them by putting a timestamp at the end of the filename.

```bash
    function backup
    {
            cp -r "$1" "$HOME/backup/"
            mv "$HOME/backup/$(basename $1)" "$HOME/backup/$(basename $1)_$(date)"
    }
```

`grep` - find a line of text with given contents

```bash
    grep alias ~/.bash_profile # prints every line from the .bash_profile that contains the text 'alias'
```

### Shell Redirection (pipe)
- use output from one command as input to another

```bash
    ls | grep
```