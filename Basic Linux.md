# Linux Basic


## Terminal 

        
1. Any administrative task, including file manipulation, package installation, and user management, can be accomplished through the terminal.


## Filesystem Hierarchy Standard

1. Nearly all Linux distributions are compliant with a universal standard for filesystem directory structure known as the Filesystem Hierarchy Standard (FHS). The FHS defines a set of directories, each of which serve their own special function.

1. Linux filesystems are based on a directory tree. This means that you can create directories (which are functionally identical to folders found in other operating systems) inside other directories, and files can exist in any directory.


## Navigation

1. To see what directory you are currently active in you can run the pwd command, which stands for “print working directory”:
    `pwd`

1. To see a list of files and directories that exist in your current working directory, run the ls command:
     `ls`
        *Add a path and it lists the contents of that directory. If you want to know more about the files, use the -l (--long) option, which tells you the size and date of the file, along with information about ownership and permissions, which we will look at later.


1. You can create one or more new directories within your current working directory with the mkdir command, which stands for “make directory”. For example, to create two new directories named testdir1 and testdir2, you might run the following command:
    `mkdir testdir1 testdir2`

1. To navigate into one of these new directories, run the cd command (which stands for “change directory”) and specify the directory’s name:
    `cd testdir1`

## File Manipulation

1. If you decide to rename file.txt later on, you can do so with the mv command:
    `mv file.txt newfile.txt`


1. It is also possible to copy a file to a new location with the cp command. If we want to bring back file.txt but keep newfile.txt, you can make a copy of newfile.txt named file.txt like this:
    `cp newfile.txt file.txt`
    
1. To add text to file.txt with nano, run the following command:
    `nano file.txt`
    

1. Using cat to view file contents can be unwieldy and difficult to read if the file is particularly long. As an alternative, you can use the less command which will allow you to paginate the output.

1. Use less to view the contents of the file.txt file, like this:
    `cat file.txt`
 

1. Finally, to delete the file.txt file, pass the name of the file as an argument to rm:
    `rm file.txt`
    
1. If your question has to do with a specific Linux command, the manual pages offer detailed and insightful documentation for nearly every command. To see the man page for any command, pass the command’s name as an argument to the man command:
    `man command`

## What goest Where?

1. `/` The root of the filesystem, which contains the most critical components.

1. `/bin` and /usr/bin General commands.

1. `/sbin` and /usr/sbin System administration commands for the root user.

1. `/etc` Where system configuration files are kept.

1. `/usr` Where most of the operating system lives. This is not for user files, although it was in the dim and distant past of Unix and the name has stuck.

1. `/lib` and /usr/lib The home of system libraries.

1. `/var` Where system programs store their data. Web servers keep their pages in /var/www and log files live in /var/log.

1. `/home` Where users' data is kept. Each user has a home directory, generally at /home/username.
