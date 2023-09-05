
# Bash Cheatsheet
Based on notes taken from [missing semester](https://missing.csail.mit.edu)
## General Stuff
- Spaces are v important!
- `<program> --help` gets (simple) docs for most programs
- `--flag` is called a flag
- `-o` is an option
- `ctrl + l` clear terminal
- `ctrl + r` search history
- `!!` is shorthand for your previous command

### Streams
Programs have an input & an output stream, these are by default the terminal. These can be altered using <, >, |
- `echo hello > hello.txt` creates a file called hello.txt containing the argument "hello"
- `cat < hello.txt > hello2.txt` works the same as `cp`
- `>>` appends, rather than writes
- `|` takes output of right program & inputs it into left program
    - `ls -l / | tail -n1` takes the output, the long list of files in root & feeds it into tail, returning details of the last file

### The Superuser
- The superuser/ root can run any file. To run a program temporarily as root, run `sudo <program name>`
- To become superuser, run `sudo su`. This is indicated by `$` changing to `#`. Run `exit` to revert to `$`.
- You can avoid becoming the user permanently when piping by running 
    ```bash
    $ <program> | sudo tee <program-that-needs-sudo>
    ```


## Programs

### Meta
- `echo` prints arguments
- `history` returns all of your previous commands
- `man` manual page: documentation
    - `q` to quit
    - `tldr` is a similar downloadable & cleaner option
- `sudo` run program as superuser
- `tee` feed input into output stream and the given program
- `open` (mac) opens a file in relevent program. (`xdg-open` for linux )
- `tree`, `broot` and `nnn` can allow looking through a directory manually, rather than using `ls` repeatedly
- `code` VSCode things

### File Management
- `cd` change directory
    - `-` goes to previous directories
- `ls` show child directories
    - `-l` long format, gives more info 
        - 9 characters, split into 3 blocks giving permissions for owner, group and everyone else, respectively
        - 3 characters account for read, write excecute (`rwx`). For a dir read means use ls, write is adding, renaming, removing files and excecute is 'search' and allows you to open the dir.
    - `*.sh` will list all files ending in .sh
    - `thing?` would list thing1 and thingg but not thing42
- `mv` rename file (including path)
- `cp` copy file (including path)
- `rm` remove file (non-recursive)
    - `-r` recursive: removes directory & everything below
- `rmdir` remove directory, only if empty
- `mkdir` I'm a teapot

### File editing
- `cat` prints content of file
- `tail` prints the last n lines of a file
- `diff` shows difference between files

### Searching
- `find .-name */src/*.tsx -type f` finds all files with a tsx extension somewhere within a src directory, somewhere within the current directory
- `locate src` finds all files with src within their path. Uses an optimised db
- `grep` searches files an expression
- `rg` or `ripgrep` searches the given directory for files containing an expression. `ag` is similar
- `fzf` fuzzy finder: allows active changing of the search expression
    ```bash
    $ cat example-file.txt | fzf
    ```
    - binds with `ctrl + r` to fuzzy search history

## Scripting
To define a variable use `=`
```bash
$ foo=bar
$ echo $foo
bar
```
`"` and `'` are subtley different
```bash
$ echo "Value is $foo"
Value is bar
$ echo 'Value is $foo'
Value is $foo
```
- `source` makes a program available to be used

Example file, `mcd.sh`:
```bash
mcd () {
    # This is a comment
    mkdir -p "$1"
    cd "$1"
}
```
We then run the following, to make and change to the directory `./foo`
```bash
$ source mcd.sh
$ mcd foo
```
- `$?` returns the exit code from the previous command
    - `0` means there were no errors
- `grep this || grep that` runs first program & if it fails runs the second one
- `grep this && grep that` runs first program & if it succeeds runs the second one 
- The following are equivalent
    - `ls thing{1..4}`
    - `ls thing{1,2,3,4}`
    - `ls thing1 thing2 thing3 thing 4`

We run a python file by starting it with a shebang (e.g `#!/usr/local/bin/python`), depending on the location. For example
```python
#!/usr/local/bin/python
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```
Something like `#!/usr/bin/env python` would be more portable

- `shellcheck thing.sh` will debug your script