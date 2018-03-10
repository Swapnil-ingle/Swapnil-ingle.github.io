# Intuition for Git : for starters.

The main motivation for me to write this blog is how comlpex and convoluted the git tutorials are online. If you went right now and searched for "git tutorials linux" sure you will get hundreds of site leading you to the enlightment. Sure you will get the syntaxes that are used to operate over git. But the motivation behind git and its each command, atleast basic one, is hardly discussed or is discussed in a way too complicated way that is more than often a bouncer for the guy who's a newbie.

Here I'll discuss the philosophy of git from my point of view (a relatively newbie) and explain the cardinal commands without wandering deep into much of the complexity and technicalities of the commands or rather we will be discussing the intuitions(*feel*) of using git.

This will be enough for you to get kick-started with using git and wander into the depths into your own.

## What is git(hub)?

Software were being developed before git right? 

So what was the motivation behind git? what made people so fixated towards using git to develop their software? Yeah let's talk about that.

git is a _**"version control system" that is not centralized**_ and to understand that, and the context, imagine that you are senior software devoloper or senior software architect. You have a team of 50 developers working for you and you just had this revolutionary idea for a software, you wrote the basic code structure over night *phew*. All the Interfaces, Classes, Packages are ready for further implementation of business logic now you just need all the people you have, working on different modules of this software but these "different modules" need to be tested and integrated together right? 

So you go to office the other day all charged up and motivated but now you have the following questions before you:

1. Distribution.

How are you gonna distribute your very own code structure to all the developers?

Pendrive? (Make them pass further?) What if some new guy joins? What if the pendrive is lost? and so many obvious reason as why this is not the most efficient.

Upload it on a cloud server and send link to all and make the team download it to thier own system?

Seems plausible right!?

Any new guy can download this upfront from the _**upstream server**_. 50 or 500 people can download it simultaneously. Everyone can see what's going on **upstream**. Your whole project folder is called **repository** and as this is on cloud we can call it *_upstream repository_*.

This, my friends, is your _*master*_ branch and the upstream server is _*github*_ only stable, working version of your project will always be on your _*master*_ branch and when the team download this into their computer **locally** this is the  *_local repository_*.

To download a new *repository* the developers use the git *clone* command.

> git clone *Link of the Repository*

This downloads/clones the *Upstream Repoository* locally. A new folder is created into the current path of terminal this new folder will contain all the online data along with *.git* file that contains all the *sync* inforamtion.

So the first problem is sovled using git.

2.
3.
4.

## Installing.......

Insatlling git on linux is just about easy as anything.
