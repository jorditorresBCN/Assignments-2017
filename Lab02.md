# Lab 2: Doors in the Cloud


# Tasks to do:


### Task 2.1:  Installing Anaconda
 Download and installing Anaconda in your labtop following hands-on 4 ([Python Development Environment Quick Start](Phyton-Development-Environment-Quick-Start.md)) guidelines. Start a Jupyter notebook on your terminal and create a note book that contains your previous [`Lab1.guessnumber.py`](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab01.md) code including some explanation of your steps using `markdown` cells. Save your notebook as `Lab2.guessnumber.pynb` and add it to your remote github repository.

### Task 2.3: Getting Started with NLTK
Tokenisation is one of the most basic, yet most important, steps in text analysis required in the following task. The purpose of tokenisation is to split a stream of text into smaller units called tokens, usually words or phrases. For this purpouse we will use the [NLTK](http://www.nltk.org) Python Natural Language Processing Toolkit. 

The notebook `WordCountWithNLTK.ipynb` contains some steps required for a word count of the book [First Contact with TensorFlow](http://www.jorditorres.org/Tensorflow). You will find both, code and text elements, that describe the main steps. Run the notebook document step-by-step (one cell a time) by pressing `shift` + `enter` that will help to you understand the [NLTK](http://www.nltk.org) package. 
### Task 2.3.1: Word Count
Write the code that computes the total number of word of this book.
```
TotalWords=0
for paraula, counter in count.most_common(100):
    print 'paraula: %s, count: %s' % (paraula, counter)
    TotalWords=TotalWords+counter
print 'Total number of word in the book = %s' % (TotalWords)
```
### Task 2.3.2: Word Count
We can see that punctiation are many of the most common words. We can remove the punctuation using the character deletion step of translate:

```
    lowers = text.lower()
    no_punctuation = lowers.translate(None, string.punctuation)
    tokens = nltk.word_tokenize(no_punctuation)
```
Which are the 10 most common word (without punctuation characters)?

    lowers = text.lower()
    no_punctuation = lowers.translate(None, string.punctuation)
    tokens = nltk.word_tokenize(no_punctuation)
    
    
We can see that punctiation are many of the most common words. We can remove the punctuation using the character deletion step of translate

### Task 1.14: (optional)
What are Stop Words? When working with text mining applications, we often hear of the term “Stop Word Removal". Create a new version of your program that remove the stop words in our example file with the name `Lab1.WordCountStopWordsRemoved.pynb` and update your remote github repository with this new file


### Task 1.10:  
Be sure that you have updated your remote github repository (using the `git`commands `add`, `commit` and `push`) with all the Lab files generated along this Lab. Submit **before the deadline** to the RACO a "Lab2.txt" file including: 

1. Group number
2. name and email of the members of this group
3. github url that contains your lab answers (the same as Lab1)
4. Include the answer to the previous tasks.
5. add any comment that you consider necessary.


