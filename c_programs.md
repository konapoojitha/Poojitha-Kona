# C Programming Practice Problems

### 1.to set and clear a particular bit
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int num,pos;
    scanf("%d",&num);
    scanf("%d",&pos);
    if(num > 0)
    {
        //num = num &~(1<<pos);
        num = num | (1<<pos)
        
    }
    printf("number:%d",num);
}
```

### 2.reverse a string using poniters
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

void reverse_str(char *start,char *end)
{
   char temp;
   while(start < end)
   {
       temp = *start;
       *start = *end;
       *end = temp;
       *start++;
       *end--;
   }
}
int main()
{
    char str[] = "pointer";
    char *start = str;
    char *end = str+strlen(str)-1;
    reverse_str(start,end);
    printf("%s",str);
}
```
### 3.count no.of set bits and clear bits in a number?
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int num,setcount=0,clearcount=0;
    printf("enter number: ");
    scanf("%d",&num);
    while(num>0)
    {
        if((num & 1) == 1)
        {
            setcount++;
        }
        else
        {
            clearcount++;
        }
        num = num >> 1;
    }
    printf("setcount :%d\n",setcount);
    printf("clearcount :%d",clearcount);
}
```
### 4.check number is power of 2
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int num,setcount=0;
    printf("enter number: \n");
    scanf("%d",&num);
    while(num>0)
    {
        if(num&1 ==1)
        {
            setcount++;
        }
        num=num>>1;
    }
    if(setcount == 1)
    {
        printf("power of 2\n");
    }
    else
    {
        printf("not a power of 2");
    }
    
}
```
### 5.check number is power of 4
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int num,pos=0;
    printf("enter number: \n");
    scanf("%d",&num);
    if((num&(num-1))==0)
    {
        while(num>1)
        {
            num = num>>1;
            pos++;
        }
        if(pos%2 == 0)
        {
            printf("power of 4\n");
        }
        else
        {
            printf("not a power of 4\n ");
        }
    }
    else
    {
        printf("not a power of 4");
    }
}
```
### 6.power of 8 --> if(position % 3 == 0)

### 7.power of 16 --> if(position % 4 == 0)

### 8.set a range of bits
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int num,start=3,end=6;
    printf("enter number: \n");
    scanf("%d",&num);
    for(int i= start;i<end;i++)
    {
        num = num | (1<<i);
    }
    printf("%d",num);
    
    
}
```
### 9.clear a range of bits
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int num,start=2,end=6;
    printf("enter number: \n");
    scanf("%d",&num);
    for(int i= start;i<end;i++)
    {
        num = num &~ (1<<i);
    }
    printf("%d",num);
    
    
}
```
### 10.toggle a range of bits
```
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int num,start=2,end=6;
    printf("enter number: \n");
    scanf("%d",&num);
    for(int i= start;i<end;i++)
    {
        num = num ^ (1<<i);
    }
    printf("%d",num);
}
```
### 11.program to size of struct with and without padding
```c
#include <stdio.h>

struct normalStruct {
    char a;
    int b;
    char c;
};

struct __attribute__((packed)) packedStruct {
    char a;
    int b;
    char c;
};

int main() {
    printf("Size of normalStruct: %lu bytes\n", sizeof(struct normalStruct));
    printf("Size of packedStruct: %lu bytes\n", sizeof(struct packedStruct));
    return 0;
}
```
### 12.create an array using dma calls and find their sum of elements
```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int sum=0;
    int *arr =(int *)malloc(5*sizeof(int));
    //int *arr =(int *)calloc(5,sizeof(int));
    for(int i=0;i<5;i++)
    {
        scanf("%d",&arr[i]);
    }
    for(int i = 0;i<5;i++)
    {
        sum += arr[i];
    }
    printf("sum: %d",sum);
    
    free(arr);
}
```
### 13.simple code for function pointer
```c
#include <stdio.h>
#include <stdlib.h>

int test(int a,int b)
{
    return a+b;
}

int main()
{
    int (*fptr)(int,int) = &test;
    int res = (*fptr)(5,10);
    printf("%d",res);
}
```
### 14.program to implement function pointer in callback function
```c
#include <stdio.h>
#include <stdlib.h>

int test1(int a,int b)
{
    printf("sum:%d",a+b);
    return 0;
}

int test2(int a,int b)
{
    printf("sum:%d",a-b);
    return 0;
}

int test3(int a,int b)
{
    printf("sum:%d",a*b);
    return 0;
}

int hp(int (*fptr)(int,int))
{
    (*fptr)(2,5);
    return 0;
}


int main()
{
    hp(test1);
    hp(test2);
    hp(test3);
}
```
### 15.return a function pointer in a function
```c
#include<stdio.h>

char type_A(char x)
{
    printf("%c\n",x);
    return x;
}
char type_B(char y)
{
    printf("%c\n",y);
    return y;
}
char type_C(char z)
{
    printf("%c\n",z);
    return z;
}
char (*hp(char w))(char)
{
    if(w=='A'||w=='a')
        return type_A;
    else if(w=='B'||w=='b')
        return type_B;
    else if(w=='C'||w=='c')
        return type_C;
    else
        printf("invalid input\n");
        return NULL;
}
void main()
{
    char a;
    printf("Enter character\n");
    scanf("%c",&a);
    char(*HP)(char)=hp(a);
    if(HP!=NULL)
        HP(a);
    
}
```
### 16.find maximum and minimum element in array
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int n,temp;
    printf("enter the number of elements in an array: ");
    scanf("%d",&n);
    int arr[n];
    printf("enter the array elements: ");
    for(int i=0;i<n;i++)
    {
        scanf("%d",&arr[i]);
    }
    int min = arr[0],max = arr[0];
    for(int i=0;i<n;i++)
    {
        if(arr[i]<min)
        {
            min = arr[i];
        }
        if(arr[i]>max)
        {
            max = arr[i];
        }
    }
    printf("minimum element: %d\n",min);
    printf("maximum element: %d",max);
}
```
### 17.program to find sum in one thread and subtraction in one thread
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>

pthread_mutex_t mt;

void *sum_thread(void *args)
{
    int a = 10,b = 5;
    printf("a+b : %d\n",a+b);
}

void *sub_thread(void *args)
{
     int a = 10,b = 5;
    printf("a-b : %d",a-b);
}

int main()
{
    pthread_t th1,th2;
    pthread_create(&th1,NULL,sum_thread,NULL);
    pthread_create(&th2,NULL,sub_thread,NULL);
    
    pthread_join(th1,NULL);
    pthread_join(th2,NULL);
    
}
```
### 18.program to print even numbers in one thread and odd numbers in another thread using condition variable
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

#define NUM 10

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;

int count = 0;
int turn = 0;  // 0 for even, 1 for odd

void* print_even(void* arg) {
    while (count < NUM) {
        pthread_mutex_lock(&mutex);
        while (turn != 0)
            pthread_cond_wait(&cond, &mutex);

        if (count < NUM && count % 2 == 0) {
            printf("%d is even\n", count);
            count++;
            turn = 1;
            pthread_cond_signal(&cond);
        }

        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

void* print_odd(void* arg) {
    while (count < NUM) {
        pthread_mutex_lock(&mutex);
        while (turn != 1)
            pthread_cond_wait(&cond, &mutex);

        if (count < NUM && count % 2 == 1) {
            printf("%d is odd\n", count);
            count++;
            turn = 0;
            pthread_cond_signal(&cond);
        }

        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;

    pthread_create(&t1, NULL, print_even, NULL);
    pthread_create(&t2, NULL, print_odd, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}
 ```
### 19.program to rotate an array left by 2
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    int n,temp;
    printf("enter the number of elements: ");
    scanf("%d",&n);
    int arr[n];
    for(int i=0;i<n;i++)
    {
        scanf("%d",&arr[i]);
    }
    for(int i=0;i<=2;i++)
    {
        temp = arr[0];
        for(int j=0;j<n-1;j++)
        {
            arr[j] = arr[j+1];
        }
        arr[n-1] = temp;
    }
    for(int i=0;i<n;i++)
    {
        printf("%d",arr[i]);
    }
}
```
### 20.program to extract numbers in a string
```c
#include<stdio.h>
#include<stdlib.h>

int main()
{
    char str[] = "a1bc23d45";
    int count = 0;
    char ext[count];
    for(int i = 0;str[i] != '\0';i++)
    {
        if(str[i] >= '0' && str[i] <= '9')
        {
            ext[count++] = str[i];
        }
    }
    ext[count] = '\0';
    for(int i = 0;i<count;i++)
    {
        printf("%c ",ext[i]);
    }
    
}
```
### 21.find a substring in a string and count
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    char str[100], sub[20];
    int i, j, count = 0;

    printf("Enter main string: ");
    fgets(str, 100, stdin);  
    int len = strlen(str);
    if (len > 0 && str[len - 1] == '\n') {
        str[len - 1] = '\0';
    }

    printf("Enter substring to search: ");
    fgets(sub, 20, stdin);  
    len = strlen(sub);  // FIXED: apply strlen to sub
    if (len > 0 && sub[len - 1] == '\n') {
        sub[len - 1] = '\0';
    }

    for (i = 0; str[i] != '\0'; i++) {
        for (j = 0; sub[j] != '\0'; j++) {
            if (str[i + j] != sub[j]) {
                break;
            }
        }
        if (sub[j] == '\0') {
            count++;
        }
    }

    printf("Substring found %d time(s).\n", count);
    return 0;
}
```
### 22.count number of words in a string
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>

int main()
{
    char str[100],wcount=0;
    printf("enter a string: ");
    fgets(str,100,stdin);
    int size = strlen(str)-1;
    for(int i=0;i<=size;i++)
    {
        if(str[i] == ' ')
        {
            wcount++;
        }
    }
   printf("no of words: %d\n",wcount+1); 
}
```
### 23.linkedlists
```c
#include<stdio.h>
#include<stdlib.h>

struct linkedlist{
    int data;
    struct linkedlist *link;
};

struct linkedlist *head = NULL;

void list_creation()
{
    struct linkedlist *newnode = NULL;
    struct linkedlist *temp = NULL;
    int numofnodes;
    printf("enter the number of node u want:");
    scanf("%d",&numofnodes);
    for(int i=0;i<numofnodes;i++)
    {
        newnode = (struct linkedlist*)malloc(sizeof(struct linkedlist));
        if(newnode == NULL)
        {
            printf("error while allocating memory\n");
            return;
        }
        int value;
        printf("enter the data: ");
        scanf("%d",&value);
        newnode -> data = value;
        if(head == NULL)
        {
            head = temp = newnode;
        }
        else
        {
        temp -> link = newnode;
        temp = newnode;
        }
        
    }
    printf("\n");
}

void display()
{
    struct linkedlist *temp = head;
    while(temp)
    {
        printf("node : %d\n",temp->data);
        temp = temp->link;
    }
    printf("\n");
}

void add_node_at_beg()
{
    struct linkedlist *newnode = (struct linkedlist*)malloc(sizeof(struct linkedlist));
    newnode -> data = 100;
    newnode -> link = head;
    head = newnode;
    printf("\n");
}

void add_node_at_end()
{
    struct linkedlist *newnode = (struct linkedlist*)malloc(sizeof(struct linkedlist));
    struct linkedlist *temp = head;
    newnode -> data = 200;
    newnode -> link = NULL;
    while(temp -> link)
    {
        temp = temp -> link;
    }
    temp -> link = newnode;
    printf("\n");
}

void del_node_at_beg()
{
    struct linkedlist *temp = head;
    head = temp -> link;
    free(temp);
    printf("\n");
    
}

void del_node_at_end()
{
    struct linkedlist *temp = head;
    struct linkedlist *prev = NULL;
    while(temp -> link)
    {
        prev = temp;
        temp = temp -> link;
    }
    free(temp);
    prev -> link = NULL;
    printf("\n");
}

int main()
{
    list_creation();
    display();
    add_node_at_beg();
    display();
    add_node_at_end();
    display();
    del_node_at_beg();
    display();
    del_node_at_end();
    display();
}
```
### 24.reverse linked_list
```c
#include<stdio.h>
#include<stdlib.h>

struct linkedlist{
    int data;
    struct linkedlist *link;
};

struct linkedlist *head = NULL;
//struct linkedlist *temp;

void reverse_list()
{
    struct linkedlist *temp = NULL;
    struct linkedlist *prev = NULL;
    
    while(head)
    {
        temp  = head -> link;
        head -> link = prev;
        prev = head;
        head = temp;
    }
    head = prev;
}

void list_display()
{
 struct linkedlist *temp = head;
 if(temp == NULL)
 {
     printf("list is empty\n");
     return;
 }
 printf("reversed linked list nodes :");
 while(temp)
 {
     printf("%d",temp -> data);
     temp = temp -> link;
 }
 printf("\n");
}

void list_creation()
{
    struct linkedlist *newnode = NULL;
    struct linkedlist *temp = NULL;
    int numberofnodes=0;
    printf("enter no of nodes: ");
    scanf("%d",&numberofnodes);
    for(int i=0;i<numberofnodes;i++)
    {
    newnode = (struct linkedlist *)malloc(sizeof(struct linkedlist));
    if(newnode == NULL)
    {
        printf("error while allocating memory\n");
        return;
    }
    int value;
    printf("enter value:");
    scanf("%d",&value);
    newnode->data = value;
    newnode->link = NULL;
    if(head == NULL)
    {
        head = temp = newnode;
    }
    else
    {
        temp -> link = newnode;
        temp = newnode;
    }
    }
    printf("\n");
    
}

int main()
{
    list_creation();
    reverse_list();
    list_display();
}
```
