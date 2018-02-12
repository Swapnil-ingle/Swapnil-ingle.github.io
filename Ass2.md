---
title: Assignment 2
---
### [Assignment One](https://swapnil-ingle.github.io)  ---     [Assignment Two](https://swapnil-ingle.github.io/Ass2) --- [Concepts List](https://swapnil-ingle.github.io/Concepts) --- [What is Biobank](https://swapnil-ingle.github.io/what_is_biobank)

# Assignment 2

> ## Subject - Take an input file and find the number of occurrences of individual words.

Say this is a String from the input file. **"Apple is a healthy food but Soda is a Junk food."** The expected output would be 

> * Apple : 1
> * is : 2
> * a : 2
> * healthy : 1
> * food : 2
> * but : 1
> * Soda : 1
> * Junk : 1

Listed below is the summary of the approaches and overview of my thinking process as I headed towards solving this problem. All the programs are implemented in *python(3)* programming language.

> ### Approach 1.0

The First obvious approach would be to 
1. Take a dictionary-like data type(Hashmap).
2. Read file char by char until a *Space* is found.
3. Add newly found word into the data-type with value as *previous+1*.
4. Repeat!

This would be a generic approach germane to any programming language given. But Talking about python specifically we can make some modification to the above approach. So my approach to the problem was,
1. Take a dictionary.
2. Read the file.
3. Use **text.split(' ')** to split the string into spaces and save it into a list.
4. **for** Traverse the list while adding the current element to dictionary with value as *previous+1*.
5. Repeat!

The problems with this approach are : 
> 1. Can't Read big file as a whole into memory. 
> 2. No thought was given to the storage of the "words occurrences".
> 3. Given enough input file size the dictionary will run out of space giving **_MemoryError()_**.
> 4. Can't print out large size of outputs.

> ### Approach 2.0

Trivial errors with Approach 1.0 are removed in this one. 

This Approach was all about,
> 1. Reading from a file in chunks.
> 2. Writing the dictionary into a file.

It was obvious to read from the file in chunks also I decided to write the output into a file as printing out the *whole words and their occurrence count* was not a viable option.  

However the following problems still persisted,

I still needed to do something about the **MemoryError()** of the dictionary as the limited size of the dictionary was a liability and some alternative was needed.

> ### Approach 3.0

To avoid the limited size of dictionary what I could do is to use the dictionary as a temporary storage and update the output, wherever it was stored, accordingly. Doing this in a file was not practical, as I'd have to check for each word in the file everytime it occurs in the dictionary and then update the value into a file and overwriting into a .txt file would also become an issue.

So instead of using a normal text file as output, we could use a .csv file or more perfectly we could use a database to write the output to. I've used sqlite3 module in python to do all the database operations.

This was finally a decent approach that I decided to implement in code. 

> ## Version 1.0

```python
import sqlite3,time

start=time.time()

conn=sqlite3.connect('test1.db')
c=conn.cursor()

def makechunk(filer,size=25000000):
    while True:
        data=filer.read(size).lower()
        if data:
            yield data
        else:
            break

def Show_entry(table_name):
    for i in c.execute("SELECT * FROM {}".format(table_name)):
        print(i)

def Create_table(table_name):
    c.execute('CREATE TABLE IF NOT EXISTS {}(Name TEXT UNIQUE, value INT)'.format(table_name))
    conn.commit()
    print('Table Created!')

def Drop_table(table_name):
    c.execute("DROP TABLE {}".format(table_name))
    conn.commit()
    print('Table Dropped!')

def Update_db(table_name):
    param1=list(frequency.items())
    print("Updating...")
    c.executemany("UPDATE Dict SET value=value+? WHERE Name=?",param1)
    if c.rowcount == 0:
        c.executemany("INSERT INTO Dict (value,name) VALUES (?,?)",param1)   
    conn.commit()
    del param1[:]
    
frequency = {}
table_name='Dict'
Drop_table(table_name)
Create_table(table_name)
file_read = open('500mb.txt', 'r')

for d in makechunk(file_read):
    match=d.split(' ')
    for word in match:
        count = frequency.get(word,0)
        frequency[word] = count + 1
    Update_db('Dict')
    frequency.clear()
    
conn.commit()
#Show_entry('Dict')
    
file_read.close()
c.close()
conn.close()
print("***** Execution time : {} sec ******".format(time.time()-start))
```
> #### Execution Flow:

1. Read a chunk from file.
2. Split the chunk using **chunk.split(' ')** and saving it into a list.
3. For loop on the size of list
    * Update Dictionary for each item.
4. Make a list of tuple of the dictionary to pass to **_Executemany()_**(Prepared Statement).  
    * Update Database.
5. Make the dictionary empty and ready for a new chunk.
6. Repeat!

We have used Prepared Statements to efficiently execute multiple queries at a time. 
