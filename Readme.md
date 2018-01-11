# Assignment One.

> ## Subject - Take an input file and reverse its content.

This seemingly menial task took me about about a week to complete, *intrigued why!?*

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

Another approach has been experimented on in this particular version. I mean *if we want to reverse the file as whole why not read it from backwards?*.

> ####Logic: Reading from backend.

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


