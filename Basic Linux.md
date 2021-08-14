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

Use less to view the contents of the file.txt file, like this:
    `cat file.txt`
 

1. Finally, to delete the file.txt file, pass the name of the file as an argument to rm:
    `rm file.txt`
    
1. If your question has to do with a specific Linux command, the manual pages offer detailed and insightful documentation for nearly every command. To see the man page for any command, pass the command’s name as an argument to the man command:
    `man command`

