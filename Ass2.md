---
title: Assignment 2
---
<!DOCtype = html>
<html>  

<link type="text/css" rel="stylesheet" href="/stylesheets/main.css" />

<body>

<nav order=inline>
 <ul list-style-type: none; margin: 0; padding: 0;>
  <li><a href="https://swapnil-ingle.github.io">Home</a></li>
  <li><a href="https://swapnil-ingle.github.io">Assignment 1</a></li>
  <li><a href="https://swapnil-ingle.github.io/Ass2">Assignment 2</a></li>
  <li><a href="about.asp">About</a></li>
</ul>
</nav>  

# Assignment 2

> ## Subject - Take an input file and find the number of occurences of individual words.

Say this is a String from the input file. "Apple is a healthy food but Soda is a Junk food." The expected output would be 

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
1. Take a dictionary like data type(Hashmap).
2. Read file char by char until a *Space* is found.
3. Add newly found word into the data-type with value as *previous+1*.
4. Repeat!

This would be a generic approach germane to any programming language given. But Talking about python specifically we can make some modification to the above approach. So my approach to the problem was,
1. Take a dictionary.
2. Read the file.
3. Use **text.split(' ')** to split the string at spaces and save it into a list.
4. **for** Travese the list while adding the current element to dictionary with value as *previous+1*.
5. Repeat!

The problems with this approach are : 
> 1. Can't Read big file as a whole into memory. 
> 2. No thought given to the storage of the "words occurences".
> 3. Given enough input file size the dictionary will run out of space giving **_MemoryError()_**.
> 4. Can't print out large size of outputs.

> ### Approach 2.0

Trivial errors with Approach 1.0 are removed in this one. 

This Approach was all about,
> 1. Reading from file in chunks.
> 2. Writing the dictionary into a file.

It was obvious to read from the file in chunks also I decided to write the output into a file as printing the *whole words and their occurence count* was not a viable option.  

However the following problems still persisted,

I still needed to do something about the **MemoryError()** of dictionary as the limited size of the dictionary was a liability and some alternative was needed.

> ### Logic: Generic

</body>
</html>
