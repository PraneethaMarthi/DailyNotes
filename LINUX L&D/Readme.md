# Basic Commands

* Echo : To print anything **echo Hello**
* cal : prints calendar **cal**
* date : prints date and timezone
* ctrl+l : clears screen
* history : prints previous history **history**
    - to view the particular line in history **!3**
* echo $PATH : prints the path 
* exit : closes terminal
## basic command structure **$ CommandName options inputs**

* **Command Names** needs to be on shells path and *8Commands** operate on inputs.
* **options** modify on command's behaviour
 
 # Linux Manual - The Manual Structure 
  
* User Commands : Commands that can be run from the shell by a normal user (typically no administrative privileges are needed)
   
* System Calls : Programming functions used to make calls to the Linux kernel

* C Library Functions : Programming functions that provide interfaces to specific programming libraries.

* Device and Special Files : File system nodes that represent hardware devices or software devices.

* File Formats and Conventions : The structure and format of file types or specific configuration files.

* Games : Games available on the system

* Miscellaneous : Overviews of miscellaneous topics such as protocols, filesystems and so on.

* System administration tools and Daemons : Commands that require root or other administrative privileges to use

# Linux Manual - Reading the Man Pages

* [THING] - THING is optional.
* <THING> - THING is mandatory (required)
* THING ... - THING can be repeated (limitlessly)
* THING1 | THING2 - Use THING1 OR THING2. Not Both.
* _THING_ - [Notice the Italics] Replace THING with whatever is appropriate.

- To Search in the manual **$ man -k which** which is a search input and -k is option

## Command Input and Output

* Inputs are **Standard Inputs** which are Standard Data Stream
and **Command Arguments** which are Not a Data Stream
* Outputs are **Standard O/P** and **Standard Error** Both are Standard Data Stream
* you can redirect the standard **output** of the one command to the standard **input** of another in a process known as **Piping**.

* To **redirect** a standard output to the file as **$ cat 1> output.txt** (output.txt created)

0-standard input
1-standard output
2-standard error

* Find more in https://mywiki.wooledge.org/BashGuide/InputAndOutput?#Redirection

* https://www.gnu.org/software/bash/manual/html_node/Redirections.html


# Piping 

* **Piping** is the connection of the standard output of one command to the standard input of
another command.  
* Piping using the pipe character (|) which is accessed by pressing **SHIFT + BACKSLASH (\)** on most keyboards.
* pipe together commandOne and commandTwo:
    * **commandOne –options arguments | commandTwo –options arguments**
    * Notice how both commands can have their own options and command line arguments as usual. This piping can go on for as long as is required with as many commands as is required.

For example, this wouldn’t work:
* **commandOne –options arguments > snapshot.txt | commandTwo –options arguments**  Because redirection is processed by the shell before piping is, snapshot.txt would be created, but this locks up the standard output stream and therefore no data can be passed through the pipeline to commandTwo.
* NB: Redirection breaks pipelines However, the tee command allows us to take a “snapshot” of the data in the pipeline without breaking the pipeline.
    * commandOne –options arguments | tee snapshot.txt | commandTwo –options arguments

* Piping connects the standard output of one command to the standard input of another command. But what if the second command doesn’t accept standard input? e.g. the echo command.
* The key is to transform the data coming in, into command line arguments. This is possible using the xargs command.
    * For example, this would not work:
        **commandOne –options arguments | echo**
    * This would work:
        **commandOne –options arguments | xargs echo**

# Aliases

* Aliases allow you to save your pipelines and commands with easy to remember nicknames so that they can be used later much easier.

* You define aliases in your .bash_aliases file in your home directory. If it does not exist, you need to create it spelled exactly as shown. Note that the preceding period (.) must be included and there should be no file extension (such as .txt, or .pdf).
* Here is how you define an alias in .bash_aliases:
    **alias aliasName=”THING YOU WANT TO ALIAS”**
* Notice that there are no spaces between the equals sign (=) and the aliasName and the quotes (“). The quotes can be double quote (“) or single quotes (‘).
* Let’s take an example:
    alias calmagic=”cal –A 1 –B 1 12 2017”. With this alias defined in our .bash_aliases file, whenever we run the calmagic command it is as if we ran the cal –A 1 –B 1 12 2017 command. calmagic is now said to be an alias of “cal –A 1 –B 1 12 2017”.
* NB: Aliases may contain either one command or an entire pipeline!

## Piping to Aliases

* If the first command in an alias accepts standard input, then the alias can be piped to; even if it is an entire pipeline!
* Our alias is currently:
        **alias calmagic=”cal –A 1 –B 1 12 2017”**
    - cal is the first command in this alias, but cal doesn’t accept standard input. 
    - Therefore, this would not work: 
        **commandOne –options arguments | calmagic**
    - However, if we adjust our alias so that it can accept standard input.
        **alias calmagic=”xargs cal –A 1 –B 1 12 2017”**
    - This will now work:
        **commandOne –options arguments | calmagic**
* And yes, you can pipe out of an alias as well, if the alias produces standard output.
        **commandOne –options arguments | calmagic | commandTwo –options arguments**

# Mastery Level 3 :

* The Linux File System follows a tree-like structure starting at a base (or root) directory, indicated by the slash (/).
* Locations on the file system are indicated using file paths.
* Paths that start at the base directory (/) are known as absolute paths.
* Paths that start from the current working directory of the shell, are known as relative paths.
* For example both of these examples refer to a file called “file1.txt” in the Documents folder for a user called Sarah. The relative path assumes the shell is in Sarah’s home directory.
    * Absolute: /home/sarah/Documents/file1.txt
    * Relative: Documents/file1.txt

## Key Commands for Navigating the File System

* **pwd** : Print on standard output the absolute path to the shell’s
current working directory
* **cd [<*new location>]**  : Change the shell’s current working directory to the
optional <*new location>. If no location is provided, return
to the user’s home directory.
* **ls [<*location>]** : List out the contents of the optional <location> directory.
If no <location> is provided, print out the contents of the
shell’s current working directory. 

## Key Shortcuts when Navigating File System.
* ~ - The current user’s home directory
* . - The current folder.
* .. - The parent directory of the current folder.

## Wildcards and Regular Expressions

* Regular expressions are patterns that can be used to match text. In Linux, they are used to allow a user to make rather generic expressions about what files they want a command to operate on.
* Creating regular expressions to match filenames is known as globbing.
* The regular expression patterns can be made using special building blocks known as wildcards.
* Wildcards are symbols with specific meanings to the shell.
* The 3 most common types of wildcard:

    '*' - Matches anything, regardless of length.

    '?' - Matches anything, but for one place only.

   ' [options]' - Matches any of the options inside for 1 place only.
* These 3 wildcards are incredibly versatile and will cover an exhaustive amount of your regular expression requirements. 

### Creating Files and Directories


* **touch <file>** -  Creates an empty file.
    - e.g. touch ~/Desktop/file1.txt
* **mkdir <directory>** -  Creates an empty directory.
    - e.g. mkdir ~/newdir

### Deleting Files and Directories

* **rm <file>** - Remove a file
    e.g. rm ~/Desktop/file1.txt
* **rm -r <directory>** - Removes a directory.
    e.g. rm -r ~/newdir
* **rm –i** - Removes in an interactive manner. This is a good safety measure.
* **rmdir <*empty directory>**- Only remove empty directories
    e.g. rmdir ~/emptydir 


## Editing Files with the Nano Editor

* Nano is a versatile terminal-based txt editor. It comes with a built-in toolbar of various commands and all one needs to know in order to use these options are the meaning of 2 special symbols.
    * ^ - This is the CTRL key on your keyboard.
            For example, ^O is CTRL + O.
    * M- - This is the “meta” key on your keyboard. Depending on your keyboard layout this may be the ALT, ESC, CMD key. Try it out  Assuming M- is the ALT key, then M-X is ALT + X

## The Locate Command

* The locate command searches a database on your file system for the files that match the text (or regular expression) that you provide it as a command line argument.
* If results are found, the locate command will return the absolute path to all matching files.
    - For example: 
    **locate *.txt**: will find all files with filenames ending in .txt that are registered in the database. The locate command is fast, but because it relies on a database it can be error prone if the database isn’t kept up to date.
* Below are some commands to update the database and some reassuring procedures in case one cannot access administrator privileges.
* **Locate -S** Print information about the database file. 
* **sudo updatedb** Update the database. As the updatedb command is an administrator command, the sudo command is used to run updatedb as the root user (the administrator)
* **Locate --existing** Check whether a result actually exists before returning it.
* **locate –limit 5** Limit the output to only show 5 results 

## The Find Command

* The find command can be used for more sophisticated search tasks than the locate command. This is made possible due to the many powerful options that the find command has.
* The first thing to note is that the find command will list both files and directories, below the point the file tree that it is told to start at.
    For example:
        **find .** - Will list all files and directories below the current working directory (which is denoted by the .)
        **find /** - Will list all files and directories below the base directory (/); thereby listing everything on the entire file system!
* By default, the find command will list everything on the file system below its starting point, to an infinite depth.
* The search depth can however be limited using the –maxdepth option.
    For example
        **find / -maxdepth 4** - Will list everything on the file system below the base directory, provided that it is within 4 levels of the base directory.
* There are many other options for the find command. Some of the most useful are below:

    * **-type** Only list items of a certain type. –type f restricts the search to file and –type d restricts the search to directories.
    * **-name “*.txt”** Search for items matching a certain name. This name may contain a regular expression and should be enclosed in      double quotes as shown. In this example, the find command will return all items with names ending in .txt.
    * **-iname** Same as –name but uppercase and lowercase do not matter.
    * **-size** Find files based on their size.
        - e.g **–size +100k** finds files over 100 KiB in size **–size -5M** finds files less than 5MiB in size. Other units include G for GiB and c for bytes**.
* **Note**: 1 Kibibyte (KiB) = 1024 bytes. 1 Mebibyte (MiB) = 1024 KiB. 1 Gibibyte = 1024 MiB.

* A supremely useful feature of the find command is the ability to execute another command on
each of the results.
* For example
    - **find /etc –exec cp {} ~/Desktop \,**; - will copy every item below the /etc folder on the file system to the ~/Desktop directory.
* Commands are executed on each item using the –exec option.
* The argument to the –exec option is the command you want to execute on each item found by
the find command.
* Commands should be written as they would normally, with {} used as a placeholder for the results of the find command.
* Be sure to terminate the –exec option using \; (a backslash then a semicolon).
* The –ok option can also be used, to prompt the user for permission before each action.
* This can be tedious for a large number of files, but provides an extra layer of security of a small number of files; especially when doing destructive processes such as deletion.
* An example may be:
    - **find /etc –ok cp {} ~/Desktop \,**;
