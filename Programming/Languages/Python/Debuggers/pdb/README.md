# pdb 

[pdb](https://docs.python.org/3/library/pdb.html) is a module for debugging python programs. The following contains how to start the debugger and example usage of some commands.

You can drop into the debugger by using built-in `breakpoint()` function.

```python
# test.py

def access(data, index):
    breakpoint()
    return data[index]

print(access([1, 2], 2))
```

```shell
$ python3 test.py
> [...](3)access()
-> return data[index]
(Pdb)
```

Or you can start the debugger from the command line. Unlike `breakpoint()`, this will debug the program from the beginning.

```shell
$ python3 -m pdb test.py
> [...](1)<module>()
-> def access(data, index):
(Pdb)
```

## help

`help` can be used to learn more about commands in the debugger. Without any argument, it will display list of available commands.

```
(Pdb) help

Documented commands (type help <topic>):
========================================
EOF    c          d        h         list      q        rv       undisplay
a      cl         debug    help      ll        quit     s        unt
alias  clear      disable  ignore    longlist  r        source   until
args   commands   display  interact  n         restart  step     up
b      condition  down     j         next      return   tbreak   w
break  cont       enable   jump      p         retval   u        whatis
bt     continue   exit     l         pp        run      unalias  where

Miscellaneous help topics:
==========================
exec  pdb

(Pdb) help help
h(elp)
        Without argument, print the list of available commands.
        With a command name as argument, print help about that command.
        "help pdb" shows the full pdb documentation.
        "help exec" gives help on the ! command.
```

## pm()

We can enter post-mortem debugging using [pm](https://docs.python.org/3/library/pdb.html#pdb.pm) method.

```python
$ python3
[...]
>>> import pdb
>>>
>>> def access(data, index):
...     return access(data[index])
...
>>> access([1,2], 2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in access
IndexError: list index out of range
>>> pdb.pm()
> <stdin>(2)access()
(Pdb) 
```

## p

`p` prints value of an expression. 

```python
$ python3
[...]
>>> import pdb
>>>
>>> def access(data, index):
...     return access(data[index])
...
>>> access([1,2], 2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in access
IndexError: list index out of range
>>> pdb.pm()
> <stdin>(2)access()
(Pdb) p data
[1, 2]
(Pdb) p index
2
(Pdb)
```

## list/ll/source

We can use `list` or `ll` commands to print source code. The difference is that `list` prints out 11 lines around the current line of debugging by default while `ll` prints out whole source code.

```python
# test.py

def access(data, index):
    breakpoint()
    return data[inedx]

print(access([1, 2], 2))
```

```python
$ python3 -m pdb test.py
> [...](3)<module>()
-> def access(data, index):
(Pdb) ll
  0     # test.py
  1
  2     def access(data, index):
  3  ->     breakpoint()
  4         return data[inedx]
  5
  6     print(access([1, 2], 2))
(Pdb) 
```

`source` will print out source code an object.

```python
# test.py

class Dog:

    def __init__(self, name):
        self.name = name
        
    def bark(self):
        print("Bark! Bark!")

bob = Dog("Bob")
bob.bark()
```

```python
$ py -m pdb .\test.py
> [...](3)<module>()
-> class Dog:
(Pdb) next
> [...](11)<module>()
-> bob = Dog("Bob")
(Pdb) next
> [...](12)<module>()
-> bob.bark()
(Pdb) source Dog
  3     class Dog:
  4
  5         def __init__(self, name):
  6             self.name = name
  7
  8         def bark(self):
  9             print("Bark! Bark!")
```

## Moving

- ### Step

Execute the current line and moves to next line. `step` follows function calls.

```python
$ python3
[...]
>>> def foo(x):
...     return x + 1
...
>>> def bar(y):
...     breakpoint()
...     z = y + foo(y)
...     return z
...
>>> bar(1)
> <stdin>(3)bar()
(Pdb) step
--Call--
> <stdin>(1)foo()
(Pdb) p x
1
(Pdb) step
> <stdin>(2)foo()
(Pdb)
--Return--
> <stdin>(2)foo()->2
(Pdb)
> <stdin>(4)bar()
(Pdb) p z
3
```

`pdb` will run previous command if nothing is entered. So, you don't need to type `step` again and again.

- ### Next

Execute the current line and moves to next line. `next` does not follow function calls.

```python
$ python3
[...]
>>> def foo(x):
...     return x + 1
...
>>> def bar(y):
...     breakpoint()
...     z = y + foo(y)
...     return z
...
>>>  bar(1)
> <stdin>(3)bar()
(Pdb) next
> <stdin>(4)bar()
(Pdb) p z
3
```

- ### Continue

Continue execution until next breakpoint.

```python
$ python3
[...]
>>> def foo():
...     a = 1
...     breakpoint()
...     a += 1
...     breakpoint()
...     return a
...
>>> foo()
> <stdin>(4)foo()
(Pdb) p a
1
(Pdb) continue
> <stdin>(6)foo()
(Pdb) p a
2
```

- ### Jump

Jump to a line number.

```python
# test.py

number = input("Enter a number: ")

if number == 10:
    print("Equals to 10")
else:
    print("Not equal to 10")
```

```python
$ py -m pdb .\test.py
> [...](3)<module>()
-> number = input("Enter a number: ")
(Pdb) next
Enter a number: 1
> [...](5)<module>()
-> if number == 10:
(Pdb) jump 6
> [...](6)<module>()
-> print("Equals to 10")
(Pdb) next
Equals to 10
--Return--
> [...](6)<module>()->None
-> print("Equals to 10")
```

## Breakpoint

`break` command can be used to set a breakpoint at a line number. Without any argument, it will list all breakpoints.

Breakpoints can also be set with a condition.

```python
# test.py

def calculate(a, b):
    a = a + 1
    b = b + a
    c = a * b
    return c

print(calculate(1, 2))
```

```python
$ py -m pdb test.py
> [...](3)<module>()
-> def calculate(a, b):
(Pdb) list
  1     # test.py
  2
  3  -> def calculate(a, b):
  4         a = a + 1
  5         b = b + a
  6         c = a * b
  7         return c
  8
  9     print(calculate(1, 2))
[EOF]
(Pdb) break 5
Breakpoint 1 at [...]:5
(Pdb) break 6, 2 > 1
Breakpoint 2 at [...]:6
(Pdb) break
Num Type         Disp Enb   Where
1   breakpoint   keep yes   at [...]:5
2   breakpoint   keep yes   at [...]:6
        stop only if 2 > 1
```

You can clear a break point using `clear` command. Without any argument, it will clear all current breakpoints.

Breakpoints can be disabled using `disable`. Unlike `clear` command, the disabled breakpoint is not removed from the list of breakpoint and can be reenable using `enable` command.

Also, you can ignore a breakpoint for a certain number of time using `ignore`.

## Where

`where`command shows stack trace with most recent frame at the bottom.

```python
$ py
[...]
>>> def c():
...     pass
...
>>> def b():
...     c()
...
>>> def a():
...     breakpoint()
...     b()
...
>>> a()
> <stdin>(3)a()
(Pdb) step
--Call--
> <stdin>(1)b()
(Pdb)
> <stdin>(2)b()
(Pdb)
--Call--
> <stdin>(1)c()
(Pdb)
> <stdin>(2)c()
(Pdb) where
  <stdin>(1)<module>()
  <stdin>(3)a()
  <stdin>(2)b()
> <stdin>(2)c()
```

## Commands

You can use `commands` to execute some commands at a certain breakpoint.

```python
# test.py

def calculate(a, b):
    a = a + 1
    b = b + a
    c = a * b
    return c

print(calculate(1,2))
```

```python
$ py -m pdb .\test.py
> [...](3)<module>()
-> def calculate(a, b):
(Pdb) list
  1     # test.py
  2
  3  -> def calculate(a, b):
  4         a = a + 1
  5         b = b + a
  6         c = a * b
  7         return c
  8
  9     print(calculate(1,2))
[EOF]
(Pdb) break 5
Breakpoint 1 at [...]:5
(Pdb) break
Num Type         Disp Enb   Where
1   breakpoint   keep yes   at [...]:5
(Pdb) commands 1
(com) p "Breakpoint hit!"
(com) p a
(com) p b
(com) end
(Pdb) continue
'Breakpoint hit!'
2
2
> [...](5)calculate()
-> b = b + a
```