# Lab session #1: 
# Basic "Knowledge Toolbox" to get started in the Cloud
In this Lab session you will be asked to put in practice the basic knowledge required for the Lab sessions of this course.



#  Pre-lab homework 0 
Take a look at the following hands-on guides to check if you already have the basic knowledge to follow this course. If not, please do the assigments. 
* Hands-on 0: [Run a Linux OS in a Virtual Machine](https://github.com/angeltoribio-UPC-BCN/Quick-Start/blob/master/LinuxOS-VirtualMachine.md) (Only for Windows users)
* Hands-on 1: [Git and GitHub Quick Start](https://github.com/angeltoribio-UPC-BCN/Quick-Start/blob/master/Git-Github-Quick-Start.md)
* Hands-on 2: [Markdown syntax](https://github.com/angeltoribio-UPC-BCN/Quick-Start/blob/master/Quick-Start-Markdown.md)
* Hands-on 3: [Python Quick Start](https://github.com/angeltoribio-UPC-BCN/Quick-Start/blob/master/Python-Quick-Start.md) 

#  Pre-lab homework 1
Create an AWS account following the following hands-on:
* Hands-on 5: [Getting Started in the Cloud with AWS](https://github.com/angeltoribio-UPC-BCN/Quick-Start/blob/master/Quick-Start-AWS.md)

#  Tasks of Lab session #1 
## Task 1.1: 
Install Python in your laptop.
## Task 1.2: 
Create a python code that uses the “random” library. We will value positively if you build a creative program in python. The minimum example accepted will be a code that generates a random number between 1 and 20. Then let the player guess the number introduced, displaying if the number is to low or high. The game ends either when the number is guessed correctly. The suggested program name is `Lab1.guessnumber.py`. 
## Task 1.3:  
Create a `private repository` **CLOUD-COMPUTING-CLASS-2018** in your github account. Use your student email account (".upc.edu") to create your github account in order to benefit from private repositories and other perks of the student pack.
## Task 1.4:  
Update your remote repository from the local repository at your laptop:
```
echo "# CLOUD-COMPUTING-CLASS-2018" >> README.md
git init
git add README.md
git add Lab1.guessnumber.py
git commit -m "first commit"
git remote add origin https://github.com/<username>/CLOUD-COMPUTING-CLASS-2018.git
git push -u origin master
```
> change `<username>` for your github account

## Task 1.5:  
Update the `README.md` file including all the information of your group (member's name and email addresses).
## Task 1.6:  
Invite `angeltoribio-UPC-BCN` to your remote private repository as a collaborator using `settings` button (for evaluation purpose).
## Task 1.7:  
Create an EC2 instance at AWS. Login and pull down all the contents of your github repository to make an exact clone by using `git clone` command. 
## Task 1.8:  
Execute the program `Lab1.guessnumber.py` in your AWS instance. Take an screenshot of the xterm that are you using as proof. 
Include that screenshot in your local repository at your laptop with the name `Lab1.AWSterminal.png`.
## Task 1.9:    
Update your remote github repository with the updated `README.md`and the new file `Lab1.AWSterminal.png` using the `git`commands `add`, `commit` and `push`.  

# How to submit this assignment:
Submit **before the deadline** to the *RACO Practicals section* a "Lab1.txt" file including: 

1. group number
2. name and email of the members of this group
3. github url that contains your lab answers
4. indicate if you did or not the optional task and why 
5. add any comment that you consider necessary.

