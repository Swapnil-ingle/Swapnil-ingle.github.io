### [Assignment One](https://swapnil-ingle.github.io)  ---     [Assignment Two](https://swapnil-ingle.github.io/Ass2) --- [Concepts List](https://swapnil-ingle.github.io/Concepts) --- [What is Biobank](https://swapnil-ingle.github.io/what_is_biobank) --- [Git Intuition](https://swapnil-ingle.github.io/git_for_starters)

# Concepts list.

Listed Below are the new concepts that I learned while completing the Assignments on www.swapnil-ingle.github.io

## * Internal Working of seek():
Syntax: fileObject.seek(offset,Whence)
The seek in python is similar to fseek in stdio.h. It is used to manipulate the current file pointer by the passed offset. The default value of the offset is 0.

Whereas passing 1 as argument loads the fpointer to start of the file and passing 2 loads it to the end of the file.

## * Using the time() module in python

The time module in python is used to get the current time from the operating system which can be used to know the execution time of the program. **time.time()** is a function of this module that returns the current time, which can be saved into a variable at any point in the program. The starting time can be saved into a variable **start** and at the last line of the program, we can save the end time into another variable **end**. Now subtracting start from the end we get the total execution time of the program in seconds. 

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
In my program to make a custom size .txt file, I've used mostly the os module functions. 

Some of the cardinal functions of os module, which I used, are listed below.

> **os.getcwd()**

os.getcwd() gets the current working directory in the python whichever the current program might be working in.

> **os.listdir(_path_)**

os.listdir() lists the currently present files in the *path* given as the argument to this function.

> **os.path.getsize(_path_)**

os.path.getsize(_path_) gives the size of the file/folder of the path we have given to it as an argument.

> ***os.chdir(_path_)*

os.chdir(_path_) changes the current working directory to the path given to this function as an argument.

## * Creating custom size .txt file in python

I've used the os module function, UNIX commands(In subprocess module in python) and file handling in python to create a custom size .txt file to run as a test file upon which the implementation of the program was done.

#### Execution flow:

1. Write a sample text in a file.
2. Until file size is < 500 MB
        * Read from the file.
        * Write into the same file.
3. We made a 500MB chunk.
4. Write this chunk into the same file until **output file size < required file size**.

### Python .txt file-maker

```python
import os, subprocess

path=os.getcwd()
listdir=os.listdir(path)
size=int(input('What size of file do you want(in Bytes)?'))
file_name="{}bytes.txt".format(size)
file=open('%dbytes.txt'%(size),'w')
file.close()

filesize=os.path.getsize(os.path.join(path,'%dbytes.txt'%(size)))
texttobecopied='In the third week of November, in the year 1895, a dense yellow\
fog settled down upon London. From the Monday to the Thurs-\
a day I doubt whether it was ever possible from our windows in\
Baker Street to see the loom of the opposite houses. The first day\
Holmes had spent in cross-indexing his huge book of references.\
The second and third had been patiently occupied upon a subject\
which he had recently made his hobby -- the music of the Middle\
Ages. But when, for the fourth time, after pushing back our\
chairs from breakfast we saw the greasy, heavy brown swirl still\
drifting past us and condensing in oily drops upon the window\
panes, my comrade\'s impatient and active nature could endure\
this drab existence no longer. He paced restlessly about our\
sitting-room in a fever of suppressed energy, biting his nails,\
tapping the furniture, and chafing against inaction.\
  "Nothing of interest in the paper, Watson?" he said.\
  I was aware that by anything of interest, Holmes meant any-\
a thing of criminal interest.'

file=open(file_name,'a')
file.write(texttobecopied)
file.close()

if size>500000000:
    while filesize < 500000000:
        subprocess.call('cat {} > temp.txt'.format(file_name),shell=True)
        subprocess.call('cat temp.txt>>{}'.format(file_name),shell=True)
        filesize=os.path.getsize(os.path.join(path,'%dbytes.txt'%(size)))
        print('File size now: '+str(filesize))
        subprocess.call('rm -rf temp.txt',shell=True)

#subprocess.call('echo "{}" >> {}'.format(texttobecopied,file_name),shell=True)

file=open(file_name,'r')
chunk=file.read()
file.close()

while filesize < size:
    file=open(file_name,'a')
    file.write(chunk)
    file.close()
    print('File size now: '+str(filesize))

print('Done!')

```

## * Reversing using index slicing

In python, we use a concept called as slicing to access a sub-range of elements based upon the index of a list.

For example; say we have a list; 

```python list_sample=[1,2,3,4,5,6,7,8]```

we can access the first element as ```python list_sample[0]```. The output would be 1. But if we need to access the element from the sample list from index 0 to 4 we can use slicing upon the list, the syntax of the slicing operation would be: ```python list_name[starting_index:ending_index: steps]``` 
* starting index: start from this index number.(From starting if left void)
* ending index: End slicing at *second last* from ending index.(Up to end if left void)
* steps: number of indexes to be skipped/hopped upon.(default: 1)

The great thing about these operations is that we can execute these operations upon a commonplace string also. So when we execute this bit of code it reverses the string effectively.

```python
a="This is a sample string."
b=a[::-1]
print(b)
```
We are including the whole string by leaving first two parameters void and reading backward by stepping -1 index at each time.

## * Reading from file in chunks

To read from a file in chunks of data we can use a **generator** in python.

### Generator.

> Generators are used either by calling the next method on the generator object or using the generator object in a for loop. > In python, generator functions or just generators return generator objects. These generators are functions that contain   > the yield keyword.

In generic terms, generators are functions that are used to manipulate the iterating behavior of loop in python. Generators generate a sequence of terms but they don't return the value all at once, instead, they **yield** one value at a time which is far more memory efficient.

By using generators and bypassing the **chunk_size** inside the file_object.read(_size_) a method we can effectively read from a file in chunk after chunk style.

Following is a small code snippet that helps to do so.

```python
def chunks(file,size=1024):
    while True:
        data=file.read(size)
        if data:
            yield data
        else:
            break
            
fileread=open('Input.txt','r')

for f in chunks(fileread):
        print(f)
        print("Delimiter: *------------------*")
        #Printing each chunk seperated by delimiter

```

## * Using For() Iterator on file object

As we can use for() iterator upon a generator we can also use the for iterator upon a file object. Doing so will result in the reading of the file in a line by line way.

Following is a code snippet that elaborates on how to do so.

```python
file_read=open('Input.txt','r')

for line in file_read:
        print(line)
        print("Delimiter: *------------------*")
        #Printing each line seperated by delimiter.
```

## * Subprocess module

We can use UNIX commands inside a python shell/IDLE, for such purposes we need to import the subprocess module that helps us to run UNIX commands cross-platforms, upon which they can be executed.

Mainly there are two functions that are used to execute the UNIX commands which are passed as *strings* into the functions.

**subprocess.call("Stringed UNIX command",Shell=True)**

**subprocess.check_output("Stringed UNIX command",Shell=True)**

*subprocess.call()* executes the UNIX commands while *subprocess.check_output()* returns the string output of the executed command which can be stored into a variable and also be printed.

```python
import subprocess

subprocess.call("echo 'This is UNIX print line' ",Shell=True)

```

## * UNIX commands: cat, tac, rm, mv

There are tonnes of UNIX commands which can be used extensively to carry out our 'programming' errands, however, the UNIX commands mentioned in the header are specific to file management field in UNIX. The description of the commands is mentioned below.

1. cat

> syntax: cat file0.txt file2.txt...fileN.txt > output.txt

When we use the above syntax all the left side files are **overwritten** upon the right side file(output.txt)

> syntax: cat file0.txt file1.txt .... fileN.txt >> output.txt

However, this syntax just appends the right side files upon the output.txt.

2. tac

> syntax: tac file.txt

This returns a file where the order of the file lines is inverted. For example, line1 becomes lineN, line2 becomes lineN-1... and so on.

3. rm

> syntax: rm file.txt

Removes(Deletes) a file.

> syntax: rm file1.txt file2.txt....fileN.txt

Removes(Deletes) multiple files.

4. mv

> syntax: mv filename.txt NEWfilename.txt

Renames a file.

## * Passing variables to subprocess call

To pass a variable to subprocess call we can use string formatting as the command we are passing to the subprocess.call() method are nothing but strings. So we can use string formatting to pass various variables to the commands. There is more than one way to do this.

We can use the 'C programming' native (%s as a placeholder) approach.

Or the .format method in python with {} as a placeholder to do our bidding as required.

```python
# Sample string format demo program


for i in range(20):
        print("This is line number {}!".format(i))

```
## * Dynamically creating files

To Dynamically create the .txt file we can use string formatting(To dynamically change the file name) and open them in 'Write' mode to create a new file each time.

Also, we have to maintain a *current_file_name* and a *current_file_id* which we have to change at the start of each iteration.

A simple Demo program snippet to do this is mentioned below

```python
curr_file_name="1.txt"
curr_file_id=1

for i in range(10):
        file=open(curr_file_name,'w')
        curr_file_id+=1
        curr_file_name="{}.txt".format(curr_file_id)

```

## * Using Database in python with sqlite3 module

We can use sqlite3 module in python to do all the database related operations. The steps to do so are mentioned below.

1. import sqlite3
2. connect the database
        * conn=sqlite3.connect('DBname.db')
3. Get a cursor of the current connection
        * cur=conn.cursor()
        * (This cursor would be required to make all the operation upon the Database)
        * cursor.execute() is used to execute sql queries inside python.
4. Use the cursor to fire queries from python into a database.
5. For Eg: cur.execute("INSERT INTO tableDemo VALUES('Swapnil',1)")

A sample program with more operation is given below

```python
import sqlite3

start=time.time()

conn=sqlite3.connect('test.db')
c=conn.cursor()

def Show_entry(table_name):
    for i in c.execute("SELECT * FROM {}".format(table_name)):
        print(i)

def Create_table(table_name):
    c.execute('CREATE TABLE IF NOT EXISTS {}(Name TEXT UNIQUE, value INT)'.format(table_name))
    c.execute("INSERT INTO Dict(Name,value) VALUES('computer',2),('this',1)")
    conn.commit()
    print('Table Created!')

def Drop_table(table_name):
    c.execute("DROP TABLE {}".format(table_name))
    conn.commit()
    print('Table Dropped!')

Create_table('Demo')
Show_entry('Demo')
Drop_table('Demo')

c.close()
conn.close()
```

## * Indexing in DBMS

Indexing is used in Database to efficiently access a specific row of the database, This makes the overhead of the lookup of the specific row in Database much much smaller. As we don't have to 'Search' every entry of the database instead we can go directly to the specific entry we want in our database So the time complexity everytime is O(1) for any row.

We can INDEX upon a table using the query mentioned below.

> CREATE UNIQUE INDEX index_name ON table_name (column1, column2, ...);

**OR**

> CREATE INDEX index_name ON table_name (column1, column2, ...);

## * Prepared Statements in DBMS

The overhead of executing a query from python is large so if we want to execute a query; say a million times this gets kinda heavy for the compiler and the time efficiency is greatly reduced.

So to avoid doing so and for executing *similar queries* for many numbers of times we use prepared statements in Database. 
Prepared statements pre-compiles a template of the query we want to fire and executes with all our values at the same time instead of compiling and executing each statement each time. So using the prepared statement in DBMS saves us lots of time and there is less overhead upon the compiler doing so.

To use Prepared Statements in DBMS we have to use **cursor.executemany()** command and have to pass a list of tuples of all our arguments that we need for the query to run at multiple times.

Sample program of Prepared Statement is mentioned below:

```python
import sqlite3,time

conn=sqlite3.connect('testnew.db')
c=conn.cursor()
row=0
dic={}

c.execute("DROP TABLE testing")
conn.commit()
c.execute("CREATE TABLE IF NOT EXISTS testing (Name TEXT, value INT)")
print('Table Created.!(With Prepared)')
print("//////////////////////////////\n\n")

start=time.time()

while len(dic) < 10000000:
    ran='Swap{}'.format(row)
    dic[ran]=12
    row+=1

print('Dictionary generated!---{} secs'.format(time.time()-start))
print("//////////////////////////////\n\n")

parameter=list(dic.items())

print("Inserting now into the Table...\n\n")

start=time.time()
c.executemany("INSERT INTO testing VALUES (?,?)",parameter)
print("Inserted into the table!--{} secs".format(time.time()-start))

conn.commit()
c.close()
conn.close()
```
In this program we are dynamically generating a dictionary of values that we need to update into Database that are later updated using the *executemany()* statement.
