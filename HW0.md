# Welcome to SP2020!
```C
// First, can you guess which lyrics have been transformed into this C-like system code?
char q[] = "Do you wanna build a C99 program?";
#define or "go debugging with gdb?"
static unsigned int i = sizeof(or) != strlen(or);
char* ptr = "lathe"; size_t come = fprintf(stdout,"%s door", ptr+2);
int away = ! (int) * "";

int* shared = mmap(NULL, sizeof(int*), PROT_READ | PROT_WRITE, MAP_SHARED | MAP_ANONYMOUS, -1, 0);
munmap(shared,sizeof(int*));

if(!fork()) { execlp("man","man","-3","ftell", (char*)0); perror("failed"); }
if(!fork()) { execlp("make","make", "snowman", (char*)0); execlp("make","make", (char*)0)); }

exit(0);
```

## So you want to master System Programming? And get a better grade than B?
```C
int main(int argc, char** argv) {
	puts("Great! We have plenty of useful resources for you, but it's up to you to");
	puts(" be an active learner and learn how to solve problems and debug code.");
	puts("Bring your near-completed answers the problems below");
	puts(" to the first lab to show that you've been working on this.");
	printf("A few \"don't knows\" or \"unsure\" is fine for lab 1.\n"); 
	puts("Warning: you and your peers will work hard in this class.");
	puts("This is not CS225; you will be pushed much harder to");
	puts(" work things out on your own.");
	fprintf(stdout,"This homework is a stepping stone to all future assignments.\n");
	char p[] = "So, you will want to clear up any confusions or misconceptions.\n";
	write(1, p, strlen(p) );
	char buffer[1024];
	sprintf(buffer,"For grading purposes, this homework 0 will be graded as part of your lab %d work.\n", 1);
	write(1, buffer, strlen(buffer));
	printf("Press Return to continue\n");
	read(0, buffer, sizeof(buffer));
	return 0;
}
```
## Watch the videos and write up your answers to the following questions

**Important!**

The virtual machine-in-your-browser and the videos you need for HW0 are here:

http://cs-education.github.io/sys/

The coursebook:

http://cs241.cs.illinois.edu/coursebook/index.html

Questions? Comments? Use Piazza:
https://piazza.com/illinois/spring2020/cs241

The in-browser virtual machine runs entirely in Javascript and is fastest in Chrome. Note the VM and any code you write is reset when you reload the page, *so copy your code to a separate document.* The post-video challenges (e.g. Haiku poem) are not part of homework 0 but you learn the most by doing (rather than just passively watching) - so we suggest you have some fun with each end-of-video challenge.

HW0 questions are below. Copy your answers into a text document because you'll need to submit them later in the course. More information will be in the first lab.

## Chapter 1

In which our intrepid hero battles standard out, standard error, file descriptors and writing to files.

### Hello, World! (system call style)
1. Write a program that uses `write()` to print out "Hi! My name is `<Your Name>`".
```C
#include <unistd.h>
#define STDOUT_FNO 1
#define STDOUT_FILENUM 2
int main() {
	write(1, "Hi! My name is Elliott Morales", 30);
	return 1023;
	
}
```
### Hello, Standard Error Stream!
2. Write a function to print out a triangle of height `n` to standard error.
   - Your function should have the signature `void write_triangle(int n)` and should use `write()`.
   - The triangle should look like this, for n = 3:
   ```C
   *
   **
   ***
   ```
   ```C
   #include <unistd.h>
void write_triangle(int n){
	int len;
	int lenn;
	for (len = 1; len <= n; len++){
		for (lenn = 1; lenn <= len; lenn++){
			write(1,"*",1);
		}
		write(1, "\n", 2);
	}
}
   ```
### Writing to files
3. Take your program from "Hello, World!" modify it write to a file called `hello_world.txt`.
   - Make sure to to use correct flags and a correct mode for `open()` (`man 2 open` is your friend).
   ```C
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/types.h>
int main() {
	int count;
	mode_t mode = S_IRUSR | S_IWUSR;
	int fildest = open("Hello_World.txt", O_CREAT | O_TRUNC | O_RDWR, mode);
	write(fildest, "Hi! My name is Elliott Morales", 30);
	
	return 0;}
   ```
### Not everything is a system call
4. Take your program from "Writing to files" and replace `write()` with `printf()`.
   - Make sure to print to the file instead of standard out!
```C
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/types.h>
int main() {
	int count;
	mode_t mode = S_IRUSR | S_IWUSR;
	close(1)
	int fildest = open("Hello_World.txt", O_CREAT | O_TRUNC | O_RDWR, mode);
	write( "Hi! My name is Elliott Morales");
	
	return 0;}
 ```
5. What are some differences between `write()` and `printf()`?
```C
The only action write() does is print out bytes to a file descriptor. write() does not have a buffer.  printf() is a library call that formats the bytes and print / write to the output.
```

## Chapter 2

Sizing up C types and their limits, `int` and `char` arrays, and incrementing pointers

### Not all bytes are 8 bits?
1. How many bits are there in a byte?
At least 8 bits in a byte
2. How many bytes are there in a `char`?
1 byte in a char
3. How many bytes the following are on your machine?
   - `int`, `double`, `float`, `long`, and `long long`
Int: 3 | Double: 8| Float: 4| Long: 8| Long Long: 8 
### Follow the int pointer
4. On a machine with 8 byte integers:
```C
int main(){
    int data[8];
} 
```
If the address of data is `0x7fbd9d40`, then what is the address of `data+2`?
0x7fbd9d50

5. What is `data[3]` equivalent to in C?
   - Hint: what does C convert `data[3]` to before dereferencing the address?
Data + 3 

### `sizeof` character arrays, incrementing pointers
  
Remember, the type of a string constant `"abc"` is an array.

6. Why does this segfault?
```C
char *ptr = "hello";
*ptr = 'J';
```
ptr is on the text segment and is therefore read-only
7. What does `sizeof("Hello\0World")` return?
12 bytes

8. What does `strlen("Hello\0World")` return?
5
9. Give an example of X such that `sizeof(X)` is 3.
X = “aa”
10. Give an example of Y such that `sizeof(Y)` might be 4 or 8 depending on the machine.
Y = 4

## Chapter 3

Program arguments, environment variables, and working with character arrays (strings)

### Program arguments, `argc`, `argv`
1. What are two ways to find the length of `argv`?
You can use argc or count the number of values in argv by iterating through it.

2. What does `argv[0]` represent?
 The string in argv[0] represents the program name

### Environment Variables
3. Where are the pointers to environment variables stored (on the stack, the heap, somewhere else)?
 Pointers to environment variables are on the 

### String searching (strings are just char arrays)
4. On a machine where pointers are 8 bytes, and with the following code:
```C
char *ptr = "Hello";
char array[] = "Hello";
```
What are the values of `sizeof(ptr)` and `sizeof(array)`? Why?
sizeof(ptr) =  8; sizeof(array)= 6;
This is because ptr is a pointer, which are 8 bytes, However the array is 5 characters and the null character, resulting in 6 bytes


### Lifetime of automatic variables

5. What data structure manages the lifetime of automatic variables?
Stack data structure is used for automatic variables

## Chapter 4

Heap and stack memory, and working with structs

### Memory allocation using `malloc`, the heap, and time
1. If I want to use data after the lifetime of the function it was created in ends, where should I put it? How do I put it there?
To keep data after the function lifetime has ended, you need to put it on the heap using the malloc (or calloc or realloc)  function.

2. What are the differences between heap and stack memory?
 Stack is used for static memory allocation and Heap for dynamic memory allocation. 

3. Are there other kinds of memory in a process?
 Program arguments, uninitialized variables, code, initialized variables, environment variables.

4. Fill in the blank: "In a good C program, for every malloc, there is a ___".
“free” 

### Heap allocation gotchas
5. What is one reason `malloc` can fail?
Malloc can fail if you ask to reserve too much memory, in which case it will return NULL

6. What are some differences between `time()` and `ctime()`?
ctime(): a function that takes in a time_t pointer and helpls with human readability.
time(): a system call. time(NULL) = time since 1970 
7. What is wrong with this code snippet?
```C
free(ptr);
free(ptr);
```
You cannot free a pointer twice. Trying the second time will result in undefined behavior.

8. What is wrong with this code snippet?
```C
free(ptr);
printf("%s\n", ptr);
```
 The pointer has been freed already, and ptr is a dangling pointer.

9. How can one avoid the previous two mistakes? 
After you free a pointer, set its value to 0 (a.k.a. NULL). This prevents dangling pointers.

### `struct`, `typedef`s, and a linked list
10. Create a `struct` that represents a `Person`. Then make a `typedef`, so that `struct Person` can be replaced with a single word. A person should contain the following information: their name (a string), their age (an integer), and a list of their friends (stored as a pointer to an array of pointers to `Person`s).
```C
struct Person {
	int age;
	char* name;
	struct Person** friends_list;
};

typedef struct Person person_t;


```
11. Now, make two persons on the heap, "Agent Smith" and "Sonny Moore", who are 128 and 256 years old respectively and are friends with each other.
```C
#include <stdlib.h>
struct Person {
	int age;
	char* name;
	struct Person** friends_list;
};

typedef struct Person person_t;

int main() {
	person_t* pers1 = (person_t*) malloc(sizeof(person_t));
	person_t* pers2 = (person_t*) malloc(sizeof(person_t));
	
	pers1->name = "Agent Smith";
	pers1->age = 128;
	
	pers2->name = "Sonny Moore";
	pers2->age = 256;
	
	pers2->friends = pers1;
	pers1->friends = pers2;

	free(pers1->name);
	free(pers2->name);

	free(pers1->name);
	free(pers2->name);
	
	return 0;}


```
### Duplicating strings, memory allocation and deallocation of structures
Create functions to create and destroy a Person (Person's and their names should live on the heap).
12. `create()` should take a name and age. The name should be copied onto the heap. Use malloc to reserve sufficient memory for everyone having up to ten friends. Be sure initialize all fields (why?).
Assuming I have code from previous questions.
```C
person_t* create(char* name, int age){
  person_t* p = (person_t* ) malloc(sizeof(person_t));
  p->age = age;
  p->name = strdup(name);
  p->friends = malloc(10*sizeof(person_t*));
}

```
13. `destroy()` should free up not only the memory of the person struct, but also free all of its attributes that are stored on the heap. Destroying one person should not destroy any others.
Assuming I have code from previous questions
```C
void destroy(person_t* p){
  free(p->friends);
  p->friends = 0;
  
  free(p);
  p = 0;
}
```

## Chapter 5 

Text input and output and parsing using `getchar`, `gets`, and `getline`.

### Reading characters, trouble with gets
1. What functions can be used for getting characters from `stdin` and writing them to `stdout`?
putchar() and getchar()
2. Name one issue with `gets()`.
Cannot control the amount of bytes it takes in.
### Introducing `sscanf` and friends
3. Write code that parses the string "Hello 5 World" and initializes 3 variables to "Hello", 5, and "World".
```C
#include <stdio.h>
int main(){
    char* input = ("Hello 5 World");
    char first[10];
    int second = 5;
    char third[10];
    sscanf(input, "%s %d %s", first, & second, third);
    return 0;
}

```
### `getline` is useful
4. What does one need to define before including `getline()`?
_GNU_SOURCE
5. Write a C program to print out the content of a file line-by-line using `getline()`.
```C
#include <stdio.h>
int main()
{
    char file_name[241];
    FILE* fptr = NULL;

    fptr = fopen(file_name, "r");
    if (!fptr){
        printf("Bad file\n");
        exit(0);
    }
    char* buff = NULL;
    int cap = 0;
    while (getline(&buff, &cap, fptr)){
        printf("%s\n", buffer);
    }
    fclose(fptr);
    free(buffer);
    return 0;
}
```
## C Development

These are general tips for compiling and developing using a compiler and git. Some web searches will be useful here

1. What compiler flag is used to generate a debug build?

2. You modify the Makefile to generate debug builds and type `make` again. Explain why this is insufficient to generate a new build.

3. Are tabs or spaces used to indent the commands after the rule in a Makefile?

4. What does `git commit` do? What's a `sha` in the context of git?

5. What does `git log` show you?

6. What does `git status` tell you and how would the contents of `.gitignore` change its output?

7. What does `git push` do? Why is it not just sufficient to commit with `git commit -m 'fixed all bugs' `?

8. What does a non-fast-forward error `git push` reject mean? What is the most common way of dealing with this?


## Optional (Just for fun)
- Convert your a song lyrics into System Programming and C code and share on Piazza.
- Find, in your opinion, the best and worst C code on the web and post the link to Piazza.
- Write a short C program with a deliberate subtle C bug and post it on Piazza to see if others can spot your bug.
- Do you have any cool/disastrous system programming bugs you've heard about? Feel free to share with your peers and the course staff on piazza.
