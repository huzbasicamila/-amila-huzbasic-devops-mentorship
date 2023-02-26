# TASK-2: OverTheWire - bandit-labs#8

## Level 0

![Level 0](pictures/level0.png)

1. First, I opened a terminal on my local computer. I used the terminal application on my Linux machine to do this.
2. I typed the following command in the terminal and pressed Enter: ssh bandit0@bandit.labs.overthewire.org -p 2220. This command specified the username, the host, and the port number to connect to the game server using SSH.

## Level 0 -> Level 1
![Level 1](pictures/level1.png)

1. To see the contents of the home directory, I used the ls command. I saw that there was a file called "readme" in the home directory.
2. To read the contents of the "readme" file, I used the cat command. I typed cat readme and pressed Enter. The password for the next level was displayed on the screen.
3. To log in to the next level using SSH, I used the same command as before, but with the username and password for the next level. I typed ssh bandit1@bandit.labs.overthewire.org -p 2220 and pressed Enter. I was prompted to enter the password for the "bandit1" user, which was the password I found in the "readme" file.

## Level 1 -> Level 2
![Level 2](pictures/level2.png)

1. To see the contents of the home directory, I used the ls command. I saw that there was a file called "-" in the home directory.
2. I used the cat command and specified the path of the file using the full path name. The ./ part of the command specifies that the file is located in the current directory, and the - is the name of the file. 
3. The password for the next level was displayed on the screen.

## Level 2 -> Level 3 
![Level 3](pictures/level3.png)

1. To see the contents of the home directory, I used the ls command. I noticed that there was a file named "spaces in this filename" in the directory.
2. I used the cat command to display the contents of the file. I typed "cat spaces\ in\ this\ filename". This tells the shell that the spaces are part of the filename.
3. The password for the next level was displayed on the screen.

## Level 3 -> Level 4
![Level 4](pictures/level4.png)

1. After logging in, I used the ls command to see the contents of my home directory. There was only one directory called "inhere".
2. I used the cd command to change my current working directory to "inhere". I typed cd inhere and pressed Enter.
3. I used the ls command to see the contents of the "inhere" directory.  There was no password file there. 
4. I used the ls -a command to see all the files in the directory, including the hidden ones. I found a hidden file called ".hidden".
5. o display the contents of the hidden file, I used the cat command. I typed cat .hidden
6. The password for the next level was displayed on the screen.

## Level 4 -> Level 5
![Level 5](pictures/level5.png)

1. After logging in, I used the ls command to see the contents of my home directory. There was only one directory called "inhere".
2. I used the cd command to change my current working directory to "inhere". I typed cd inhere and pressed Enter.
3. I used the ls command to see the contents of the "inhere" directory. There were several files in the directory.
4. I used the ls -l command to see the details of the files in the "inhere" directory, including their permissions, ownership, and size.
5. The password is stored in the only human-readable file in the directory. To find the file, I used the file --* command to determine the file type of each file in the directory. The output of the file command revealed that most of the files were data files or binary files, but file07 was a human-readable text file.
6. I used file -m -file07 command to read it and there I found password to the next level.

## Level 5 -> Level 6
![Level 6](pictures/level6.png)

1. I used the ls command to see the contents of my home directory. There was only one directory called "inhere".
2. I Navigated to the inhere directory: cd inhere
3. I used the ls command to see the contents of the "inhere" directory.
4. I used command find . -type f -size 1033c. "-type f": only looks for regular files. "-size 1033c": finds files that are exactly 1033 bytes in size. 
5. The result of the command was that the file is in directory maybehere07 and that the file is .file2
6. I used cat command to read -file2
7. The password for the next level was displayed on the screen.

## Level 6 -> Level 7
![Level 7](pictures/level7.png)

1.  I used the find command to search for a file that meets the specified criteria: "find / -type f -user bandit7 -group bandit6 -size 33c". This command searches the entire file system for a file owned by user bandit7, group bandit6, and with a size of 33 bytes.
2.  The command output showed the location of the file containing the password. I used the cat command to view the contents of the file and retrieve the password: "cat /var/lib/dpkg/info/bandit7.password". 
3.  The password for the next level was displayed on the screen.

## Level 7 -> Level 8
![Level 8](pictures/level8.png)

1. I Used the ls command to list the files in the directory and found the file data.txt .
2. Used the grep command to search for the word "millionth" in the data.txt file using the command grep millionth data.txt.
3. The password for the next level was displayed on the screen.

## Level 8 -> Level 9
![Level 9](pictures/level9.png)

1. I Used the ls command to list the files in the directory and found the file data.txt .
2. Then I used sort data.txt | uniq -u command. To find the line we need, we can use the "sort" command to sort all the lines alphabetically, and then pipe that output to the "uniq -u" command to filter out any duplicates and only show the line that occurs once. 
3. The password for the next level was displayed on the screen.

## Level 9 -> Level 10
![Level 10](pictures/level10.png)

1. I Used the ls command to list the files in the directory and found the file data.txt .
2. Then I used command strings data.txt | grep ===. The strings command prints the printable character sequences that are at least 4 characters long. The output of strings data.txt is then piped (|) to grep which searches for any line that contains the characters ====. This is done because the password is expected to be found in a human-readable string that is preceded by several = characters.
3. The password for the next level was displayed on the screen.

## Level 10 -> Level 11
![Level 11](pictures/level11.png)

1. I Used the ls command to list the files in the directory and found the file data.txt .
2. To solve this level, I used the following command: base64 -d data.txt. This command decodes the base64 encoded data in the file data.txt and outputs the result to the terminal. 
3. The password for the next level was displayed on the screen.