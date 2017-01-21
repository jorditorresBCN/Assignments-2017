# Lab 1: Basic "Knowledge Toolbox" to getting started in the Cloud
In this Lab session you will be asked to put in practice the basic knowledge acquired with homeworks based on these following hands-on:

* Hands-on 0: Run a Linux OS in a Virtual Machine (Only for Windows users)
* Hands-on 1: Git and GitHub Quick Start
* Hands-on 2: Python Quick Start (afegir part de NKTK) 
* Hands-on 3: Create a AWS or OCEAN .... maquina

#  ull! treure d'aquí el tema de wordcount i posar només el petit exemple de GuestNumber 
# tot ho de word count, i seguir el .pynd que tinc fer-ho ja en el seguent Lab2, que 
# que faria minims de Twitter API i tambe de ANACONDA (i aixo suficient per 2 hores)

# I el tres es generar el fitxer de tweets JSON (donar un fet) com a programa .py i a partir d'aqui fer el sentiment analysi d'aquest senyor? haig de resoldre tema de codificacio


# Tasks to do:
### Task 1.1: 
Install Python in your local labtop.
### Task 1.2: 
Create a python code that do a word counts of the book First Contact with Tensorflow (posar link) downloaded in the file `FirstContactWithTensorflow.txt` (or any book your prefer). The suggested program name is `Lab1.WordCount.py`.
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
### Task 1.14: (optional)
What are Stop Words? When working with text mining applications, we often hear of the term “Stop Word Removal". Create a new version of your program that remove the stop words in our example file with the name `Lab1.WordCountStopWordsRemoved.pynb` and update your remote github repository with this new file

### Task 1.15:  
Submit **before the deadline** to the RACO a short ".txt" file including: 

1. Group number
2. name and email of the members of this group
3. github url that contains your lab answers
4. indicate if you did or not the optional task and why 
5. add any comment that you consider necessary.

