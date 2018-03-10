### [Assignment One](https://swapnil-ingle.github.io)  ---     [Assignment Two](https://swapnil-ingle.github.io/Ass2) --- [Concepts List](https://swapnil-ingle.github.io/Concepts) --- [What is Biobank](https://swapnil-ingle.github.io/what_is_biobank) --- [Git Intuition](https://swapnil-ingle.github.io/git_for_starters)

# Intuition for Git : for starters.

The main motivation for me to write this blog is how comlpex and convoluted the git tutorials are online. If you went right now and searched for "git tutorials linux" sure you will get hundreds of site leading you to the enlightment. Sure you will get the syntaxes that are used to operate over git. But the motivation behind git and its each command, atleast basic one, is hardly discussed or is discussed in a way too complicated way that is more than often a bouncer for the guy who's a newbie.

Here I'll discuss the philosophy of git from my point of view (a relatively newbie) and explain the cardinal commands without wandering deep into much of the complexity and technicalities of the commands or rather we will be discussing the intuitions(*feel*) of using git.

This will be enough for you to get kick-started with using git and wander into the depths into your own.

## Installing.......

Installing git on __*linux*__ is just about easy as anything:

> sudo apt-get git

## Create an account:

Create an account on www.github.com

## What is git(hub)?

Software were being developed before git right? 

So what was the motivation behind git? what made people so fixated towards using git to develop their software? Yeah let's talk about that.

git is a _**"version control system" that is not centralized**_ and to understand that, and the context, imagine that you are senior software devoloper or senior software architect. You have a team of 50 developers working for you and you just had this revolutionary idea for a software, you wrote the basic code structure over night *phew*. All the Interfaces, Classes, Packages are ready for further implementation of business logic now you just need all the people you have, working on different modules of this software but these "different modules" need to be tested and integrated together right? 

So you go to office the other day all charged up and motivated but now you have the following questions before you:

### 1. Distribution.

How are you gonna distribute your very own code structure to all the developers?

Pendrive? (Make them pass further?) What if some new guy joins? What if the pendrive is lost? and so many obvious reason as why this is not the most efficient.

Upload it on a cloud server and send link to all and make the team download it to thier own system?

Seems plausible right!?

Any new guy can download this upfront from the _**upstream server**_. 50 or 500 people can download it simultaneously. Everyone can see what's going on **upstream**. Your whole project folder is called **repository** and as this is on cloud we can call it **_upstream repository_**.

This, my friends, is your _**master**_ branch and the upstream server is _**github**_ only stable, working version of your project will always be on your _**master**_ branch and when the team download this into their computer **locally** this is the  **_local repository_**.

To download a new *repository* the developers use the git *clone* command.

> git clone ***Link of the Repository***

Make sure you __*pull*__ from the repository(Branch_name) to get the latest changes to your local system.

> git pull

This will pull the changes to your current branch on your local repository.

This downloads/clones the *Upstream Repoository* locally. A new folder is created into the current path of terminal this new folder will contain all the online data along with *.git* file that contains all the *sync* inforamtion.

So the first problem is sovled using git.

### 2. Simutaneous editing

So now everybody has a local copy of the your project. Now each one of the developers need to work upon each module and somehow integrate it with everyone elses's module so every modification byu each dev should be combined by all moification of all dev and the final product should be stored upon the *master branch* as discussed earlier.

Here the branching concept of git can be used.

  1. Everyone makes a branch from master branch.
  2. User branch name that is relevant to your job. For ex: one dev doing data_validation can make a branch from master named "DEVNAME_DATA_VALIDATION"

Here's how the developer's screen will look like in this phase:

> git branch

This will show all the branches locally and the current(active) branch with be in green color with a \* pointer.

So the dev needs to make a new branch, the command for the same is:

> git branch -b \*branch_Name\*

### Notes:

> This will only create a new branch on the local repository not _**upstream**_. Basically _**pushing**_, excuse the pun, anything *upside* has "three steps process" we'll discuss it later.

Basically what making a branch does is it copies all the data from msater to your branch (kinda like your own seperate sub-folder where you can edit without any hesitance or worries about polluting the original code). So all the changes you do will be stored in your own branch/sub-folder as long as your current branch is *your_branch* while *pushing the changes*.
(NOTE: This subfolder is virtual and won't be present actually on your system but once __*pushed*__ would show as a sepearate branch upon UI in your folder on github.com)

### 3. Done with changes locally.

Now most of the devs are done with the changes locally and now they want to upload their changes to their upstream respective branch which would be later merge with the master once reviewed by you (Senior Software Architect). 

Now here comes the showdown which is really the crux of the interaction between local and upstream repo.

The _**pushing**_ your changes _**upstream**_ comprise of three steps process:

  1. add - Adds to staging area.
  2. commit - commits the changes locally.
  3. push - finally pushes the changes to upstream branch.
  
Before going for this trio make sure to check your branch *status* we can use git status command for this. If any of the developer has modififed 5 files and created a new file *locally* the git status will give output as:

> git status

> Changes not staged for commit:

>                 *File1* path
>                 *File2* path
>                 *File3* path
>                 *File4* path
>                 *File5* path
>                 *NewFile* path

See these are the changes that are _**not staged**_ for commit so to add these to staging area.

> git add file_path_1 file_path_2 file_path_3 file_path_4 file_path_5 file_path_6 

After this the 'git status' command will show that all the files are now green this means that the files are successfully added to staging area.

Now it's time to **commit** (the commit will save the changes for the last push) along with your message the command for the same is:

> git commit -m "Any relevant message that will reflect your changes."

This will go to your commit logs from where you can keep track of the changes done to your application.

> git log

### 4. After this you'll need to push your changes along with the branch.

> git push origin *branch_Name*

Now the dev has successfully pushed his local repository to his branch. Now you (_**Senior Software Architect**_ \*wink\* \*wink\*) need to review this. After which this branch can be successfully _**merged**_ with _**master**_ branch.

#### **6 Hours Later**

Congrats you've reviewed all the changes from all the developers now you tell each of them to merge their changes into the _**master**_ branch. This will update the __*master*__ branch in accordance with the *reviewed* branch.

To merge __*your branch*__ with the __*master*__ branch 
  * Go to __*master*__ branch.(*Command: git checkout master*)
    * Gets your pointer to master branch.  
    
The command for the same is:

> git merge *branch_NAME*

This will merge the changes from *your_branch* to master branch.

So there you have it properly managing 50 people team on your project using git.

Some additional important commands for using git are:

1. Operating into existing branch (Checkout some existing branch).

> git checkout *existing_branch_NAME*

2. Delete a branch:

  * From local repository: git branch -D *branch_NAME*
  * From upstream repository: git push origin --delete *branch_NAME*
  
#### That's all Folks!  

Hope you got an intuition and a basic understanding to get started with git and github. 
