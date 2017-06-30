### Lecture 1: Introduction

Procedural/Imperative Paradigm
  - verb oriented
  - function call is first thing you see
  - function name usually has strong verb in name

He wrote these on the board in this order:
C
  - imperative paradigm
  - Very similar to C++
  - More is exposed so it's easier to make memory errors
Assembly
  - the 1s and 0s that programs compile down to
C++
  - First thing you see is object (object oriented)
  - When you execute the program, it's translated to Assembly code
Concurrent Programming
  - OO/imperative are sequential programs
    - things happen in one time line
  - conc. gets two + functions to run simultaenously (or seemingly simultaneously)
  - Very useful in networking
  - Have to be directives put in place to ensure shared resources (e.g. db records) can't be modified by both at the same time
Scheme 
  - related to Lisp
  - functional programming paradigm
  - always rely on the return value of a function to move forward
  - program without side effects
  - small language (thinks it's the easiest one to master)
Python
  - modern object oriented version of Perl
  - lot of good libraries to deal with web programming
  - "Web programming can seem really boring to a lot of
people because it just seems like it’s just HTML and web pages and things like that."

Next Time: Low level pointer stuff

### Lecture 2: 
- low level memory mechanics (how things are represented in memory)
- datatypes you've dealt w/in c/c++
  - bool - true/false (could be single bit but it's complicated to do that)
  - char - 256 different characters - 1 byte
  - short - scalar # - 2 bytes
  - int - scalar # - 2 to 4 bytes
  - long - scalar # - 4 bytes
  - float - arbitarily precise numbers - 4 bytes
  - double - arbitarily precise numbers - 8 bytes

- bit (binary digit)
  - very small unit of memory that can distinguish between two different values
  - 0 or 1
  - 8 bits is a byte, 256 different values
  * chars represented in memory by 0s and 1s
    - e.g. "A" = 65 = 01000001
  * short done with 2 bytes (2^16 values)
    - 00000010 00000111 = 519

  ## NOTE TO SELF: Reread part about signed ints

- 0 followed by all ones in binary is one less than a perfect binary number (e.g. 0111111 = 64 - 1 = 63)
- binary addition works same as decimal (but with fewer places to go 1 + 1 = 0 (carry a 1))
  - look up 1s complement and 2s complement for how to deal with negative numbers in binary addition (2s complement seems to be the one people use)
    - invert all the bits and add 1 to it
  - short can represent negative two to the fifteenth through two to the fifteenth minus one

char ch = 'A';
short s = ch;  // don't need to cast, s will be equal to number representation of 'A'
  - this is because ch doesn't hold 'A' it actually holds integer value that was assigned
  - if 1 bit number is represented by 2 bits, the left side is just padded with 0s

short s = 67;  
char ch = s;  // it assumes you want the right most numbers (the first byte)
  - this is because ch can only hold 1 byte of info and a short is made up of 2 bytes


If you store an short in an int (2 bytes into 4 bytes) the sign bit is extended (so you'll still get the 'roll over' when you add negative 1)

- floats
  - instead of counting from 1, use some of the bits to represent 2^0 through 2^-9 (give you decimals)
    - would have been one way of doing it, but it's not what they went with
  - how a float is made
    - first bit is sign bit (+/-)
    - next 8 bits are unsigned integer 
    - remaining 23 bits are 2^-1, 2^-2, etc
    * is done to represent this number (-1)^2 1.xxxxx*2^exp-127


int i = 5; 00000000 00000000 00000000 00000101
float f = i;

Assigning an int to a float, or a float to an int
  - has to evaluate what number the original bit pattern corresponds to 
  - has to invent a new bit pattern that can lay down in a float variable

int i = 37;
float f = *(float*)&i;
//  doesn't evaluate i, evaluates the location of i
// &i : f will be assigned to address of 1 (type int* -> pointer to an int)
// float* : coaxes the int point type into being a float pointer type
// * : we dereference it to get the number at that address instead of the address

float f = 7.0;
short s = *(short*)&f;
// would only see the first two bytes of the float
// replicates the bit pattern of the top two bytes so you'll get that converted into a number