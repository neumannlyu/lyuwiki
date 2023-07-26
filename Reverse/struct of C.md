# What is struct?

C struct is a user-defined data type that allows data items of different types to be combined to form a single entity. Structs are a way of representing composite data in C language that allows multiple members to be defined in a single struct. Each member can be of different data type.

A struct defined using the keyword `struct`, followed by the name of the struct, followed by a list of members of the struct. Structure members are separated by commas and terminated with a semicolon. For example:

```c
struct Person {
    char name[50];
    int age;
    float height;
};
```



# define struct

The struct definition consists of the keyword `struct` and the struct name, which can be customized according to your needs. A struct statement defines a new data type that contains multiple members. The struct statement has the following foramt :

```c
struct tag {
    member-list
    member-list
    member-list  
    ...
} variable-list;
```

**tag** is a structure tag.

**member-list** is the standard variable definition, such as `int i;` or `float f;`, or other valid variable definition. 

**variable-list** is struct variables, defined at the end of the struct, before the last semicolon. You can specify one or more struct variables. Here is how to declare the struct of the Book:

```c
struct Books
{
  char title[50];
  char author[50];
  char subject[100];
  int book_id;
} book; 
```

under normal circumstances, at least 2 of the 3 parts of **tag, member-list, variable-list** must appear. Here are some examples:

```c
// declaration 1:
// this declaration declares a struct with 3 members, namely an integer a, a character b, and a double-precision c.
// this structure does not have a tag.
struct
{
    int a;
    char b;
    double c;
} s1;

// declaration 2:
// this declaration declares a struct with 3 members, namely an integer a, a character b, and a double-precision c.
// the tag of struct named SIMPLE, and no variables.
struct SIMPLE
{
    int a;
    char b;
    double c;
};
// The variables t1, t2, and t3 are declared using the struct of the SIMPLE tag. 
struct SIMPLE t1, t2[20], *t3;


// declaration 3:
// You also can use a typedef keyword to create a new type.
typedef struct
{
    int a;
    char b;
    double c;
} Simple2;
// you can declare a new struct variable with simple2 as a type.
Simple2 u1, u2[20], *u3;
```

in the above declaration, the first and the second declarations are treated by the compiler as two completely different types, even if their members list is the same. `t3 = &s1` is illegal.

the members of a struct can contain other structs, as well as pointers to its struct type. The point often used to implement more advanced data structure such as lists and trees.

```c
// the declaration of this struct contains other structs.
struct COMPLEX
{
    char string[100];
    struct SIMPLE a;
};

// the declaration of this struct contains a pointer to its own type.
struct NODE
{
    char string[100];
    struct NODE *next_node;
};
```

if two structs contain each other, incomplete declaration of one of two structs is required, as follows:

```c
struct B;    // making an incomplete declaration of struct B.

// struct A contains a pointer to struct B.
struct A
{
    struct B *partner;
    // other members;
};

// struct B contains a pointer to struct a, and after a declared, B is declared as well.
struct B
{
    struct A *partner;
    // other members;
};
```



# initialization of struct variables

As with other types of variables, struct variables can be defined with an initial value.

## example 

```c
#include <stdio.h>

struct Books
{
  char title[50];
  char author[50];
  char subject[100];
  int book_id;
} book = {"C Language", "RUNOOB", "Programming Language", 123456};

int main()
{
  printf("title : %s\nauthor: %s\nsubject: %s\nbook_id: %d\n", book.title, book.author, book.subject, book.book_id);
}
```

执行输出结果为：

```
title : C Language
author: RUNOOB
subject: Programming Language
book_id: 123456
```

# access members of struct

to access members of struct, we use the **member access operator (.)**. Member access operator is a period between the name of the struct variable and the struct member we want to access. The following example demonstrates the use of struct:

```c
#include <stdio.h>
#include <string.h>
 
struct Books
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
};
 
int main( )
{
   struct Books Book1;        /* declare Book1, of type Books */
   struct Books Book2;        /* declare Book2, of type Books */
 
   /* Book1 details */
   strcpy( Book1.title, "C Programming");
   strcpy( Book1.author, "Nuha Ali");
   strcpy( Book1.subject, "C Programming Tutorial");
   Book1.book_id = 6495407;

   /* Book2 details */
   strcpy( Book2.title, "Telecom Billing");
   strcpy( Book2.author, "Zara Ali");
   strcpy( Book2.subject, "Telecom Billing Tutorial");
   Book2.book_id = 6495700;
 
   /* print Book1 infomation */
   printf( "Book 1 title : %s\n", Book1.title);
   printf( "Book 1 author : %s\n", Book1.author);
   printf( "Book 1 subject : %s\n", Book1.subject);
   printf( "Book 1 book_id : %d\n", Book1.book_id);

   /* print Book2 information */
   printf( "Book 2 title : %s\n", Book2.title);
   printf( "Book 2 author : %s\n", Book2.author);
   printf( "Book 2 subject : %s\n", Book2.subject);
   printf( "Book 2 book_id : %d\n", Book2.book_id);

   return 0;
}
```

When the above code is complied and executed, it produces the following results:

```c
Book 1 title : C Programming
Book 1 author : Nuha Ali
Book 1 subject : C Programming Tutorial
Book 1 book_id : 6495407
Book 2 title : Telecom Billing
Book 2 author : Zara Ali
Book 2 subject : Telecom Billing Tutorial
Book 2 book_id : 6495700
```

# struct as a function parameters

You can use a struct as a function argument, passing arguments in the same way as other types or Pointers. You can access struct  variables in the way shown in the example above:

```c
#include <stdio.h>
#include <string.h>
 
struct Books
{
  char title[50];
  char author[50];
  char subject[100];
  int  book_id;
};

/* declare function */
void printBook( struct Books book );
int main( )
{
  struct Books Book1;     /* declare Book1，type Books */
  struct Books Book2;     /* declare Book2, type Books */

  /* Book1 detail */
  strcpy( Book1.title, "C Programming");
  strcpy( Book1.author, "Nuha Ali");
  strcpy( Book1.subject, "C Programming Tutorial");
  Book1.book_id = 6495407;

  /* Book2 detail */
  strcpy( Book2.title, "Telecom Billing");
  strcpy( Book2.author, "Zara Ali");
  strcpy( Book2.subject, "Telecom Billing Tutorial");
  Book2.book_id = 6495700;
 
  /* print Book1 information  */
  printBook( Book1 );

  /* print Book2 information */
  printBook( Book2 );

  return 0;
}
void printBook( struct Books book)
{
  printf( "Book title : %s\n", book.title);
  printf( "Book author : %s\n", book.author);
  printf( "Book subject : %s\n", book.subject);
  printf( "Book book_id : %d\n", book.book_id);
}
```

when the above code is complied and executed, it produced the following results:

```c
Book title : C Programming
Book author : Nuha Ali
Book subject : C Programming Tutorial
Book book_id : 6495407
Book title : Telecom Billing
Book author : Zara Ali
Book subject : Telecom Billing Tutorial
Book book_id : 6495700
```



# a pointer to a struct

You can define pointers to structs in a similar way to defining pointers to variables of other types, as follows:

```c
struct Books *struct_pointer;
```

now, you can store the address of struct variable in the pointer variable defined above. To find the address of structure variable, place & operator before the structure name, as follows:

```c
struct_pointer = &Book1;
```

in order to access a member of a struct using a pointer to that structure, you must use `->` operator, as follows:

```c
struct_pointer->title;
```

let us rewrite the example above using structure pointers.


```c
#include <stdio.h>
#include <string.h>

struct Books
{
  char title[50];
  char author[50];
  char subject[100];
  int  book_id;
};

/* declare function */
void printBook( struct Books *book );
int main( )
{
  struct Books Book1;     /* declare Book1，type Books */
  struct Books Book2;     /* declare Book2，type Books */

  /* Book1 details */
  strcpy( Book1.title, "C Programming");
  strcpy( Book1.author, "Nuha Ali");
  strcpy( Book1.subject, "C Programming Tutorial");
  Book1.book_id = 6495407;

  /* Book2 details */
  strcpy( Book2.title, "Telecom Billing");
  strcpy( Book2.author, "Zara Ali");
  strcpy( Book2.subject, "Telecom Billing Tutorial");
  Book2.book_id = 6495700;

  /* Output Book1 infomation by passing the address of Book1 */
  printBook( &Book1 );

  /* Output Book2 information by passing the address of Book2 */
  printBook( &Book2 );

  return 0;
}
void printBook( struct Books * book)
{
  printf( "Book title : %s\n", book->title);
  printf( "Book author : %s\n", book->author);
  printf( "Book subject : %s\n", book->subject);
  printf( "Book book_id : %d\n", book->book_id);
}


```

when the above code is compiled and executed, it produces the following results: 

```c
Book title : C Programming
Book author : Nuha Ali
Book subject : C Programming Tutorial
Book book_id : 6495407
Book title : Telecom Billing
Book author : Zara Ali
Book subject : Telecom Billing Tutorial
Book book_id : 6495700
```

# Structure Size

structs have an alignment value, which can usually be set in the compiler. Alignment values are generally 1, 2, 4, 8, and 16-byte alignment. Other values may not compile.

The struct size follows the following rules:

Let the complier alignment value of structs be **zp**;

let the difference between the address of the struct member and the first address of the struct be **offset**;

let the type of struct member be **member type**;

**The size of each member must meet:**

```
// compute the size of the struct members.
offset % min(zp, sizeof(member type)) == 0
```

Let the size of struct itself be **structAlignment**,

```
structAlignment = max(member1, member2, ... menberN)
```

The alignment value of the structure itself is the minimum of the  compiler alignment value and the maximum members type.

Let the size of the struct be **size**;

**The size of struct must meet:**

```
size % min(zp, structAlignment) == 0
```

For example:

```c
struct tag
{
char chars[6];
int integer;
float flaot;
double duoble;
bool boolean;
short shrot;
};
```

**the default alignment value in the VC6.0 is 8-bytes.**

![image-20230726174536941](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307261745417.png)

The offset of chars is 0;

the offset of interger is 8;

 the offset of flaot is 12;

the offset of duoble is 16;

the offset of boolean is 24;

the offset of shrot is 28;

the alignment value of the struct itself is 8;

the size of the tag is 32.

**Change the alignment value to 4-byte alignment, the size of the same struct will change.**

![image-20230726175532643](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307261755679.png)

The offset of chars is 0;

the offset of interger is 8;

 the offset of flaot is 12;

the offset of duoble is 16;

the offset of boolean is 24;

the offset of shrot is 26;

the alignment value of struct itself is 4;

the size of the tag is 28.

![image-20230726195025334](https://pics-place.oss-cn-shanghai.aliyuncs.com/pic/202307261950368.png)

# Size of nested struct

Let the alignment value be 8-byte.

```c
struct inner
{
int m1;   // 0
char m2;  // 4
short m3; // 6
char m4;  // 8
};

struct tag
{
char chars[6]; // 0
int integer; // 8
float flaot; // 12
double duoble;// 16
inner in; // 24 
bool boolean; //36
short shrot;//38
};
```

first calculate the size of the inner struct and the alignment value of the struct itself. The size of the inner struct is 12 bytes. The alignment value is 4-byte.  

The offset value of struct variable needs to meet the following:

```
offset % structAlignment == 0
```

So, the size of tag struct is 40 types. 

# Custom alignment value

When we need to individually control the alignment value of a structure, we can use the `#pragma` preprocessing instruction.

```c
// store the current alignment value
#pragma pack(push) 

// set the alignment value is 1
#pragma pack(1)

// restore the alignment value
#pragma pack(pop)
```



# address of members

Let the type of struct to be structType, A variable `obj` be structType type.

```
obj,member address is:
(int)(&obj) + the offset of the member

obj,member equal:
 *(member type*)(the address of the member)
```

For example:

**The default alignment value is 8-byte.**

```c
struct inner
{
int m1; //0
char m2; //4
short m3; //6
char m4;
};

struct tag
{
char chars[6];  // 0
int integer; // 8
float flaot; //12
double duoble; // 16
inner in; // 24
bool boolean;
short shrot;
};

int main()
{
struct tag *obj = NULL;
printf("%d\r\n",  &obj->flaot);//  0+ 12
printf("%d\r\n",  &obj->in.m3); //  30
return 0;
}
```

the above code is compiled and executed, it produces the following results:

```
12
30
```

# offsetof

Using `offsetof` function to get the offset of the struct member.

```c
#include <stdio.h>
#include <stddef.h>

struct inner
{
int m1; //0
char m2; //4
short m3; //6
char m4;
};

struct tag
{
char chars[6];  // 0
int integer; // 8
float flaot; //12
double duoble; // 16
inner in; // 24
bool boolean;
short shrot;
};

int main()
{
struct tag *obj = NULL;
printf("%d\r\n",  &obj->flaot);//  0+ 12
printf("%d\r\n",  &obj->in.m3); //  30
printf("%d\r\n",  offsetof(tag, in.m3)); //  30
return 0;
}
```

the above code is compiled and executed, it produces the following results:

```
12
30
30
```
