### [Assignment One](https://swapnil-ingle.github.io)  ---     [Assignment Two](https://swapnil-ingle.github.io/Ass2) --- [Concepts List](https://swapnil-ingle.github.io/Concepts)

# Concepts list.

Listed Below are the new concepts that I learned while completing the Assignments on www.swapnil-ingle.github.io

## * Internal Working of seek():
Syntax: fileObject.seek(offset,Whence)
The seek in python is similar to fseek in stdio.h. It is used to manipulate the current file pointer by the passed offset. The default value of the offset is 0.

Whereas passing 1 as argument loads the fpointer to start of the file and passing 2 loads it to the end of the file.

## * Using the time() module in python

The time module in python is used to get the current time from the operating system which can be used to know the execution time of program. **time.time()** is a funtion of this module that returns the current time, which can be saved into a variable at any point in the program. The starting time can be saved into a variable **start** and at the last line of the program we can save the end time into another variable **end**. Now subtracting start from end we get the total execution time of the program in seconds. 

```python
import time

start=time.time()

for i in range(1000):
    print("Sample Printing.!")
    
end=time.time()    
print("-----Execution time is {} secs-----".format(end-time))

```

## * Using the os() module in python

The OS module in python is used for all the operating system calls also it is used extensively for file management in python.
In my program to make a custom size .txt file I've used mostly the os module functions. 

Sme of the cardinal funtions of os module, which I used,are listed below.

> **os.getcwd()**




## * Creating custom size .txt file in python
## * Reversing using index slicing
## * Reading from file in chunks
## * Using For() Iterator on file object
## * Subprocess module
## * UNIX commands: cat, tac, rm, mv
## * Passing variables to subprocess call
## * Dynamically creating files
## * Using Database in python with sqlite3 module
## * Indexing in DBMS
## * Prepared Statements in DBMS
