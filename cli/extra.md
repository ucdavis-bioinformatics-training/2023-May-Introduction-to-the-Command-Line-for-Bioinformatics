Extra Command-Line
=======================

Find
-----

Find is a very powerful command that is used to recursively find files/directories in a file system. First take a look at the find man page:

    man find

Notice there are LOTS of options. The simplest version of the find command will simply list every file and directory within a path, as far down as it can go:

    find /usr/lib

If we want to refine the command to only show you files that end in ".so", we use the "-name" option:

    find /usr/lib -name "*.so"

One of the most powerful uses of find is to execute commands on every file it finds. To do this, you use the "-exec" option. When you use that option, everything after the "-exec" is assumed to be a command, and you use the "{}" characters to substitute for the file names that it finds. So in the command below, "wc -l" will get executed sequentially for every file it finds. Finally, the exec option needs to end with a semi-colon, however, since the semi-colon is a special character that the shell will try to interpret, you need to "escape" the semi-colon with a backslash, to indicate to the shell that the semi-colon is NOT to be interpreted and just sent as is to the find command:

    find /usr/lib -name "*.so" -exec wc -l {} \;

You will probably want to Ctrl-C out of this because it will take a long time to go through them all.


Xargs
------

xargs is another command that can be very useful for running a program on a long list of files. For example, the "find" commands we ran above could be run using xargs like this:

    find /usr/lib -name "*.so" | xargs wc -l

This is taking the output of the find command and then creating a list of all the filenames which it adds to the command given to xargs. So, in this case, after "xargs" comes "wc -l"... so "wc -l" will get run on the entire list of filenames from find.


.bashrc/.zshrc, aliases & the PATH variable
-----------------------------------------------------

On a Linux system, there is usually a user-modifiable file of commands that gets run every time you log in. This is used to set up your environment the way that you want it. On Windows it is called ".bashrc" and it resides in your home directory. On Mac it is called ".zshrc". Sometimes the file is called ".bash_profile". Take a look at a .bashrc:

    cat ~/.bashrc

This one has a lot of stuff in it to set up the environment. Now take a look at your own .bashrc. One of the things you set up was adding to the PATH variable. One thing that is very useful to add to the PATH variable is the "." directory. This allows you to execute things that are in your current directory, specified by ".". So, let's use nano to edit our .bashrc and add "." to PATH:

    nano ~/.bashrc
Add ":." to the PATH variable by adding this line:

**export PATH=$PATH:.**

 Now, next time you log in, "." will be in your PATH.

Another thing that is very useful are aliases. An alias is a user-defined command that is a shortcut for another command. For example, let's say you typed the command "ls -ltrh" a lot and it would be easier to have it be a simpler command. Use an alias:

    alias lt='ls -ltrh'

Now, you've created an alias that lists the contents of a directory with extra information (-l), in reverse time order of last modified (-r and -t), and with human readable file sizes (-h). Try it out:

    lt

Typing alias by itself give you a list of all the aliases:

    alias

You can now put the alias command for lt in your .bashrc and you will have it automatically when you log in.


Environment variables
---------------------

In Linux, environment variables act as placeholders for information stored within the system that passes data to programs launched in your shell. To look at most of the environment variables currently in your shell, use the **env** command:

    env

In order to see the value of any one variable, you can "echo" the value using a "$" in front of the variable name:

    echo $USER

This gives you your user ID. Some of the most commonly used environment variables are USER, HOSTNAME, SHELL, EDITOR, TERM, and PATH.

You can also create your own environment variables:

    MYVAR=1
    echo $MYVAR

By convention, variable names are in all caps, but they don't have to be. You can then update the variable as well:

    MYVAR=2
    echo $MYVAR

In order to use any environment variable in a program that you execute from the command-line, you need to use the **export** command:

    export MYVAR=3

You can also use backticks (\`) to execute a command and send the output into a variable:

    MYVAR=`pwd`
    echo $MYVAR


More grep
----------

Grep is a very powerful tool that has many applications. Grep can be used to find lines in a file that match a pattern, but also you can get lines above and below the matching line as well. The "-A" option to grep is used to specify number of lines after a match and the "-B" option is for number of lines before a match. So, for example, if you wanted to find a particular sequence in a fastq file and also the 3 other lines that form that entire fastq record, you would do this:

    zcat C61.subset.fq.gz | grep -B1 -A2 CACAATGTTTCTGCTGCCTGAACC

This looks for the sequence "CACAATGTTTCTGCTGCCTGAACC" in the fastq file and then also prints the line before and two lines after each match. 

Another thing grep can do is regular expressions. Regular expressions are a way of specifying a search pattern. Two very useful characters in regular expressions are "^" and "$". The "^" symbol specifies the beginning of a line and the "$" specifies the end of a line. So, for example, if you wanted to find just the lines that began with "TTCCAACACA" you would do this:

    zcat C61.subset.fq.gz | grep ^GTGGCC

Without the "^", grep will find any line that has "TTCCAACACA" *anywhere* in the line, not just the beginning. Conversely, if you wanted to find the lines that ended in "TAAACTTA":

    zcat C61.subset.fq.gz | grep GCCTGAAG$

There are also extended regular expression that grep can use to do more complex matches, using the "-E" option:

    zcat C61.subset.fq.gz | grep -E '^GTGGCC|GCCTGAAG$'

This command will find any line that begins with "TTCCAACACA" **OR** ends with "TAAACTTA". The "\|" character means OR.

**CHALLENGE:**
Find a way to use grep to match any line that has between 7 and 16 'A's in a row at the end of the line. You will probably need to look at the man page for grep.

The Prompt
------------

The Prompt is the part of the command line that is the information on the screen before where you type commands. Typically, it has your username, the name of the machine you are on, and your current directory. This, like everything else in Linux, is highly customizable. Your current prompt probably has your username, your hostname, and your current directory. However, there are many other things you could add to it if you so desired. The environment variable used for your prompt is "PS1". So to change your prompt you just need to reassign PS1. There are some special escape characters that you use to specify your username, hostname, etc... these can be found in the "PROMPTING" section of the bash man page:

    man bash

Then type "/" and "PROMPTING" to find the section. You'll see that for username it is "\u", for hostname it is "\h", and for the full current working directory it is "\w". So you would change your prompt by doing this:

    export PS1="\u@\h:\w\\$ "

The convention is to put the "@" symbol and the ":" symbol to delineate the different parts, but you can use anything you want. Also, it is convention to put a "$" at the end to indicate the end of the prompt.

You can also do all kinds of fancy things in your prompt, like color and highlighting. Here is an example of such a prompt:

    export PS1='\[\033[7;29m\]\u@\h\[\033[0m\]:\[\e[1m\]\w\[\e[m\]$ '

When you have a prompt you like, you can put it in your .bashrc/.zshrc so that it is automatically set when you log in.


Awk
----

Awk is a simple programming language that can be used to do more complex filtering of data. Awk has many capabilities, and we are only going to touch on one of them here. One really useful thing is to filter lines of a file based on the value in a column. Let's get a file with some data:

    wget https://raw.githubusercontent.com/ucdavis-bioinformatics-training/2022-Jan-Introduction-to-the-Command-Line-for-Bioinformatics/master/cli/DMR.GBM2.vs.NB1.bed -O DMR.GBM2.vs.NB1.bed

Take a look at the beginning of the file:

    head DMR.GBM2.vs.NB1.bed

Let's say we wanted to get only the lines where the pvalue (column 10) was below a certain value. In awk, the default delimiter is the tab character, which most bioinformatics files use as their delimiter. Using the tab as a delimiter, it assigns each value in a column to the variables $1, $2, $3, etc... for as many columns as there are. So if we wanted to get lines where the pvalue column was under 0.00000005 pvalue:

    cat DMR.GBM2.vs.NB1.bed | awk '$10 < 0.00000005'

And lines where the pvalue >= 0.00000005:

    cat DMR.GBM2.vs.NB1.bed | awk '$10 >= 0.00000005'

You can also use it to extract lines with a particular word in a column:

    cat DMR.GBM2.vs.NB1.bed | awk '$1 == "chr3"'

A double equals (==) is used for equality comparisons. This will pull out lines where the chromosome column is "chr3".

Let's say you wanted only the lines where the p-value<0.00000005 AND only on chr3:

    cat DMR.GBM2.vs.NB1.bed | awk '$1 == "chr3" && $10 < 0.00000005'

You can also change the value of one of the columns and then print each line:

    cat DMR.GBM2.vs.NB1.bed | awk '{$2=$2+11; print $0;}'

Or if you want to print a count variable appended to a column, you can use a count variable. You can use the BEGIN keyword to specify awk code that is to be run once before the rest, i.e. initialize the count. And space is the concatenating operator for strings that you use to add the count value to the first column.:

    cat DMR.GBM2.vs.NB1.bed | awk 'BEGIN{cnt=0;} {$1 = $1 "_" cnt; cnt=cnt+1; print $0}'

Take a look at the [awk manual](https://www.gnu.org/software/gawk/manual/gawk.html) to learn more about the capabilities of awk.




