c
### 1.Write a C program to create a new text file and write "Hello, World!" to it?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret;
char buf[]="helloworld";
fd=open("myfile.txt",O_CREAT|O_WRONLY,0660);
if(fd<0)
{
printf("faild to open the file\n");
return;
}
printf("file opened sucessfully\n");
ret=write(fd,buf,strlen(buf));
if(ret<0)
{
printf("failed to write\n");
return;
}
close(fd);
}
```
### Develop a C program to open an existing text file and display its contents?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret;
char buf[64];
fd=open("myfile.txt",O_RDONLY);
if(fd<0)
{
printf("faild to open the file\n");
return;
}
printf("file opened sucessfully\n");
ret=read(fd,buf,64);
if(ret<0)
{
printf("failed to write\n");
close(fd);
return;
}
buf[ret]='\0';
printf("%s\n",buf);
close(fd);
}
```

### Implement a C program to create a new directory named "Test" in the current directory?

```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret;
fd=mkdir("TEST",0777);
if(fd==0)
printf("successfully created\n");
else
printf("not created\n");
}
```

### Write a C program to check if a file named "sample.txt" exists in the current directory?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd;
fd=open("sample.txt",O_RDONLY);
if(fd<0)
{
printf("file doesn't exists\n");
return;
}
printf("file already exists\n");
close(fd);
}
```

### Develop a C program to rename a file from "oldname.txt" to "newname.txt"?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd;
fd=rename("oldname.txt","newname.txt");
if(fd==0)
{
printf("sucessfully renamed\n");
return;
}
printf("failed\n");
}
```

### Implement a C program to delete a file named "delete_me.txt"?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<string.h>
#include<fcntl.h>
#include<sys/stat.h>
void main()
{
int fd;
fd=remove("delect_me");
if(fd==0)
{
printf("sucessfully removed\n");
return;
}
printf("failed\n");
}
```

### Write a C program to copy the contents of one file to another?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<string.h>
#include<fcntl.h>
#include<sys/stat.h>
void main()
{
int file1,file2,ret1,ret2;
char buf[64];
file1=open("textfile.txt",O_RDONLY);
if(file1<0)
{
  printf("failed to read the file\n");
  return;
}
file2=open("textfile2.txt",O_WRONLY|O_CREAT,0660);
if(file2<0)
{
  printf("failed to write the file\n");
  return;
}
ret1=read(file1,buf,64);
if(ret1<0)
{
  printf("failed to read\n");
  return;
  close(file1); 
}
ret2=write(file2,buf,strlen(buf));
if(ret2<0)
{
  printf("failed to write\n");
  return;
  close(file2);
}
printf("sucessfully copied\n");
close(file1);
close(file2);
}
```

### Develop a C program to move a file from one directory to another?
```c
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
void main()
{
int fd;
fd=rename("textfile.txt","TEST/filename.txt");
if(fd==0)
{
printf("sucessfully moved\n");
return;
}
printf("failed\n");
}
```
### Implement a C program to list all files in the current directory?
```c
#include<stdio.h>
#include<sys/types.h>
#include<dirent.h>
void main()
{
struct dirent *name;
DIR *dir;
dir=opendir(".");
if(dir==NULL)
{
printf("opendir");
return;
}
printf("files in the current directory\n");
while(name=readdir(dir))
 printf("%s\n",name->d_name);
}
```
### Write a C program to get the size of a file named "file.txt"?
```c
#include<stdio.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<unistd.h>
void main()
{
int fd;
struct stat file;
fd=stat("textfile.txt",&file);
if(fd<0)
{
printf("error\n");
return;
}
printf("size of the file: %ld\n",file.st_size);
}
```

### Develop a C program to check if a directory named "Test" exists in the current directory?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret;
fd=mkdir("TEST",0777);
if(fd==0)
printf("not exists\n");
else
printf("already exists\n");
}
```
### Implement a C program to create a new directory named "Backup" in the parent directory?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret;
fd=mkdir("Backup",0777);
if(fd==0)
printf("successfully created\n");
else
printf("not created\n");
}
```
### Write a C program to recursively list all files and directories in a given directory?
```c
#include<stdio.h>
#include<sys/types.h>
#include<dirent.h>
#include<stdlib.h>
#include<unistd.h>
#include<stdlib.h>


void list_files()
{
struct dirent *name;
DIR *dir;
dir=opendir(".");
if(dir==NULL)
{
printf("opendir");
}
printf("in recurstion:\n");
while(name=readdir(dir))
 printf("%s\n",name->d_name);
}

void main()
{
printf("files in current directory:\n");
list_files(".");
}
```
### Develop a C program to delete all files in a directory named "Temp"?
### Implement a C program to count the number of lines in a file named "data.txt"?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret,count=0;
fd=open("data.txt",O_CREAT|O_RDONLY);
char buf[1];
if(fd<0)
{
printf("faild to open the file\n");
return;
}
while(ret=read(fd,buf,1)>0)
{
if(buf[0]=='\n')
count++;
}
if(ret<0)
{
printf("failed to read\n");
return;
}
printf("no of lines :%d\n",count);
close(fd);
}
```
#### Write a C program to append "Goodbye!" to the end of an existing file named "message.txt"?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret,count=0;
fd=open("message.txt",O_WRONLY|O_APPEND);
if(fd<0)
{
printf("faild to open the file\n");
return;
}
char buf[]="Goodbye!";
ret=write(fd,buf,strlen(buf));
if(ret<0)
{
printf("failed to write\n");
return;
}
close(fd);
}
```
### Implement a C program to change the permissions of a file named "file.txt" to read-only?
```c
#include<stdio.h>
#include<sys/stat.h>
void main()
{
char fd;
fd=chmod("myfiles.txt",S_IRUSR|S_IWUSR| S_IROTH|S_IRGRP);
if(fd==0)
  printf("sucessfully changed permissions");
else
  printf("failed to change");
}
```
### Write a C program to change the ownership of a file named "file.txt" to the user "user1"?
```c
#include<stdio.h>
#include<pwd.h>
#include<sys/types.h>
#include<fcntl.h>
#include<unistd.h>

int main()
{
char *filename="file.txt";
char *new_owner="jyothi";
struct passwd *pw;
pw=getpwnam(new_owner);
if(pw==NULL)
        printf("user not found");
if(chown(filename,pw->pw_uid,-1)==0)
        printf("ownership of file '%s' is changed to '%s'\n",filename,new_owner);
else
        printf("failed to change");
}
```
### Develop a C program to get the last modified timestamp of a file named "file.txt"?
```c
#include<stdio.h>
#include<sys/stat.h>
#include<stdlib.h>
#include<time.h>
void main()
{
int fd;
struct stat file;
fd=stat("file.txt",&file);
if(fd==-1)
{
printf("error\n");
return;
}
struct time *modtime;
modtime=localtime(&file.st_mtime);
if(modtime<0)
{
printf("localtime");
return;
}
printf("last modified time of %s:%s","file.txt",asctime(modtime));
}
```
### Implement a C program to create a temporary file and write some data to it?
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret;
fd=open("textfile.txt",O_CREAT|O_RDWR,0660);
if(fd<0)
{
printf("faild to open the file\n");
return;
}
printf("file opened sucessfully\n");
char buf[]="this is the temporary file";
ret=write(fd,buf,strlen(buf));
if(ret<0)
{
printf("failed to write\n");
return;
}
close(fd);
}
```
### Write a C program to check if a given path refers to a file or a directory?
```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat st;
    char path[100];

    printf("Enter the path: ");
    scanf("%s", path);

    if (stat(path, &st) == -1) {
        perror("stat");
        return 1;
    }

    if (S_ISREG(st.st_mode))
        printf("It's a file.\n");
    else if (S_ISDIR(st.st_mode))
        printf("It's a directory.\n");
    else
        printf("It's neither a regular file nor a directory.\n");

    return 0;
}
```
22. Develop a C program to create a hard link named "hardlink.txt" to a file named "source.txt"?

#include <stdio.h>
#include <unistd.h>

int main() {
    int result;

    //result = link("newname.txt", "myfile.txt");
    unlink("hardlink.txt");
    if (link("source.txt", "hardlink.txt") == 0)
        printf("Hard link created successfully.\n");
    else{
        perror("link");
        return 1;
    }
    return 0;
}

23. Implement a C program to read and display the contents of a CSV file named "data.csv"?

#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret;
char buf[64];
fd=open("data.csv",O_RDONLY);
if(fd<0)
{
printf("faild to open the file\n");
return;
}
printf("file opened sucessfully\n");
ret=read(fd,buf,64);
if(ret<0)
{
printf("failed to write\n");
close(fd);
return;
}
buf[ret]='\0';
printf("%s\n",buf);
close(fd);
}

24. Write a C program to get the absolute path of the current working directory?

#include<stdio.h>
#include<unistd.h>
void main()
{
char path[50];
if(getcwd(path,100)!=NULL)
{
printf("%s\n",path);
return;
}
else
{
printf("error\n");
return;
}
}

25. Develop a C program to get the size of a directory named "Documents"?

#include<stdio.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<unistd.h>
void main()
{
int fd;
struct stat file;
fd=stat("documents",&file);
if(fd<0)
{
printf("error\n");
return;
}
printf("size of the documents: %ld\n",file.st_size);
}

26. Implement a C program to recursively copy all files and directories from one directory to another?


27. Write a C program to get the number of files in a directory named "Images"?

#include<stdio.h>
#include<sys/types.h>
#include<dirent.h>
void main()
{
struct dirent *name;
DIR *dir;
int c=0;
dir=opendir(".");
if(dir==NULL)
{
printf("opendir");
return;
}
printf("files in the current directory\n");
while(name=readdir(dir))
{
 c++;
 printf("%s\n",name->d_name);
}
printf("%d",c);
}

28. Develop a C program to create a FIFO (named pipe) named "myfifo" in the current directory?

#include<stdio.h>
#include<sys/stat.h>
int main()
{
   if(mkfifo("myfifo",0666)==0)
     printf("created successfully.\n");
   else 
     printf("error");
}

29. Implement a C program to read data from a FIFO named "myfifo"?

#include<stdio.h>
#include<unistd.h>
int main()
{
  int fd;
  char buf[50];
  fd=open("myfifo",O_RDONLY);
  read(fd, buf,sizeof(buf));
  printf("data read:%s\n",buf);
  close(fd);
}


30. Write a C program to truncate a file named "file.txt" to a specified length?

#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/ipc.h>
int main()
{
  int l;
  printf("enter length to truncate:");
  scanf("%d",&l);
  if(truncate("file.txt",l)==0)
	printf("file is truncated\n");
  else
     printf("error");
}

31. Develop a C program to search for a specific string in a file named "data.txt" and display the line number(s) where it occurs?

#include<stdio.h>
#include<string.h>
int main()
{
FILE *fp;
fp=open("data.txt","r");
char line[200], keyword[50];
int lineno=0;
printf("enter string to search:");
scanf("%s",keyword);
while(fgets(line,sizof(line),fp))
{
  lineno++;
  if(strstr(line,keyword))
    printf("keyword found on line:%d\n", lineno);
}
}

32. Implement a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path?

#include<stdio.h>
#include<sys/stat.h>
#include<fcntl.h>
#incllude<unistd.h>
int main()
{
 struct stat st;
 char path[100];
 printf("enter path:");
 scanf("%s",path);
 if(S_ISREG(st.st_mode))
   printf("Regular file\n");
 else if(S_ISDIR(st.st_mode))
   printf("Directory\n");
 else if(S_ISLINK(st.st_mode))
   printf("Symbolic link\n");
 else 
   printf("other type\n");
}

33. Write a C program to create a new empty file named "empty.txt"?

#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
void main()
{
int fd;
fd=open("empty.txt",O_CREAT|O_RDWR,0660);
if(fd<0)
{
printf("failed to open\n");
return;
}
printf("sucessfully opened\n");
close(fd);
}

34. Develop a C program to get the permissions (mode) of a file named "file.txt"?

#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat st;

    // Get file status
    if (stat("file.txt", &st) == -1) {
        perror("stat");
        return 1;
    }

    // Extract and display permissions in octal format
    printf("Permissions of file.txt: %o\n", st.st_mode & 0777);

    return 0;
}



35. Implement a C program to recursively delete a directory named "Temp" and all its contents?


36. Write a C program to read and display the first 10 lines of a file named "log.txt"?

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
void main()
{
FILE *fd;
fd=fopen("log.txt","r");
if(fd<0)
{
printf("failed to open\n");
return;
}
printf("opened sucessfully\n");
char buf[102];
int l_count=0;
//while((fgets(buf,sizeof(buf),fd)!=NULL) && l_count<10)
//{
while(1)
{
ret=read(fd,buf,102)
if(l_count>10)
return;
if(ret<0)
{
printf("failed to read");
return;
}
printf("%s",buf);
l_count++;
}
fclose(fd);
}

37. Develop a C program to read data from a text file named "input.txt" and write it to another file named "output.txt" in reverse order?


38. Implement a C program to create a new directory named with the current date in the format "YYYY-MM-DD"?
39. Write a C program to read and display the contents of a binary file named "binary.bin"?
40. Develop a C program to get the size of the largest file in a directory?
41. Implement a C program to check if a file named "data.txt" is readable?
42. Write a C program to create a new directory named "Logs" and move all files with the ".log" extension into it?
43. Develop a C program to check if a file named "config.ini" is writable?
44. Implement a C program to read the contents of a text file named "instructions.txt" and execute the instructions as shell commands?
45. Write a C program to get the number of hard links to a file named "file.txt"?
46. Develop a C program to copy the contents of all text files in a directory into a single file named "combined.txt"?
47. Implement a C program to recursively calculate the total size of all files in a directory and its subdirectories?
48. Write a C program to get the number of bytes in a file named "data.bin"?
49. Develop a C program to create a new directory named with the current timestamp in the format "YYYY-MM-DD-HH-MM-SS"?
50. Write a C program to create a new directory named "Documents" in the current directory?
(3)
51. Develop a C program to check if a file named "file.txt" exists in the current directory?
(4)
52. Implement a C program to open a file named "data.txt" in read mode and display its contents?
(2)
53. Write a C program to create a new text file named "output.txt" and write "Hello, World!"to it?
(1)
54. Develop a C program to delete a file named "delete_me.txt" from the current directory?
(6)
55. Implement a C program to rename a file from "old_name.txt" to "new_name.txt"?
(5)
56. Write a C program to copy the contents of one file to another file?
(7)
57. Develop a C program to move a file named "file.txt" to a directory named "Backup"?
(12)
58. Implement a C program to list all files and directories in the current directory?


59. Write a C program to get the size of a file named "data.txt"?

(10)

60. Develop a C program to create a new directory named "Pictures" in the parent directory?

61. Develop a C program to count the number of lines in a file named "log.txt"?
62. Implement a C program to append "Goodbye!" to the end of an existing file named "message.txt"?
63. Write a C program to create a symbolic link named "link.txt" to a file named "target.txt"?
64. Develop a C program to change the permissions of a file named "file.txt" to read-only?
65. Implement a C program to change the ownership of a file named "file.txt" to the user "user1?
66. Write a C program to get the last modified timestamp of a file named "file.txt"?

#include<stdio.h>
#include<sys/stat.h>
#include<stdlib.h>
#include<time.h>
void main()
{
int fd;
struct stat file;
fd=stat("file.txt",&file);
if(fd==-1)
{
printf("error\n");
return;
}
struct time *modtime;
modtime=localtime(&file.st_mtime);
if(modtime<0)
{
printf("localtime");
return;
}
printf("last modified time of %s:%s","file.txt",asctime(modtime));
}

67. Develop a C program to create a temporary file and write some data to it?

#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret;
fd=open("textfile.txt",O_CREAT|O_RDWR,0660);
if(fd<0)
{
printf("faild to open the file\n");
return;
}
printf("file opened sucessfully\n");
char buf[]="this is the temporary file";
ret=write(fd,buf,strlen(buf));
if(ret<0)
{
printf("failed to write\n");
return;
}
close(fd);
}

68. Implement a C program to get the size of a file named "image.jpg"?

#include<stdio.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<unistd.h>
void main()
{
int fd;
struct stat file;
fd=stat("image.jpg",&file);
if(fd<0)
{
printf("error\n");
return;
}
printf("size of the file: %ld\n",file.st_size);
}

69. Write a C program to create a new text file named "notes.txt" and write multiple lines of text to it?

#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret;
fd=open("notes.txt",O_CREAT|O_RDWR,0660);
if(fd<0)
{
printf("faild to open the file\n");
return;
}
printf("file opened sucessfully\n");
char buf[]="this is the temporary file\n"
           "it is named as notes\n"
           "it has 4 lines\n"
           "this is 4 th line\n";
ret=write(fd,buf,strlen(buf));
if(ret<0)
{
printf("failed to write\n");
return;
}
close(fd);
}

70. Develop a C program to count the number of words in a file named "essay.txt"?

#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret,count=0;
fd=open("essay.txt",O_RDONLY);
char buf[1];
if(fd<0)
{
printf("faild to open the file\n");
return;
}
while(ret=read(fd,buf,1)>0)
{
if(buf[0]==' '|| buf[0]=='\n')
count++;
}
if(ret<0)
{
printf("failed to read\n");
return;
}
printf("no of words :%d\n",count);
close(fd);
}

71. Write a C program to create a symbolic link named "shortcut.txt" to a file named "target.txt"?


72. Develop a C program to change the permissions of a file named "important.doc" to read and write for the owner only?

#include<stdio.h>
#include<sys/stat.h>

void main()
{
if(chmod("important.doc",S_IRUSR|S_IWUSR)==0)
printf("sucessfully changed\n");
else
printf("failed\n");
}

73. Write a C program to get the last access time of a file named "data.txt"?

#include<stdio.h>
#include<sys/stat.h>
#include<stdlib.h>
#include<time.h>
void main()
{
int fd;
struct stat file;
fd=stat("data.txt",&file);
if(fd==-1)
{
printf("error\n");
return;
}
struct time *modtime;
modtime=localtime(&file.st_mtime);
if(modtime<0)
{
printf("localtime");
return;
}
printf("last accessed time of file %s is :-%s","data.txt",asctime(modtime));
}

74. Develop a C program to read and display the contents of a CSV file named "data.csv"?

#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
int fd,ret;
char buf[64];
fd=open("data.csv",O_RDONLY);
if(fd<0)
{
printf("faild to open the file\n");
return;
}
printf("file opened sucessfully\n");
ret=read(fd,buf,64);
if(ret<0)
{
printf("failed to write\n");
close(fd);
return;
}
buf[ret]='\0';
printf("%s\n",buf);
close(fd);
}

75. Implement a C program to truncate a file named "file.txt" to a specified length?(30)
76. Write a C program to search for a specific word in a file named "data.txt" and display the line number(s) where it occurs?
77. Develop a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path?()
78. Implement a C program to read and display the contents of a binary file named "binary.bin"?
79. Implement a C program to create a new directory named "Logs" and move all files with the ".log" extension into it?
80. Write a C program to check if a file named "config.ini" is writable?
81. Develop a C program to read the contents of a text file named "instructions.txt" and execute the instructions as shell commands?
82. Develop a C program to read data from a binary file named "data.bin" and display it in hexadecimal format?
83. Develop a C program to recursively copy all files and directories from one directory to another?
84. Develop a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path?

85. What are the types of files present in Linux OS?
	Linux treats everything as a file. The main types of files are:
	
	1.Regular files – Text, binary, etc.
	
	2.Directory files – Folders that contain other files.
	
	3.Character device files – For devices like serial ports, tty, etc. (e.g., /dev/ttyS0)
	
	4.Block device files – For devices like hard disks (e.g., /dev/sda)
	
	5.Named pipes (FIFOs) – For inter-process communication.
	
	6.Symbolic links – Pointers to other files.
	
	7.Sockets – Used for inter-process or network communication.

86. How do IPC Objects, named pipes, be accessed?

	Named pipes (FIFOs) are accessed like regular files using standard file system calls:
	
	Creation: mkfifo() or mknod() system calls.
	
	Opening: open()
	
	Reading/Writing: read(), write()
	
	The named pipe appears as a file (e.g., /tmp/myfifo) and can be accessed by any process that has permission.

87. User space application sends request to Hardware by using which I/O calls?

	User space apps cannot access hardware directly. They use:
	
	System calls: open(), read(), write(), ioctl(), mmap(), close()
	
	These system calls interact with device drivers in the kernel, which then communicate with the hardware.

88. Why are basic I/O calls called universal I/O calls?

	Because calls like open(), read(), write(), and close() work uniformly across all file types:
	
	1.Regular files
	
	2.Device files
	
	3.Sockets
	
	4.Pipes

89. What is the content of the Inode Object?

	The inode (index node) contains metadata about a file:
	
		File type (regular, directory, etc.)
		
		Permissions (mode)
		
		Owner and group
		
		File size
		
		Time stamps (access, modify, change)
		
		Number of hard links
		
		Pointers to data blocks on disk (addresses)
		
		Inode number

90. In which Object information of the file gets stored?

	File metadata is stored in the inode object.
	
	Data content is stored in data blocks (referenced by the inode).

91. Kernel uses which object to represent a file?

	The kernel uses the struct file object (also called file descriptor structure) to represent an open file.
	
	It also uses:
	
	struct inode → to represent file metadata.
	
	struct dentry → for directory entry mapping.
	
	struct file_operations → for driver operations.

92. Can we access the file information present in inode from the user application? If yes, then how?

        Yes, indirectly.
	
	Use the stat() or fstat() system calls in user space.
	
	These functions retrieve inode metadata like permissions, file size, timestamps, etc.

93. Which system calls are used to access the file information?

	stat(), fstat(), lstat()
	These retrieve file metadata like size, permissions, timestamps, etc., from the inode.

94. ls command internally invokes which system call to access the file information?

	It uses stat() or lstat() system calls to retrieve metadata.
	
	Also uses opendir(), readdir(), and closedir() internally (from libc).

95. What happens after the kernel finds its inode object?

	It checks the permissions.
	
	Then creates a file object (struct file).
	
	Adds an entry into the file descriptor table.
	
	Returns a file descriptor (int) to the user process.

96. What are the contents of the file object?
	The struct file in kernel contains:
	
	File offset (cursor position)
	
	Pointer to inode object
	
	Pointer to file_operations
	
	Access mode (read/write)
	
	Flags (e.g., O_APPEND)
	
	Reference count

97. Kernel uses which object to represent the open files?

	struct file (also called file object)

98. What is the difference between the primary data and other members of the file object?

	Primary data: file offset, inode pointer, mode, flags
	
	Other members: pointers to file operations, synchronization locks, reference counts, etc.
	
	Primary data controls how file I/O operates; others are for management and access control.

99. Where does the file object base address get stored?

	Stored in the file descriptor table (an array) of the process.
	
	Each entry in the table is a pointer to a struct file.

100. When is the fd (file descriptor) table created and what is its size?

	Created when a process is created (at fork() or exec()).
	
	Default max size: 1024 descriptors (can be increased with ulimit or kernel configs).
	
	Can be accessed via /proc/<pid>/fd/

101. When is the file object created?

	Created during the open() system call.
	
	After checking inode and permissions, the kernel allocates and initializes the file object.

102. What does open() return?

	Returns a file descriptor (int), which is an index into the process’s file descriptor table.

103. When a file opens for the first time using open() in your program, what is returned?

	The lowest available file descriptor, typically 3 (since 0, 1, 2 are stdin, stdout, stderr).

104. Why does read() system call need access to file objects?

	To:
	
	Get the current file offset
	
	Determine access mode (read permission)
	
	Use file_operations->read function pointer
	
	Update offset after reading
	
	All of this info is stored in the file object.

105. Which system call is used to change the cursor position without reading or writing on a file?

	lseek()
	Used to move the file offset (read/write pointer) within the file.

106. Can we open the same file from multiple processes? Explain the memory segment in kernel space.
	Yes. Multiple processes can open the same file.
	
	Each process gets its own file descriptor.
	
	But the inode and data blocks are shared in the kernel.
	
	Kernel uses:
	
	Per-process fd table
	
	Shared struct file if descriptors are duplicated (e.g., via fork() or dup()
	
	Common inode object

107. Kernel uses which object to represent a file?

	struct file for an open file (per process)
	
	struct inode for file metadata (shared)

108. Is there any limit on number of files that can be opened from a program?
	Yes.
	
	Soft limit: Typically 1024 (can be seen with ulimit -n)
	
	Hard limit: Can be increased by root using /etc/security/limits.conf or sysctl.
	
	System-wide limit: Controlled by /proc/sys/fs/file-max

109. What are the standard I/O calls? Give some examples and what is the alternate name of standard I/O call?

	Standard I/O calls are provided by the C standard library (stdio.h).
	
	Examples:
	
	printf(), scanf(), fopen(), fread(), fwrite(), fclose()
	
	Alternate name: Buffered I/O

110. What are the basic I/O calls? Give some examples and what is the alternate name of basic I/O call?

	Basic I/O calls are system calls provided by the kernel (unistd.h).
	
	Examples:
	
	open(), read(), write(), close(), lseek()
	
	Alternate name: Unbuffered I/O or System-level I/O

111. What is the difference between basic I/O calls and standard I/O calls?

	Feature	Basic I/O (Unbuffered)	Standard I/O (Buffered)
	Header	<unistd.h>	<stdio.h>
	Buffering	No	Yes (User-level buffering)
	Performance	Lower for small I/O	Higher for small I/O
	Examples	open(), read()	fopen(), fread()
	Access level	Direct system calls	Uses basic I/O under the hood

112. Other than the basic I/O and standard I/O calls, is there any method to access the file?
	Yes:
	
	Memory-mapped I/O: Using mmap() system call
	
	Direct I/O (O_DIRECT): Bypasses the page cache
	
	Asynchronous I/O (AIO): Via aio_read(), io_uring
	
	Device-specific IOCTLs: Using ioctl()

113. How do user space applications get access to the content of inode objects?

	Indirectly, using:
	
	stat(), fstat(), lstat() → extract metadata like size, permissions, timestamps, etc.
	
	You cannot access raw inode contents directly from user space for security and abstraction reasons.

114. How do you get access to kernel objects/data structures/information present in kernel space?
	Ways include:
	
	/proc filesystem (e.g., /proc/meminfo, /proc/<pid>/status)
	
	/sys filesystem (e.g., device, driver info)
	
	System calls (e.g., stat(), uname(), ioctl())
	
	Writing a kernel module
	
	Using kprobes, ftrace, or BPF (eBPF) for tracing

115. Which system calls are used to access the file object?

	open(), read(), write(), lseek(), ioctl(), close()
	
	These use and modify the kernel’s struct file object internally.
	
	116. Which system calls are used to access the inode object?
	stat(), fstat(), lstat()
	
	These retrieve metadata stored in the inode structure.

117. How to print "Hello World" on the screen using basic I/O calls?

	#include <unistd.h>
	
	int main() {
	    write(1, "Hello World\n", 12);  // 1 is the file descriptor for stdout
	    return 0;
	}

118. Which system call is used to duplicate the file descriptor?

	dup(), dup2(), and dup3() system calls.

119. What are the advantages of dup system calls?

	Duplicate a file descriptor to:
	
	Redirect input/output (e.g., dup2(fd, STDOUT_FILENO))
	
	Share file state (offset, flags)
	
	Useful in shell piping and redirection
	
	Enables I/O redirection in system programming

120. Which object stores the ownership information of a file?

	struct inode
	Stores UID (owner) and GID (group)

121. Which command is used to display the username corresponding to ID?

        id – shows UID and GID with corresponding names
	
	whoami – shows the current user's username
	
	getent passwd <UID> – maps UID to username

122. How can we see the multiple user information in our system?
	cat /etc/passwd
	
	getent passwd
	
	users – shows currently logged-in users
	
	w or who – shows active sessions

123. Which command is used to change the UID and GID?
	usermod:
	
	Change UID: sudo usermod -u <new_uid> <username>
	
	Change GID: sudo usermod -g <new_gid> <username>
			

	
	
			

































