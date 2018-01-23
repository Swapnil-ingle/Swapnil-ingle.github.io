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

Some of the cardinal funtions of os module, which I used,are listed below.

> **os.getcwd()**

os.getcwd() gets the current working directory in the pyhton whichever the current program might be working in.

> **os.listdir(_path_)**

os.listdir() lists the currently present files in the *path* given as the argument to this funtion.

> **os.path.getsize(_path_)**

os.path.getsize(_path_) gives the size of the file/folder of the path we have given to it as an argument.

> ***os.chdir(_path_)*

os.chdir(_path_) changes the current working directory to the path given to this funtion as an argument.

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
import os,subprocess

path=os.getcwd()
listdir=os.listdir(path)
size=int(input('What size of file do you want(in Bytes)?'))
file_name="{}bytes.txt".format(size)
file=open('%dbytes.txt'%(size),'w')
file.close()

filesize=os.path.getsize(os.path.join(path,'%dbytes.txt'%(size)))
texttobecopied='In the third week of November, in the year 1895, a dense yellow\
fog settled down upon London. From the Monday to the Thurs-\
day I doubt whether it was ever possible from our windows in\
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
thing of criminal interest.'

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

In python we use a concept called as slicing to access a sub-range of elements based upon the index of a list.

For example; say we have a list; 

```python list_sample=[1,2,3,4,5,6,7,8]```

we can access the first element as ```python list_sample[0]```. The output would be 1. But if we need to access the element from the sample list from index 0 to 4 we can use slicing upon the list, the syntax of the slicing operation would be: ```python list_name[starting_index:ending_index:steps]``` 
* starting index: start from this index number.(From starting if left void)
* ending index: End slicing at *second last* from ending index.(Upto end if left void)
* steps: number of index to be skipped/hopped upon.(default: 1)

The great thing about these operation is that we can execute these operation upon a commonplace string also. So when we execute this bit of code it reverses the string effectively.

```python
a="This is a sample string."
b=a[::-1]
print(b)
```
We are including the whole string by leaving first two parameters void and reading backwards by stepping -1 index at each time.

## * Reading from file in chunks


## * Using For() Iterator on file object
## * Subprocess module
## * UNIX commands: cat, tac, rm, mv
## * Passing variables to subprocess call
## * Dynamically creating files
## * Using Database in python with sqlite3 module
## * Indexing in DBMS
## * Prepared Statements in DBMS
