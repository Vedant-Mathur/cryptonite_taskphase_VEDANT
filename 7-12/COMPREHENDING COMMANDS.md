# Challenge Summary: cat - Not the Pet but the Command

## Objective
Use the `cat` command to read the flag file that has been copied to your home directory.

## Understanding `cat`
- The `cat` command is commonly used for displaying the contents of files.
- It can also concatenate multiple files:
  ```bash
  hacker@dojo:~$ cat file1 file2
![alt text](image.png)
------
# Challenge Summary: catting Absolute Paths

## Objective
Use the `cat` command to read the flag from its absolute path: `/flag`.

## Understanding Absolute Paths
- You can specify the `cat` command's arguments using absolute paths:
  ```bash
  hacker@dojo:~$ cat /path/to/file

![alt text](image-1.png)
------
# Challenge Summary: More Catting Practice

## Objective
Retrieve the flag by using the `cat` command with an absolute path from a specified crazy directory.

## Understanding the Constraints
- You **cannot** change directories using `cd`, so accessing the flag through relative paths or changing to the flag's directory is not allowed.

## Steps
1. Use the `cat` command to read the flag from its absolute path (you'll need to know the path given in the challenge):
   ```bash
   hacker@dojo:~$ cat /crazy/directory/path/to/flag
   FLAG{1234-5678-CRAZY}

![alt text](image-2.png)
------
# Challenge Summary: Grepping for a Needle in a Haystack

## Objective
Use the `grep` command to search for the flag in a large file located at `/challenge/data.txt`.

## Understanding `grep`
- The `grep` command searches for specific text within files:
  ```bash
  hacker@dojo:~$ grep SEARCH_STRING /path/to/file

![alt text](image-3.png)
-----
# Challenge Summary: Listing Files

## Objective
Use the `ls` command to list the contents of the `/challenge` directory and find the file named `run`.

## Understanding `ls`
- The `ls` command lists the files and directories in the specified path. If no path is provided, it lists the contents of the current directory:
  ```bash
  hacker@dojo:~$ ls /path/to/directory
![alt text](image-4.png)
------
# Challenge Summary: Touching Files

## Objective
Create two files in the `/tmp` directory and then run the `/challenge/run` command to retrieve your flag.

## Understanding `touch`
- The `touch` command is used to create an empty file or update the timestamp of an existing file:
  ```bash
  hacker@dojo:~$ touch filename
![alt text](image-5.png)
-----
# Challenge Summary: Removing Files

## Objective
Delete the `delete_me` file from your home directory and run `/challenge/check` to retrieve your flag.

## Understanding `rm`
- Use the `rm` command to remove files:
  ```bash
  hacker@dojo:~$ rm filename

![alt text](image-6.png)
-----
# Challenge Summary: Hidden Files

## Objective
Locate and display the flag hidden as a dot-prepended file in the root directory (`/`).

## Understanding Hidden Files
- Files that start with a dot (.) are considered hidden in Linux and will not appear in a standard `ls` listing.
- Use the `-a` option with `ls` to display all files, including hidden ones:
  ```bash
  hacker@dojo:~$ ls -a
![alt text](image-7.png)
------
# Challenge Summary: An Epic Filesystem Quest

## Objective
Use the `ls` and `cat` commands to follow clues in the filesystem and locate the hidden flag.

## Steps
1. Start at the root directory (`/`):
   ```bash
   hacker@dojo:~$ cd /

![alt text](image-8.png)
![alt text](image-9.png)
-----
# Challenge Summary: Making Directories

## Objective
Create a directory named `pwn` in `/tmp`, then create a file named `college` inside that directory, and run `/challenge/run` to retrieve your flag.

## Understanding `mkdir` and `touch`
- Use `mkdir` to create a new directory:
  ```bash
  hacker@dojo:~$ mkdir /path/to/directory
![alt text](image-10.png)
-----
# Challenge Summary: Finding Files

## Objective
Use the `find` command to locate the hidden flag file in a random directory on the filesystem.

## Understanding the `find` Command
- The `find` command can search for files and directories based on various criteria.
- Basic syntax:
  ```bash
  find [search_location] [search_criteria]

![alt text](image-11.png)
![alt text](image-12.png)
------
