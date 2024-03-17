# Input/Output (IO)
IO allows program to interact with users and other programs.
```c
// c

printf("Hello World!\n"); // Outputting text "Hello World"

int number;
scanf("%d", &number);     // Getting integer input from user and store it in variable number

```

# Variables
A variable is a named storage for containing a value. This value can be of different types (integers, floats, strings, ...). 

Some languages are *statically type* (eg. C). In most of these languages, type of a variable has to be expressed.

```c
// Assigning integer 1337 to variable named x in C
int x = 1337;
```

Some languages are *dynamically types* (eg. Python). This means that variable types are checked at runtime and usually not needed to be expressed.

```python
# Assigning string bar to variable named foo in python
foo = "bar"
```

### Modifications And Operations On Variables
After a variable is defined, it can be used to perform various operations or assign new values to it.

```python
# Python

a = 2
print(a)  # printing out the variable

b = a + 1 # adding the value of a with 1 and assign the result to variable b
b = 0     # override the previous assignment with 0
print(b)  # prints out 0
```

### Constant Variables
Constant variables are variables that cannot be altered after they are defined.

```C
// C

const int x = 1
x = 0            // gives error "error: assignment of read-only variable 'x'"
```

# Types
Followings are common data types supported by many programming languages

- ### Character
    - Example: 'a', 'b', 'c'

- ### String (Sequences of character)
    - Example: "Hello", "asdf", "1234" 

- ### Integer
    - Example: 12, 3, 1337

- ### Floats
    - Example: 1.10, 3.14, 2.1928

- ### Booleans (true and false)

- ### Arrays
    - Array is a structure consisting of multiple values of the same data type. The values can be accessed using *index number* which mostly starts at 0.

    ```C
    // C
    
    int values[3] = {1,2,3};
    printf("%d", values[1]); // prints out 2
    ```

# Operators

In programming languages there are assignment operator (`=`), arithmetic operator (`+`, `-`, `*`, `/`), comparison operator (`==`, `!=`, `<`, `>`), logical operator (`and`, `or`, `not`) and more.

- ### Logical Operator
    - AND
    ```
    T for true, F for false

    | A | B | A AND B |
    |---|---|---------|
    | T | T |    T    |
    | T | F |    F    |
    | F | T |    F    |
    | F | F |    F    |
    ```

    - OR
    ```
    T for true, F for false

    | A | B | A  OR  B |
    |---|---|----------|
    | T | T |     T    |
    | T | F |     T    |
    | F | T |     T    |
    | F | F |     F    |
    ```

    - Not
    ```
    T for true, F for false

    | A | Not A |
    |---|-------|
    | T |   F   |
    | F |   T   |
    ```

# Conditionals
Conditional allows program to execute different operations based on conditions.

```Java
// Java

if (true) {
    System.out.println("Execution 1"); // printed out
} 

if (false) {
    System.out.println("Execution 2"); // not printed out  
}

int a = 3;
int b = 5;

if (a == b) {
    System.out.println("a equals b");
} else if (a < b) {
    System.out.println("a is less than b"); // printed out  
}  else {
    System.out.println("a is greater than b");  
}
```

# Loops

Loops can be used to perform a block of code multiple times until a certain condition is met.

There are several types of loops but ones that are used commonly are:

- ### While Loop
```C
// C

int x = 0;

// prints out 01234
while (x < 5) {
    printf("%d", x);
    x += 1;
}

```

- ### For Loop
```C
// C

int x;

// prints out 01234
for (x = 0; x < 5; x++) {
    printf("%d", x);
}

```

- ### Do-While Loop
```C
// C

int x = 0;

// prints out 01234
do {
    printf("%d", x);
    x += 1;
} while(x < 5);
```

Do-While loop always execute the block of code at least once before the first check of the condition.

# Nested Structures

- ### Nested Loops
```C
// C

int data[3][2] = {{0, 0}, {2, 3}, {-1, 2}}; // Creating 2d array

/*

Accessing and printing out 2d array using nested for loop

The following is printed out

0 0 
2 3 
-1 2 

*/

int x, y;

for (x = 0; x < 3; x++) {
    for (y = 0; y < 2; y++) {
        printf("%d ", data[x][y]);
    }
    
    printf("\n");
}

```

- ### Nested Conditionals

```C
// C

int data[2] = {1, 2}; 

if (data[0] == 1) {
    if (data[1] == 3) {
        printf("Second element is equal to 3\n");
    } else {
        printf("Second element is not equal to 3\n"); // printed out
    }
}
```

# Functions

```
    #########      ############      ##########
    # Input #  =>  # Function #  =>  # Output # 
    #########      ############      ##########
```

A function is a named block of code that performs a specific task or calculation. Functions take arguments (input), perform defined operations based on those inputs, and may return a result (output). They can be called and executed from other parts of a program.

Below is a python function that takes two arguments and return the addition of them.
```python
def add(x, y):
    return x + y

result = add(1,2) # 3
```

In some programming languages, type of the arguments and type of the return value has to be defined.

The same add function written in C language.
```c
int add(int x, int y) {
    return x + y;
}
```

# Classes And Objects

A class serves as a template for creating objects. It defines the attributes and methods that all objects of that class will have.

```python
# python

class Enemy:
    def __init__(self, name, level):
        # Attributes of Enemy class
        self.name = name
        self.level = level
        
    # A method of Enemy class
    def attack(self):
        damage = self.level * 10
        print(self.name, "attacks and you take", damage, "damage")
        
bob = Enemy("bob", 3)
bob.attack() # prints "bob attacks and you take 30 damage"

```
### Inheritance

Inheritance allows a class to inherit attributes and methods from another class.

```python
# python

class Enemy:
    def __init__(self, name, level):
        self.name = name
        self.level = level
        
    def attack(self):
        damage = self.level * 10
        print(self.name, "attacks and you take", damage, "damage")

class Ghost(Enemy):
    def __init__(self, name, level):
        super().__init__(name, level)
    
    def disappear(self):
        print(self.name, "disappeared!")

bob = Ghost("bob", 3)
bob.attack() # prints "bob attacks and you take 30 damage"
bob.disappear() # prints "bob disappeared!"

```