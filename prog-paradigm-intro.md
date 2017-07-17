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

### Lecture 3: Converting between Types of Different Sizes and Bit Representations Using Pointers

short s = 45;  // two byte figure storing 45
double d = *(double *)&s;

Will start at beginning address of s and take those 2 bytes PLUS 6 more bytes that follow (regardless of what they are)

store 2 byte short for #1 
00000000 00000001 (big endian)
00000001 00000000 (little endian)

// They differ in where they add the padding zeros

struct fraction {
  int nom;
  int denom;
}
fraction pi;

// 8 bytes set aside for fraction - sum of all it's parts
// 4 bytes stacked on top of 4 bytes
// address of entire struct is always coincident w/address of first field


[.  7    ] pi.denom
[.  22   ] pi.nom

pi.nom = 22;
pi.denom = 7;

// "stores 22 to field that's at an offset of zero from base address of entire struct"

(fraction *)&(pi.denom) // goes to address of pi.denom and takes it and 4 bytes above it

((fraction *)&(pi.denom)) -> num = 12; // the memory slot for pi.denom will actually be 12
((fraction *)&(pi.denom)) -> denom = 33; // slot above pi.denom will have a 33 and no way to access it

int array[10] // creates an array of integers with 10 slots 0 - 9

// array is synonimous with &array[0]
// array + k is synonimous with &array[k]   k isn't added verbatim it's scaled by the type of k (e.g. 4 bytes for integer)
// *array is synonimous with array[0]
// *(array + k) is synonimous with array[k]

// there are no out of bounds checks on raw arrays

array[10] == 47; //will go forward to what would have been the next spot in the array and writes a 47 to it (regardless of what's there)

// you're supposed to write code that's consistent with the amount of space you told it to allocate

// All local variables are packed into the same memory block (activation record) so if you're overwriting
// other spaces in memory you're actually messing with your other local variables

int arr[5];
arr[3] = 128;
(short*)arr //arr is brainwashed into thinking it contains shorts (and there's room for 10 shorts in arr)
((short*)arr)[6] = 2;  //overwrites the first part of bytes of the 128 int
cout << arr[3]<<endl; // endl; is shortcut for getting a new line

((short *)(((char *)(&arr[1])) + 8))[3] == 100; // will write 100 wayyyyyy after the actual array

// strings don't exist in C and are always represented by character arrays with null character at the end
struct student{
  char *name;
  char suid[8];
  int numUnits;
}

[.       ].  // this is what it looks like in memory
[][][][]
[][][][]
[.       ]

student pupils[4]. // get 4 of the memory thing above laid out side by side

pupils[0].numUnits = 21; // will go in the numUnits field of the very first pupil
pupils[2].name = strdup("Adam"); 

// strdup is shorthand for string duplicate, dynamically allocates memory from heap to make the string "Adam" (5 bytes a d a m null)


strcpy(pupils[1].suid, "40415xx");

// doesn't allocate any memory, assumes address you should copy string is identified by first arguments
// strcpy loops through, copying chars until it hits the '\0' which is copied as well

// c has not templates and no references so there are very few meanings of ampersand (&)

int x =7;
int y = 117;
swap(&x,&y)

void swap(int *ap, int *bp) {
  int temp = *ap;
  *ap = *bp;
  *bp = temp;
}

### Handout 1: Introducing the Standard Template Library (C++)
// Two useful references for learning more about the STL
http://www.dinkumware.com/manuals/default.aspx
http://www.sgi.com/tech/stl

PAIR
  - template struct with 2 fields

template <class U, class V>
struct pair {
  U first;
  V second;
  pair(const U& first = U(), const V& second = V()) :
  first(first), second(second) {}
};

template <class U, class Y>
pair<U, V> make_pair(const U& first, const V& second);

  - struct (not a class)
  - can freely access first and second (no private access modifiers)
    - if it had been declared as a class, first and second would have been private by default
  * all associative containers (map, hash_map, multimap) require a pair in order to insert new data

map<string, int> portfolio;
portfolio.insert(make_pair(string("LU"), 400));
portfolio.insert(make_pair(string("AAPL"), 80));
portfolio.insert(make_pair(string("GOOG"), 6500));

VECTOR
  - type-safe, sequential container class that behaves like an array
    - grows and shrinks automatically in response to insertion and deletion
    - responds to same syntax as arrays
    * used more often than any other STL class (usually in place of an array)

    - to use it you must include this at the top of the file

  #include <vector>
  using namespace std;

  - iterate through using a standard for-loop construction
  - iterators behave like pointers (and often are when encapsulated els are laid out sequentially in memory)
  - removing an element causes all pointers to be invalidated (as vector will be resized)


MAP
  - boiler plate required to use it

#include <map>
using namespace std;

  - generic symbol table
  - allows you to specify data type of key and value

typedef map<string, vector<string> > chordMap;
chordMap jazzChords;

vector<string> cmajor;
cmajor.push_back("C");
cmajor.push_back("E");
cmajor.push_back("G");

pair<chordMap::iterator, bool> result =
  jazzChords.insert(make_pair(string("C Major"), cmajor));

  - insert won''t insert if the key already exists with a value
  - return value of insert is pair
    - 2nd field reports whether or not key is already bound to some other value
      - true == insertion really did something, key is new to the map
      - false == key is already present and no changes were made
    - first field of pair stores iterator addressing pair stored inside map on behalf of the key

chordMap::const_iterator found = jazzChords.find("F# minor 11");
if (found == jazzChords.end())
  cout << "Chord was never recorded." << endl;
else {
  cout << "Notes of the " << found->first << " chord:" << endl;
for (vector<string>::const_iterator note = found->second.begin();
      note != found->second.end(); ++note)
    cout << "\t" << *note << endl;
}

  - find allows you to determine if a key of interest is inside your map
    - false is `.end()` rather than `NULL` or `-1`

map<string, int> portfolio;

portfolio["LU"] = 400;
portfolio["AAPL"] = 80;
portfolio["GOOG"] = 6500;

  - operator[] allows access/update a map using array-like semantics
    - allows update of an existing key (which insert does not)

NOTE
  - all large data structures (regardless of origin) should be passed by address or by reference
    - C++ purist prefer reference (. is pretter than * and ->)
    - avoid passing by value since that involves a costly copy operation that if done right takes time and if done wrong takes time and might share data between original and copy

### Lecture 4: Creating a Generic Swap Function for Data Types of Arbitrary Size

void swap(int *ap, int *bp) {
  int temp =*ap;
  *ap = *bp;
  *bp = temp;
}

  - can't swap doubles/structs/etc because it's expecting 4 byte ints
    - anything bigger will overflow
    - how can you write it to be generic for all data types?
  - use generic pointer type (void *vpl)
    - doesnt mean it points to nothing but means you dont know the size

void swap (void *vp1, void *vp2) {
  void temp = *vp1; 
  *vp1 = *vp2;
  *vp2 = temp;
}

  - issues with this approach
    - cant declare variable as void type unless void * as generic pointer
    - cant dereference a void *

void swap (void *vp1, void *vp2, int size) {
  char buffer[size];
  memcpy(buffer, vp1, size);
  memcpy(vp1, vp2, size);
  memcpy(vp2, buffer, size);
}
  - size should be the number of bytes of the things being swapped
  - memcpy doesnt pay attention to \0, you have to tell it how many bytes to pay attention to

int x = 17, y = 37;
swap(&x, &y, sizeof(int));

double d = pi, e = e;
swap(&d, &e, sizeof(double))

  - implementation can be problematic as its easy to get the call wrong
int i = 44;
short s = 5;
swap(&i,&s, sizeof(short))

  - above will put 0101 in left most section of the 4bytes of 44 and the left most 0s from 44 into the 2 bytes of the short
  - if you did the opposite, youd end up with random parts of memory next to the short copied into the int and parts of the int overwriting random memory next to short

char * husband = strdup("Fred");
char * wife = strdup("Wilma");

  // want husband var to be associated with "Wilma" string and wife var to be associated with "Fred" string

swap(&husband, &wife, sizeof(char *));
  // char star is a pointer to the char in memory
  // |F|r|e|d|\0| & |W|i|l|m|a|\0| are memory in the heap
  // swap is swapping the addresses contained in the pointers not the values referenced by those pointers
  // Nothing happens to capital F or W (or any chars that come after them)
  // NOTE: If you mistakenly passed in husband/wife instead of &husband/&wife, you'd swap the first characters "Wilm\0" and "Freda\0"

// NOTE: Be very careful how you code when you're dealing with generics b/c the compiler won't be able to catch as many problems


// linear search (front to back of the array) returning index of first occurance of key in the array
int lsearch(int key, int array[], int size) {
  for(int i = 0; i<size; i++) {
    if(array[i] == key) {
      return i;
    }
  }
  return null;
}

// lot happens in array[i] == key which makes it not possible for this implementation to be generic
// reframe it in terms of 'a generic blob of memory' that needs to be searched
  // You'll need to tell it the size of the elements

void * lsearch(void * key, void * base, int n, int elemSize) {
  for(int i = 0; i < n; i++;) {
    void * elemAddr = (char*)base + (i * elemSize); // seduce it to think it's a one byte char star, pointer math and regular math are same if its a 1 byte el
    if (memcmp(key, elemAddr, elemSize) == 0){ //compares elemSize bytes at key address with elemSize bytes and elemAddr address and returns 0 if they match
      return elemAddr; //won't work for char pointers, c strings, structs that have pointers inside
    }
  }
}

// specify key and array by address, specify how wide the elements are, how many elements there are, compare material at address of the pointer, etc
// fine to use void * because we're going from more specific to less specific
// Should really just use function pointers


void * lsearch(void * key, void * base, int n, int elemSize, int (*cmpfn)(void *, void *)) {

}

  // 5th parameter has to be a pointer to a function that takes any two void* and returns an int

### Lecture 5: Generic Lsearch - Prototype
void * lsearch(void * key, void * base, int n, int elemSize, int (*cmpfn)(void *, void *)) {
  for(int i = 0; i < n; i++;) {
    void * elemAddr = (char*)base + (i * elemSize); // seduce it to think it's a one byte char star, pointer math and regular math are same if its a 1 byte el
    if (cmpfn(key, elemAddr) == 0){ //compares elemSize bytes at key address with elemSize bytes and elemAddr address and returns 0 if they match
      return elemAddr; //won't work for char pointers, c strings, structs that have pointers inside
    }
  }
  return NULL;
}

// have to pass in a callback/hook that tells it how to compare elements

// how it works - a real life example
int array[] = {4,2,3,7,11,6};
int size = 6;
int number = 7;

int * found = lsearch(&number, array, 6, sizeof(int), IntCmp)

if(found == NULL) //you're sad
else // you're happy

// NOTE: Best practice is to write a comparison function that matches the prototype exactly

int IntCmp(void * elem1, void * elem2) {
  int * ip1 = elem1; //modern c compiler doesn't require a cast
  int * ip2 = elem2;

  return *ip1 - *ip2; // matches will be 0, all else will be neg/pos number
}

// not best way to specify generics but this is what was available to C when it was created
// pros are that it's very memory efficient and runs fast

// more complicated when you're searching through arrays of c-strings to find matches

| Ab\0 | F#\0 | B\0 | Gb\0 | D\0 | <--character arrays full of char*


char * notes[] = {"Ab", "F#", "B", "Gb", "D"};

char * favoriteNote = "Eb";

char ** found = lsearch(&favoriteNote, notes, 5, sizeof(char *) StrCmp);

// passing in ampersand b/c we want true data type of the key that's held by lsearch to really be of the same type as all the pointers that are computed manually as part of the implementation
// allows you to compare two char **'s down below rather than a char * and a char **

int StrCmp(void * vp1, void * vp2) {
  // vp1 will be a char **
  char * s1 = *(char **)vp1;
  char * s2 = *(char **)vp2;

  return strcmp(s1, s2) // built in function return difference b/w two ascii values of non-matching chars or 0 if it matches
}

// usually built in function called lsearch & bsearch in compilers

// this is signature for the built-in bsearch method
void * bsearch (void * key, void * base, int n, int elemSize, int(*cmp)(void *, void *)) {

}

// you can pass in the address of anything you want to as long as the comparison function knows
// that the first element will be an address of something
// the final parameter must truly be a function (object-oriented-less, normal function), no methods w/addresses of objects floating around

// different between function and method is method has address of relevant object available as invisible parameter ('this','self')

// c++ pass around address of the manipulated object with keyword called 'this'

// IMPLEMENT A STACK DATA STRUCTURE!!!!
// We'll make it int specific before we do the work to make it generic -- no void stars yet
// stack.h file

typedef struct {
  int * elems;
  int logicalLen;
  int allocatedLen;
} stack;

// use this to declare the stack, but only use the methods below to actually manipulate it

void StackNew(stack * s);
void StackDispose(stack * s);
void StackPush(stack * s, int value);
void StackPop(stack * s)

stack s; //allocates the memory space but doesn't clear things out
StackNew(&s); // takes raw space, clears it out, and sets the starting variables (allocatedLen 4, logicalLen 0)

for(int i = 0; i < 5; i++){ // when it gets to the 5th element, it will need to double the length of the array and add in the next element
  stackPush(&s, i);
}

StackDispose(&s);

//stack.h file functions

void StackNew(stack * s) {
  s -> logicalLen = 0;
  s -> allocatedLen = 4;
  s -> elems = malloc(4 * sizeof(int)) // expects 1 arg that represents the # of bytes you need it to allocate
  assert(s -> elems != NULL);  // if passes, it's a no-op, if it breaks, it ends the program and lets you know why
}                              // this allows you to end the program if malloc failed rather than failing later when you try to use the memory (no segfaults or bus errors)

### Lecture 6: Integer Stack Implementation - Constructor and Destructor

// stack.c file ?need to check if this should be stack c or stack h (changed from lec 5 to 6)

void StackNew(stack *s) {
  s -> logicalLen = 0;
  s -> allocatedLen = 4;
  s -> elems = malloc (4*sizeof(int))
  asser(s -> elems != NULL);
}

void StackDispose(stack *s) {
  free(s -> elems);
}

// with initial chosen allocatedLen of 4, you can push in 4 integers w/ no problem
// when you try to add the 5th, things are going to get interesting

// simple implementation for when there's already enough memory allocated
void StackPush(stack *s, int value) {
  s -> elems[s -> logicalLen] = value;
  s -> logicalLen ++;
}

// in cases where there's not already memory allocated for new elem
// in C, will have to 
void StackPush(stack *s, int value) {
  if(s -> logicalLen == s -> allocatedLen){
    s -> allocatedLen *= 2;
    s -> elems = realloc(s -> elems, s -> allocatedLen * sizeof(int)) // no equivalent of realloc in c++
    assert(s -> elems != NULL);
  }
  
  s -> elems[s -> logicalLen] = value;
  s -> logicalLen ++;
}

// realloc sees if it can resize memory in place (if memory nearby is empty) rather than lifting it up and
//copying all of the contents over to a new block of memory
// if it can resize in place, it does (and returns same initial address) otherwise it calls malloc elsewhere and 
// replicates the current bit patterns so contents are copied to new block (returns the new initial address and frees the previously used memory)
// realloc defaults to a malloc call if the first argument is NULL (i.e. no initial address is given)
// if realloc is NULL (e.g. the memory didn't extend), the previous memory is left untouched so you don't lose what you already had


//assert works like a function but is actually a macro (#define wih arguments)

in StackPop(stack *s) {
  assert(s => logicalLen > 0);
  s -> logicalLen --;
  return s -> elems[s -> logicalLen];
}


// What do we need to do if we want to make a stack data type that works for multiple data types?
//stack.h

typedef struct {
  void * elems;
  int elemSize;
  int logLength;
  int allocLength;
  [???]
} stack;


void StackNew(stack *s, int elemSize)
void StackDispose(stack *s);
void StackPush(stack *s, void *elemAddr)
void StackPop(stack *s, void *elemAddr)

// stack.c

void StackNew(stack *s, int elemSize) {
  assert(s -> elemSize > 0)

  s -> elemSize = elemSize
  s -> logLength = 0;
  s -> allocLength = 4;
  s -> elems = malloc(4 * elemSize);

  assert(s -> elems != NULL);
}

// simple implementation that doesn't account for the fact the material inside the stack could be 
// quite complicated -- will have to revisit later
void StackDispose (stack *s) {
  free(s -> elems);
}

void StackPush (stack *s, void *elemAddr) {
  if(s -> logLength == s -> allocLength) {
    StackGrow(s);
  }

  void *target = (char *) s -> elems + s -> logLength * s -> elemSize;
  memcpy(target, elemAddr, s -> elemSize);
  s -> logLength ++;
}

// gotta cast it as a char* so you can do the pointer arithmetic since you can't do it on 
// a void* and that's what elems starts out as

// static (decorating prototype of c or c++ function) (e.g. static void StackGrow(stack *s))
    // - means this should be considered to be a private function that shouldn't be advertised outside this file

static void StackGrow(stack *s) {
  s -> allocLength *= 2;
  s -> elems = realloc(s -> elems, s -> allocLength *, s -> elemSize);
}

void StackPop (stack *s, void * elemAddr) {
  void *source = (char *) s -> elems + (s -> logLength - 1) * s -> elemSize;
  memcpy (elemAddr, source, s -> elemSize);
  s -> logLength --;s
}

// example
stack s
StackNew(&s, sizeof(int));
// imagine pushed on 14 elements
StackPop(&s, &top); //&top is address of top element

// two types of unit tests
  // those written on .c 
  // those written as a client to make sure it's running as you think it should

// for upcoming assignment, it's easy to get things to compile but hard to keep it from crashing (because you've failed to handle the pointers properly)

















