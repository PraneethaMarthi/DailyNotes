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


