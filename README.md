# How-to-Talk-Recursion

April 27, 2018

I appreciate comments. Shoot me an email at noel_s_cruz@yahoo.com!

Hire me!

An algorithm is called iterative when it calls for the explicit repetition of 
some process until a certain condition is met.

Such a definition which defines an object in terms of a simpler case of itself,
is called a recursive definition.

Factorial function, multiplication of natural numbers, and fibonacci sequence 
are recursion examples.

Application of recursion to one of the most common activities in computing,
that of searching is another recursion example.

For example, we wish to look up a name in a telephone book, a word in a
dictionary, or a particular employee in a personnel file. The process used to
find such an entry is called search.

Since searching is such a common activity in computing, it is desirable to find
an efficient method for performing it. Perhaps the crudest search method is the
sequential or linear search.

If the list is unordered and haphazardly constructed, the linear search may be 
the only way to find anything in it.

Binary search

Let us apply this idea to searching an array. If the array contains only one 
element, the problem is trivial. Otherwise, compare the item being search for
with the item at the middle of the array. If they are equal, the search has
been completed successfully. If the middle element is greater than the item
being searched for, the search process is repeated in the first half of the 
array(since if the item appears anywhere it must appear in the first half);
otherwise, the process is repeated in the second half. Note that each time a
comparison is made, the number of elements yet to be searched is cut in half.
For large arrays, this method is superior to the sequential search in which
each comparison reduces the number of elements yet to be searched by only one.
Because of the division of the array to be searched into two equal parts, this
search method is called the binary search. Notice that we have quite naturally
defined a binary search recursively.

We now present a recursive algorithm to search a sorted array a for an element
x between a[low] and a[high]. The algorithm returns an index of a such that 
a[index] equals x if such an index exists between low and high. If x is not 
found in that portion of the array, binsrch returns -1 (in C, no element a[-1]
can exist).

if (low > high)

  return(-1)
  
mid = (low + high) / 2;

if (x == a[mid])

  return(mid);
  
if (x < a[mid])

  search for x in a[low] to a[mid-1];
  
else 

  search for x in a[mid + 1] to a[high];
  
Let us summarize what is involved in a recursive definition or algorithm. One
important requirement for a recursive algorithm to be correct is that it not
generate an infinite sequence of calls on itself.

There must be a "way out" of the sequence of recursive calls. Without such a 
nonrecursive exit, no recursive function can ever be completed.

April 28, 2018

Recursion in C

The C language allows the programmer to write subroutines and functions that 
call themselves. Such routines are called recursive.

The recursive algorithm to compute n! may be directly translated into a C
function as follows:

int fact(int n)

{

  int x, y;
  
  if (n == 0)
  
    return(1);
    
  x = n-1;
  
  y = fact(x);
  
  return(n * y);
  
} /* end fact */

The function fact calls itself and that is the essential ingredient of a 
recursive routine.

int mult(int a, int b)

{

  return(b == 1 ? a : mult(a, b-1) + a);
  
} /* end mult */

Compact version of recursive fact function

int fact(int n)

{

  return(n == 0 ? 1 : n * fact(n-1));
  
} /* end fact */

The compact version avoids the explicit use of local variables x(to hold the 
value of n-1) and y(to hold the value of fact(x)).

fact function revised to check its input explicitly, as follows:

int fact(int n)

{

  int x, y;
  
  if (n < 0) {
  
    printf("%s", "negative parameter in the factorial function");
    
    exit(1);
    
  } /* end if */
  
  if (n == 0)
  
    return(1);
    
  x = n-1;
  
  y = fact(x);
  
  return(n * y);
  
} /* end fact */

Fibonacci Numbers in C

int fib(int n)

{

  int x, y;
  
  if (n <= 1)
  
    return(n);
    
  x = fib(n-1);
  
  y = fib(n-2);
  
  return(x + y);
  
} /* end fib */

Binary Search in C

/* binsrch.c binary search in C program

   page 122, 132 dsucc++ book. august 27, 2017 */
   
/* int a[ARRAYSIZE];

   int x;
   
   i = binsrch(a, x, 0, n-1); */

int binsrch(int a[], int x, int low, int high)

{

  int mid;

  if (low > high)
  
    return(-1);
    
  mid = (low + high) / 2;
  
  return(x == a[mid] ? mid : x < a[mid] ?
  
    binsrch(a, x, low, mid-1) :		/* recursion technique */
    
    binsrch(a, x, mid+1, high));
    
} /* end binsrch */

/* binsrch1.c binary search in C program rewritten

   page 134 dsucc++ book. august 27, 2017 */
   
/* int a[ARRAYSIZE];

   int x;
   
   i = binsrch1(low, high);
   
   i = binsrch1(0, n-1); */

int binsrch1(int low, int high)

{

  int mid;

  if (low > high)
  
    return(-1);
    
  mid = (low + high) / 2;
  
  /* recursion technique */
  
  return (x == a[mid] ? mid : x < a[mid] ? binsrch1(low, mid-1) :
  
    binsrch1(mid+1, high));
    
} /* end binsrch1 */

Recursive Chains

A recursive function need not call itself directly. Rather, it may call itself
indirectly. More than two routines may participate in a recursive chain. Thus a
routine may call b which calls c,...which calls z, which calls a. Each routine 
in the chain may potentially call itself and is therefore recursive. Of course, 
the programmer must ensure that such a program does not generate an infinite
sequence of recursive calls.

Recursive Definition of Algebraic Expressions

As an example of a recursive chain, consider the following recursive group of
definitions:

1. An expression is a term followed by a plus sign followed by a term, or a 
   term alone.
2. A term is a factor followed by an asterisk followed by a factor, or a factor
   alone.
3. A factor is either a letter or an expression enclosed in parentheses.

Let us write a program that reads and prints a character string and then prints
"valid" if it is a valid expression and "invalid" if it is not.

int getsymb(char str[], int length, int *ppos)

{

  char c;
  
  if (*ppos < length)
  
    c = str[*ppos];
    
  else
  
    c = ' ';
    
  (*ppos)++;
  
  return(c);
  
} /* end getsymb */

We can write the main routine as follows. The standard library ctype.h includes
a function isalpha called by one of the functions below.

#include <stdio.h>

#include <ctype.h>

#define TRUE 1

#define FALSE 0

#define MAXSTRINGSIZE 100

void readstr(char *, int);

int expr(char *, int, int *);

int term(char *, int, int *);

int getsymb(char *, int, int *);

int factor(char *, int, int *);

void main()

{

  char str[MAXSTRINGSIZE];
  
  int length, pos;
  
  readstr(str, &length);
  
  pos = 0;
  
  if (expr(str, length, &pos) == TRUE && pos >= length)
  
    printf("%s", "valid");
    
  else
  
    printf("%s", "invalid");
    
} /* end main */

int expr(char str[], int length, int *ppos)

{

  /* look for a term */
  
  if (term(str, length, ppos) == FALSE)
  
    return(FALSE);
    
  /* we have found a term; look at the */
  
  /* next symbol */
  
  if (getsymb(str, length, ppos) != '+') {
  
    /* we have found the longest expression */
    
    /* (a single term). reposition pos so it */
    
    /* refers to the last position of */
    
    /* the expression */
    
    (*ppos)--;
    
    return(TRUE);
    
  } /* end if */
  
  /* at this point, we have found a term and a */
  
  /* plus sign. we must look for another term */
  
  return(term(str, length, ppos));
  
} /* end expr */

int term(char str[], int length, int *ppos)

{

  if (factor(str, length, ppos) == FALSE)
  
    return(FALSE);
    
  if (getsymb(str, length, ppos) != '*') {
  
    (*ppos)--;
    
    return(TRUE);
    
  } /* end if */
  
  return(factor(str, length, ppos));
  
} /* term */

int factor(char str[], int length, int *ppos)

{

  int c;
  
  if ((c =getsymb(str, length, ppos)) != '(')
  
    return(isalpha(c));
    
  return(expr(str, length, ppos) &&
  
    getsymb(str, length, ppos) == ')');
    
} /* end factor */

All three routines are recursive, since each may call itself indirectly. For
example, if you trace through the actions of the program for the input string
"(a*b + c*d) + (e*(f) + g)", you will find that each of the routines expr, term,
and factor calls on itself.

Writing Recursive Programs

July 20, 2019

Recursion 101

Mathematical formulas and/or functions we encounter most are straightforward
and trivial to compute. For example, to compute the area of a rectangle we
multiply the length times width or apply the formula

  A = length x width
  
In some cases, the math is defined in more complex or less standard way. For
instance, we define a function f, f(0) = 0, and f(x) = 1 + 2f(x-1) + x^2. From
this definition, we see that f(1) = 2, f(2) = 9 and f(3) = 28. A function that
is defined in terms of itself is called "recursive".

Calling itself might be confusing and lead to endless iterations. The way we
define a function in terms of itself is by using the concept of a "base case".
The base case is the value of the function for which the function is directly
known without resorting to recursion. In other words, f(4) evaluated by 
computing f(4) is infinite or non-terminating whereas f(4) evaluated by 
computing f(3) is recursive and not circular.

An important point is recursive calls will keep on going or be made until the 
base case is reached. These considerations lead to the first two fundamental
rules of recursion.

1. Base Case. You must always have some base cases, which can be solved without
   recursion.
   
2. Making Progess. For the cases that are to be solved recursively, the 
   recursive call must always be to a case that makes progress toward a base 
   case.
   
Coding the recursive function f(x) = 1 + 2f(x-1) + x^2,

int f( int x)

{

  if( x == 0 )
  
    return 0;
    
  else
  
    return 1 + 2*f(x-1) + x^2;
    
}

We need to have a terminating recursive function. 

Recursion can be used to solve problems both mathematical and non-mathematical.
An example of non-mathematical use, consider a large dictionary.

Our recursive strategy to understand words is as follows: If we know the
meaning of a word, then we are done; otherwise, we look the word up in the
dictionary. If we understand all the words in the definition, we are done;
otherwise, we figure out what the definition means by recursively looking up
the words we don't know. This procedure will terminate if the dictionary is
well defined but can loop indefinitely if a word is either not defined or 
circularly defined.

I included some posts for reference.

https://github.com/noey2020/How-to-Talk-Linked-Lists

https://github.com/noey2020/How-to-Talk-Queues

https://github.com/noey2020/How-to-Talk-Stacks

https://github.com/noey2020/How-to-Talk-Lists-Stacks-and-Queues

https://github.com/noey2020/How-to-Talk-Linear-Regression

https://github.com/noey2020/How-to-Talk-Statistics-Pattern-Recognition-101

https://github.com/noey2020/How-to-Write-SPI-STM32

https://github.com/noey2020/How-to-Write-SysTick-Handler-for-STM32

https://github.com/noey2020/How-to-Write-Subroutines-in-C-Assembly-STM32

https://github.com/noey2020/How-to-Write-Multitasking-STM32

https://github.com/noey2020/How-to-Generate-Triangular-Wave-STM32-DAC

https://github.com/noey2020/How-to-Generate-Sine-Table-LUT

https://github.com/noey2020/How-to-Illustrate-Multiple-Exceptions-

https://github.com/noey2020/How-to-Blink-LED-using-Standard-Peripheral-Library

I appreciate comments. Shoot me an email at noel_s_cruz@yahoo.com!

Hire me!

Have fun and happy coding!
