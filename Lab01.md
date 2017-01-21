# Lab 01: Basic Toolbox Knowledges to getting started in the Cloud
In this Lab session you will be asked to demonstrate the basic knowledge acquired with homeworks based on these following hands-on:

* Hands-on 0: Run a Linux OS in a Virtual Machine (Only for Windows users)
* Hands-on 1: Git and GitHub Quick Start
* Hands-on 2: Python Quick Start (afegir part de NKTK) 
* Hands-on 3: Create a AWS or OCEAN .... maquina


# Tasks
### Task 1.1: 
Install Python in your local labtop.
### Task 1.2: 
Create a python code that do a word counts of the file `https://ocw.mit.edu/ans7870/6/6.006/s08/lecturenotes/files/t8.shakespeare.txt` (or any book your prefer). The suggested program name is `Lab1.WordCount.py`.
### Task 1.3:  
Create a private repository CLOUD-COMPUTING-COURSE-2017 in your github account (use you student email (".upc.edu") for creating your github account in order to have private repositories or benefits as student pack).
### Task 1.4:  
Update your remote repository from your local repository on your labtop:
```
echo "# CLOUD-COMPUTING-COURSE-2017" >> README.md
git init
git add README.md
git add Lab1.WordCount.py
git commit -m "first commit"
git remote add origin https://github.com/jorditorresBCN/CLOUD-COMPUTING-COURSE-2017.git
git push -u origin master
```
> change `jorditorresBCN` with your github account

### Task 1.5:  
Update the `README.md` file including all the information of your group (member's name and emails).
### Task 1.6:  
Invite `JordiTorresBCN` to your remote private repository as a collaborator using `settings` button (for evaluation purpouse).
### Task 1.7:  
Create a AWS instance EC2. Pull down all the contents of your github repositori making an exact clone using `git clone` command. 
### Task 1.8:  
Execute the program `Lab1.WordCount.py` in your AWS instance. Finally take an screenshot of the xterm that are you using as a proof. 
### Task 1.9:  
Include this screenshot in your local repository on your labtop with the name `Lab1.AWSterminal.png`.
### Task 1.10:  
 Download and installing Anaconda in your labtop. 
### Task 1.11:  
Start an iPython notebook on your terminal and create an create a note book that contains your previous word count code including some explanation of your steps using `markdown` cells. 
### Task 1.12:  
Include this file in your local repository on your labtop with the name `Lab1.WordCount.pynb`.
### Task 1.13:  
Update your remote github repository with the updated `README.md`and the two new files `Lab1.AWSterminal.png` and `Lab1.WordCount.pynb` using the `git`commands `add`, `commit` and `push`. 
### Task 1.14:  
Submit to the RACO a short txt file including **before the deadline**: 

1. Group number
2. name and email of the members of this group
3. github url.
4. add any comment that you consider necessary.

