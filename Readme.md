# Assignment One.

> ## Subject - Take an input file and reverse its content.

This seemingly menial task took me about about a week to complete, *intrigued why!?*

I'll be sharing the journey of my attempts to try solve this and the twist and turns my code took during the 9 versions.

Concluded by an excel sheet comprising of various time required to reverse various file sizes by all versions of the code.

```python
'''
v1: Logic:Reading straight from file.
*Reading directly whole files as a string.
*Writing directly to file.
Shortcomings:
*Memory inefficient.
*Can't load huge files into memory at once.
'''

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
