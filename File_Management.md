'''c
1.Write a C program to create a new text file and write "Hello, World!" to it?

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
}'''

2.Develop a C program to open an existing text file and display its contents?

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

3.Implement a C program to create a new directory named "Test" in the current directory?

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

4.Write a C program to check if a file named "sample.txt" exists in the current directory?

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

5.Develop a C program to rename a file from "oldname.txt" to "newname.txt"?

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

6.Implement a C program to delete a file named "delete_me.txt"?

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

7.Write a C program to copy the contents of one file to another?

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

8. Develop a C program to move a file from one directory to another?

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

9. Implement a C program to list all files in the current directory?

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

10. Write a C program to get the size of a file named "file.txt"?

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

11.Develop a C program to check if a directory named "Test" exists in the current directory?

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

12.Implement a C program to create a new directory named "Backup" in the parent directory?

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

13. Write a C program to recursively list all files and directories in a given directory?

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

14. Develop a C program to delete all files in a directory named "Temp"?


15. Implement a C program to count the number of lines in a file named "data.txt"?

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

16. Write a C program to append "Goodbye!" to the end of an existing file named "message.txt"?

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

17.Implement a C program to change the permissions of a file named "file.txt" to read-only?

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

18.Write a C program to change the ownership of a file named "file.txt" to the user "user1"?

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

19. Develop a C program to get the last modified timestamp of a file named "file.txt"?

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

20. Implement a C program to create a temporary file and write some data to it?

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

21. Write a C program to check if a given path refers to a file or a directory?

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
































