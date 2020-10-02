# Built-in data structures
A quick primer on built-ins is presented.


```python
# Built-in data structures
print('Lists -------- ')

# Lists
a = [1, 2, 3] # O(n)
for i, v in enumerate(a): # Get index and value
    print('index', i, 'value', v)

b = [4, 5]
for x, y in zip(a, b): # Enumerate over multiple iterables
    print(x, y)

# Append O(1)
a.append(4)
a[len(a):] = [5]

# Extend O(n)
b = list({1, 2, 3})
a.extend(b)

# Insert O(n)
a.insert(0, 100)

# Remove (value) O(n)
a.append(400)
a.remove(400) # ValueError if not match

# Pop O(1)
print('popped element', a.pop())

# Delete some index O(n)
del a[0]

# Sorting O(nlogn)
a.sort(key=lambda x: x)

# Reversing O(n)
a.reverse()
list(reversed(a))

print('Deque -------- ')

# Deques
from collections import deque
a = deque([1, 2, 3]) # O(n)
a.appendleft(0) # O(1)
a.append(4) # O(1)
print('popped element', a.pop()) # O(1)
print('popped element', a.popleft()) # O(1)

print('Set -------- ') # Average case linear for all operations, access avg O(1) because hash
a = {1, 2, 3}
b = {1, 5, 6}
a.discard(0)
a.add(1)
a.remove(1) # Raises ValueError if not present

# Common set operations
print(a | b, a & b, a ^ b)

print('Dicts -------- ')

# Dicts
# Non-hashable values (as in mutable types) may not be used as keys
a = {1: 'Hello', 2: ' ', 3: 'world'}
print(list(a)) # O(n) to get all keys

# Key iterator
it = iter(a.keys())
print(next(it))

# Popping items
a.pop(2) # Raising KeyError if no key present
print(a)
```

    Lists -------- 
    index 0 value 1
    index 1 value 2
    index 2 value 3
    1 4
    2 5
    popped element 3
    Deque -------- 
    popped element 4
    popped element 0
    Set -------- 
    {1, 2, 3, 5, 6} set() {1, 2, 3, 5, 6}
    Dicts -------- 
    [1, 2, 3]
    1
    {1: 'Hello', 3: 'world'}
    


```python
# Sequence data types (list, range, tuple, string, ...) have common operations
a = [1, 2, 3]

# Exist
print(1 in a)

# Multiply
print(a * 2)

# Slice
print(a[0::2]) # [i:j:k], where k is the step

# Extrema
print(max(a), min(a))

# Search and count
print(a.count(1))

# Clearing
a.clear()
print(a)
```

    True
    [1, 2, 3, 1, 2, 3]
    [1, 3]
    3 1
    1
    []
    


```python
list(range(0, 5))
```




    [0, 1, 2, 3, 4]



# Modules
Every .py file is a module, where you can import. The ```___name___``` global variable is defined on each imported module, which is the name of the python file. On the running file, ```___name___ = '___main___'```.


```python
# Importing
import math
print(math.sin(0))
from math import sin
print(sin(0))
from math import * # This imports all except definitions beginning with _
print(round(pi, 2))

# Naming
print(__name__)
print(math.__name__)
```

    0.0
    0.0
    3.14
    __main__
    math
    

### Runnable Modules
Can use the ```___name___``` variable to make some modules runnable.
```bash
python -m module_name [args]
```


```python
def some_function(x: int, y: int) -> int:
    return x + y
if __name__ == '__main__':
    import sys
    print(some_function(sys.argv[1], sys.argv[2]))
```

    -fC:\Users\Freem\AppData\Roaming\jupyter\runtime\kernel-7fa7bf58-a418-4e19-bbc4-229dd6d3d47a.json
    

### Search Path
As per Python documentation, there are three areas where search occurs:
* Built-in modules
* Directory of import | current directory if first is not valid (folders within do not count)
* PATH (ie. pip installs)

To optimize, modules are compiled in folder ```__pycache__/```.

Call ```dir()``` to get all defined names.


```python
import math
print(dir(math)[0:5])
```

    ['__doc__', '__loader__', '__name__', '__package__', '__spec__']
    

### Packages
Packages are folders containing subpackages or submodules. All packages must contain an ```___init___.py``` file to distinguish packages from module names. The ```__init___.py``` file is run when a package is imported. It can, for example, specify which names are to be exposed when a user imports ```*``` from the package.


```python
# In __init__.py
__all__ = ['module_1', 'module_2']
```

# I/O

### String Formatting


```python
a = 1
print(f'a is {a}.')
```

    a is 1.
    


```python
print('a is {0}.'.format(a))
import math
print('{0}'.format(round(math.pi, 2)))
```

    a is 1.
    3.14
    


```python
a = '%.2f' % 3.1415
print(a)
a = '%-6.2f6chars left of 6' % 3.1415
print(a)
```

    3.14
    3.14  6chars left of 6
    


```python
print(r'\\', '\\')
```

    \\ \
    


```python
print(bytearray(b'Hello, world!')[0])
a = bytearray(b'Hello, world!')
a[0] = 100
print(a.decode('utf8'))
# Converting the C way
print('%X' % 15)
```

    72
    dello, world!
    F
    

### Files
Very similar to C file I/O. Like in C, don't use readline() or read() unless specifying max number of bytes!


```python
fp = open('test_file.txt', 'w+') # Same convention as C
fp.write('Hello\nWorld!')
fp.close()
```


```python
with open('test_file.txt', 'r+') as fp:
    print(fp.readline()) # Read line
    print(fp.read(1)) # Read 1 byte
    print(fp.read(4)) # Read 4 bytes
    print(fp.read(100) == '!') # EOF
    print(fp.readline() == '') # EOF
with open('test_file.txt', 'r+') as fp:
    for lines in fp:
        print(lines)
with open('test_file.txt', 'r+') as fp:
    print(fp.readlines())
```

    Hello
    
    W
    orld
    True
    True
    Hello
    
    World!
    ['Hello\n', 'World!']
    


```python
with open('test_file.txt', 'w+') as fp: # Overwrites the previous file
    print(fp.write('A line here\nAnother line here')) # Bytes written
    print(fp.tell()) # File pointer shifts to EOF
    fp.seek(0) # Move the file pointer
    fp.write('B') # Overwrites byte 0
    fp.seek(0)
    print(fp.readlines())
with open('test_file.txt', 'r+') as fp: # Doesn't overwrite
    fp.seek(0, 2) # 2 for EOF
    fp.write(' APPENDING')
    fp.seek(0)
    print(fp.readlines())
```

    29
    30
    ['B line here\n', 'Another line here']
    ['B line here\n', 'Another line here APPENDING']
    

For JSON files, we can use 
```python
import json
json.dumps([1, 2, 3])
>>> '[1, 2, 3'
# fp is some file pointer
json.dump([1, 2, 3], fp)
json.load(fp)
>>> [1, 2, 3]
```

# Error Handling
Very important. Imagine you have a database connection, but some error causes program to stop. That connection still persists, eating away system resources. There are two types of errors.
1. SyntaxError is raised upon parsing the code.
2. Exceptions are raised when an error occurs during execution.


```python
try:
    a = 1 / 0
except ZeroDivisionError:
    print('Cannot divide by 0')
else:
    print(a)
finally:
    print('This is always outputted')
```

    Cannot divide by 0
    This is always outputted
    

# Scope, Closure, Classes (Including Iterators/Generators)
Names of objects hold a pointer to the object. Everything short of immutable literals are aliased.
* A namespace is a mapping of names to objects. They exist to prevent collision. For example, the namespaces of two different modules must be different.
* Scope is the namespaces accessable from a place. The searching procedure goes outwards, ie.
    1. Search the innermost namespace.
    2. If not found, search one layer outer.
    3. Second last layer is global variables.
    4. Last layer is the namespace of built-ins.
    5. Raise some sort of error if not found.
* Note that namespace of a function is where it was initially defined (static). Below is example of function closure. In theory, when f() finishes executing, it is taken off the call stack. However, a = g still has access to the namespace of f.


```python
# Consider the following
def f():
    a = 2
    def g():
        return a
    return g

a = f()
print(a()) 
```

    2
    

Because functions have access to its own namespace, and that scope searches start from the innermost namespace, there are explicit ways to make some inner scope have access to the outer scope.


```python
a = 10
def local_assignment():
    a = 5
local_assignment()
print(a)
def global_assignment():
    global a
    a = 5
global_assignment()
print(a)
def middle_ground():
    a = 7
    def testing():
        nonlocal a
        a = 8
    testing()
    print('Inside function', a)
middle_ground()
print(a)
```

    10
    5
    Inside function 8
    5
    


```python
class MyClass:
    def __init__(self, a, b): # self to reference the instance instead of the class
        # Everything outside here is shared across instances
        self.a = a
        self.b = b
    def add(self):
        print(self.a + self.b)
a = MyClass(1, 2)
a.add() # Instance object is passed as the self parameter
# Same as...
MyClass.add(a)
# So in essence, self can be named anything, but it's a strong convention to name it self
```

    3
    3
    


```python
def my_generator():
    for i in range(0, 10):
        yield i
it = my_generator()
for i in range(0, 10):
    print(next(it))
```

    0
    1
    2
    3
    4
    5
    6
    7
    8
    9
    


```python
class MyIterator:
    def __init__(self, a, b):
        self.a = a
        self.b = b
    def __iter__(self):
        return self
    def __next__(self):
        t = self.a
        self.a = self.b
        self.b = t + self.b
        return t
inst = MyIterator(1, 1)
it = iter(inst)
for i in range(0, 5):
    print(next(it))
```

    1
    1
    2
    3
    5
    


```python
class ParentClass:
    def f(self):
        print("Parent function")
    def g(self):
        print("Parent function")
class ChildClass(ParentClass):
    def g(self):
        print("Child function")
inst = ChildClass()
inst.f()
inst.g()
```

    Parent function
    Child function
    


```python

```
