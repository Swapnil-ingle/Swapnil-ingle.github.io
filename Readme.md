# Assignment One.

> ## Subject - Take an input file and reverse its content.

This seemingly menial task took me about a week to complete, *intrigued why!?*

I'll be sharing the journey of my attempts to try solve this and the twist and turns my code took during the 9 versions.

Concluded by an excel sheet comprising of various time required to reverse various file sizes by all versions of the code.

> ### Version v1.0

The naive first version I coded at the start of the assignment. This is a generic logic that anyone with a parochial mindset, if I may say so without being harsh, would imply in their coding practice right away without hesistance or second thought(Or if you're not reading the *notes and constraints* of the assignment).

This version of program doesn't take being "memory efficient" into consideration also the system hanged, and sometimes crashed, which was my first clue during testing large files that this was not as much pragmatic approach as I thought it would be. 

> #### Logic: Generic.

* Reading directly from file as a whole string.
* Writing that string to output file.

```python
import time

start_time=time.time()

#Opening file objects
file_read=open('./Testfiles/500mb.txt','r')
file_write=open('./Testfiles/output.txt','w')

#Reading from file
text=file_read.read()

#Reversing using index slicing
data=text[::-1]

#Writing to output file
file_write.write(data)

#Closing the files
file_read.close()
file_write.close()

print("---Execution time: {} seconds ---".format(time.time() - start_time))
```
*This looked great initially.*

Execution of files even upto 500Mb took less than 2 minutes. What it lacked was the logic for proper handling of large files.

As we cannot efficiently load a larger file into memory, at once, and perform write operation. 

> *"There at approximately 1,000,000 lines in 50Mb of text file."*

> ### Version v2.0

Another approach has been experimented on in this particular version. I mean *"if we want to reverse the file as whole why not read it from backwards?"*.

> #### Logic: Reading from backend.

* Introduced to using seek() in python.
* File Cursor Manupulation.
* Passing size in read(size) method in python.

```python
'''
V2.0
'''
import time,os

start_time=time.time()

def readlast(file):
    with file as f:
        beg=f.tell()
        f.seek(0,2)
        currentpos=f.tell()
        while currentpos>=beg:
            f.seek(currentpos)
            filew.write(f.read(1))
            currentpos-=1    
        

filew=open('./Testfiles/output.txt','w')
file=open('./Testfiles/50mb.txt','r')
readlast(file)

filew.close()    
file.close()
print('\n Done!')
print("---Execution time: {} seconds ---".format(time.time() - start_time))
```
We've seeked the file cursor at the end of the file using seek(0,2) while saving the first position in the start variable and using a simple looping condition we can back-traverse and write each character by reading each char at a time using **file_object.read(1)**.

> ### Version v3.0

This is a side-activity program that reverses the file line-by-line; which fundamental is used later in the algorithm.

> #### Logic: Reversing line by line.
* No need to load the whole file into memory at once.
* For iterated the file object.

```python
import time

start_time=time.time()
file=open('testfile1.txt','r')
filew=open('output.txt','w')



for i in file:
    text=i[::-1]
    filew.write(text)
    


filew.close()    
file.close()
print('Done!')
print("---Execution time: %s seconds ---" % round(time.time() - start_time))
```
This is a more memory efficient program as the **for() iterator** in python can be used upon file objects to load one line at a time in memory and do some operation on it. Here in this program we are reversing each line using the slicing indices on the iterator variable **i[::-1]** and then writing the data to another file.

> ### Version v4.0

In this version we are implementing the algorithm required to reverse the file while reading it from front. The Algorithm used to do this job is mentioned down below step by step.

> ## Algorithm.

* *The fundamental blocks of the algorithm are the reversed lines that move from one file to another to ultimately get the whole program reversed.*
* Three files are required *input.txt*, *temp.txt* and *output.txt*.
* The logic is as we are traversing line by line from front to end. The  **_"freshly reversed line"_** should always be at the top of the stack of lines we are reversing; where comes the auxilary file temp as intermediate storage, that should be appended below the freshly reversed line.

> #### Steps.

1. read **i'th** line from *'input.txt'* and Overwrite the *'output.txt'* with the reversed line.
2. read *'temp.txt'* Append the temp data to *'output.txt'*
3. read *'output.txt'* Overwrite the *'temp.txt'* with *'output.txt'* data
4. Repead until the compiler exausts of input lines from *'input.txt'*
5. **Done!**

> ### Logic: Using the algorithm to reverse.

```python
import os,time

start=time.time()
fr = open('./Testfiles/1mb.txt','r')
fw = open('./Testfiles/output1.txt','a')
ft = open('./Testfiles/temp.txt','a')

for i  in fr:
        data = i[::-1]
        fw = open('./Testfiles/output1.txt','w')
        fw.write(data)
        fw = open('./Testfiles/output1.txt','a')
        ft = open('./Testfiles/temp.txt','r')
        for j in ft:
                fw.write(j)
        ft = open('./Testfiles/temp.txt','w')
        ft = open('./Testfiles/temp.txt','a')
        fw = open('./Testfiles/output1.txt','r+')
        for k in fw:
                ft.write(k)
        ft = open('./Testfiles/temp.txt','r')

fr.close()
fw.close()
ft.close()
print('Done')
print("---Execution time is {}---".format(time.time()-start))
```
The shortcomings of this program are there are too many operations being done with heavy overhead; as we are doing expensive file operations from the loop repeatedly. Also too many loops are used.

> ### Version v5.0

> #### Logic: Using UNIX commands to implement the algorithm.
* Introduction to subprocess module in python.
* Used three files.

```python
import time,subprocess

start_time=time.time()

subprocess.call('cd Desktop/krishagni/Assignments/Ass1\ Final\ version\ Stack/Testfiles',shell=True)
fileread=open('./Testfiles/100west.txt','r')

for f in fileread:
    line=f[-2::-1]
    command="echo {} > output1.txt".format(line)    
    subprocess.call(command,shell=True)
    subprocess.call('cat temp.txt >> output1.txt',shell=True)
    subprocess.call('cat output1.txt > temp.txt',shell=True)
    
print('Done!')
print("---Execution time: {} seconds ---".format(time.time() - start_time))

```
We've used UNIX commands in python through subprocess module. Instead of looping over the files to overwrite and append the data from them we have used the subprocess module to make this happen hence getting rid of the nested for loops.

> ### Version v6.0

> #### Logic: Using UNIX command directly to reverse-read the lines.
* Used *tac* command to reverse read the file.

```python
import time,subprocess

start_time=time.time()

subprocess.call('cd Desktop/krishagni/Assignments/Ass1\ Final\ version\ Stack/Testfiles',shell=True)
subprocess.call('tac 1mb.txt > output.txt',shell=True)
filew=open('1mb.txt','w')
file=open('output.txt','r')

for i in file:
    text=i[::-1]
    filew.write(text)
    
file.close()
filew.close()    

print('Done!')
print("---Execution time: {} seconds ---".format(time.time() - start_time))
```
*tac file.txt > output.txt* Arranges the **lines** of the file in reverse order.
Then by simply applying the logic of line by line reversing We've ultimately reversed the whole file.

> ### Version v7.0

> #### Logic: Reading from file in chunks of data.

> ##### Steps:
> 1. Read from file in chunk.
> 2. Reverse each chunk.
> 3. Make seperate file for chunk.
> 4. Reverse concatenate the files.

```python
import time,subprocess,os

start_time=time.time()

def chunks(file,size=120600):
    while True:
        data=file.read(size)
        if data:
            yield data
        else:
            break

def concat(file_array,i=0):
    size_of_array=len(file_array)
    while i < size_of_array:
        file_list=' '.join(file_array[i:i+10])
        command="cat 0.txt {} > tmp.txt".format(file_list)
        subprocess.call(command,shell=True)
        subprocess.call('mv tmp.txt 0.txt',shell=True)
        i+=10
         
fileread=open('1mbnew.txt','r')

curr_file_name="1.txt"
curr_file_id=1
file_array=[]

for f in chunks(fileread):
    file_array.insert(0,curr_file_name)
    part=f[::-1]
    subprocess.call('echo "{}" > {}'.format(part,curr_file_name),shell=True)
    curr_file_id += 1
    curr_file_name="{}.txt".format(curr_file_id)

concat(file_array)    
    
fileread.close()

print('Done!')
print("---Execution time: %s seconds ---" % round(time.time() - start_time))
```
The only shortcomings for this program are The buffer files; one used to save each chunk, are not deleted automatically.
The concatenation is happenning at the end of the program only.

Instead the concatenation should be happenning on the go. Also the used files should be deleted on the go(After concatenating).

> ### Version v8.0

> #### Logic: Reading from file in chunks of data.
**Added:** 
1. Removing files on the go.
2. _Concat()_ function is called every 10 iteration, i.e; after every 10 files are generated.**

To remove the files on the go we've maintained a **_list_** of file names as files are generated and after every 10 files are generated. We make a call to *concat()* function where we also delete, after concatenating, the files generated using UNIX commands.

```python
import time,subprocess,os

start_time=time.time()

def chunks(file,size=120600):
    while True:
        data=file.read(size)
        if data:
            yield data
        else:
            break

def concat(file_array,i=0):
    size_of_array=len(file_array)
    while i < size_of_array:
        file_list=' '.join(file_array[i:i+10])
        command="cat 0.txt {} > tmp.txt".format(file_list)
        subprocess.call(command,shell=True)
        subprocess.call('mv tmp.txt 0.txt',shell=True)
        i+=10
    del file_array[:]
    subprocess.call('rm -rf {}'.format(file_list),shell=True)
    return
         
fileread=open('500mb.txt','r')

curr_file_name="1.txt"
curr_file_id=1
file_array=[]

for f in chunks(fileread):
    file_array.insert(0,curr_file_name)
    part=f[::-1]
    subprocess.call('echo "{}" > {}'.format(part,curr_file_name),shell=True)
    curr_file_id += 1
    curr_file_name="{}.txt".format(curr_file_id)
    if len(file_array)==10:
        concat(file_array,0)
    
concat(file_array,0)    
    
fileread.close()

print('Done!')
print("---Execution time: {} seconds ---".format(time.time() - start_time))
```
> ### Version v9
> #### Logic: Reading from file in chunks of data.

The bottle-necks of the previous version are removed in this version. 

The ```python subprocess.call('echo "{}" > {}'.format(part,curr_file_name),shell=True)``` command is very expensive to use, instead we are using ```python file.write('The Reversed part!')```.

To reduce the total number of opearations, The size of each chunk is upgraded from 120Kb to 4Mb and the chunk files, which were removed in 10 iterations priorly, are now removed once every 100 iterations.

```python
import time,subprocess,os

start_time=time.time()

def chunks(file,size=4000000):
    while True:
        data=file.read(size)
        if data:
            yield data
        else:
            break

def concat(file_array,i=0):
    size_of_array=len(file_array)
    while i < size_of_array:
        file_list=' '.join(file_array[i:i+100])
        command="cat 0.txt {} > tmp.txt".format(file_list)
        subprocess.call(command,shell=True)
        subprocess.call('mv tmp.txt 0.txt',shell=True)
        i+=100
    del file_array[:]
    subprocess.call('rm -rf {}'.format(file_list),shell=True)
    return
         
fileread=open('1gb.txt','r')

curr_file_name="1.txt"
curr_file_id=1
file_array=[]

for f in chunks(fileread):
    file_array.insert(0,curr_file_name)
    part=f[::-1]
    file=open('{}'.format(curr_file_name),'w')
    file.write(part)
    file.close()
    #subprocess.call('echo "{}" > {}'.format(part,curr_file_name),shell=True)
    curr_file_id += 1
    curr_file_name="{}.txt".format(curr_file_id)
    if len(file_array)==100:
        concat(file_array,0)
    

concat(file_array,0)    
    
fileread.close()

print('Done!')
print("---Execution time: {} seconds ---".format(time.time() - start_time))


```
Changing the three factors helped achieve a prolific growth in speed of execution.
>"80% of the results are affected by 20% of causes."
>                                       - Pareto principle.

> # Execution Time Sheet for all the versions.

Versions|1MB|50MB|500MB|1GB|5GB
--------|---|---|-----|---|---
1|0.0057 sec|1.2108 sec|86.269 sec|System Hangs and Crashes|----
2|13.2834 sec|11.95 min|Not Viable to calculate|Not Viable to calculate|Not Viable to calculate
3|0.0476 sec|0.9631 sec|19.451 sec|----|----
4|3.047 min|>5 min|----|----|----
5|No Output|No Output|No Output|No Output|No Output
6|0.1369 sec|5.7525 sec|55.7831 sec|10.2 min|51.3 min
7|0.1187 sec|22.015 sec|36.58 min|Not Viable to calculate|Not Viable to calculate
8|0.1297 sec|21.24 sec|35.55 min|----|----
9|0.059 sec|0.71368 sec|29.78 sec|95.11 sec|----
