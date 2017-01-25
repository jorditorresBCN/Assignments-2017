# Lab 2: Doors in the Cloud


# Tasks to do:


### Task 2.1:  Installing Anaconda
 Download and installing Anaconda in your labtop following hands-on 4 ([Python Development Environment Quick Start](Phyton-Development-Environment-Quick-Start.md)) guidelines. Start a Jupyter notebook on your terminal and create a note book that contains your previous [`Lab1.guessnumber.py`](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab01.md) code including some explanation of your steps using `markdown` cells. Save your notebook as `Lab2.guessnumber.pynb` and add it to your remote github repository.

### Task 2.3: Getting Started with NLTK
Tokenisation is one of the most basic, yet most important, steps in text analysis required in the following task. The purpose of tokenisation is to split a stream of text into smaller units called tokens, usually words or phrases. For this purpouse we will use the [NLTK](http://www.nltk.org) Python Natural Language Processing Toolkit:
```
import nltk
```
After importing NLTK, we need download NLTK Data which include a lot of corpora, grammars, models and etc. You can find the complete nltk data list [here](http://nltk.org/nltk_data/). You can downloaded all nltk resources by `nltk.download('all')` but it takes ~3.5G. For english text we could use `nltk.download('punkt')` to download the NLTK data package that includes a pre-trained tokenizer for English.
Let’s see the example using the NLTK to tokenise the book [First Contact with TensorFlow](http://www.jorditorres.org/Tensorflow)  (`FirstContactWithTensorFlow.txt`could be donwloaded from this github) and outputs the 10 most common word in the book.
```
import nltk
nltk.download('punkt') 
import string

from collections import Counter

def get_tokens():
   with open('FirstContactWithTensorFlow.txt', 'r') as tf:
    text = tf.read()
    tokens = nltk.word_tokenize(text)
    return tokens

tokens = get_tokens()
count = Counter(tokens)
print count.most_common(10)
```
### Task 2.3.1: Word Count
Create a notebook with the name `WordCountWithNLTK.ipynb`, that computes and prints the 10 most common word in the book.

### Task 2.3.2: Word Count
Add a new code cell into the same notebook with the code that computes and prints the total number of word of this book.   

ULL! que em dona error encara!


### Task 2.3.3: Remove puntuation
We can see that punctiation are many of the most common words. We can remove the punctuation using the character deletion step of translate method as:

```
    lowers = text.lower()
    no_punctuation = lowers.translate(None, string.punctuation)
    tokens = nltk.word_tokenize(no_punctuation)
```
Add a new code cell to the same notebook with the code (and the comments with markdown cells if you consider interesting)  that computes and prints the 10 most common word without punctuation characters. 
    
### Task 2.3.4: Stop Words
Is not "Tensorflow" the most commond word? Why? What are Stop Words? Include your answer in a markdown cells in the same notebook.

When we are working with text mining applications, we often hear of the term “Stop Word Removal". We can do it using the same `nltk` package: 
```
from nltk.corpus import stopwords
tokens = get_tokens()
filtered = [w for w in tokens if not w in stopwords.words('english')]
count = Counter(filtered)
```
Add a new code cell to the same notebook with the code (and the comments with markdown cells if you consider interesting)  that computes and prints the 10 most common word after removing the stop words.  Now it make more sense, right? "TensorFlow" is the most common word!!!



### Task 1.10:  
Be sure that you have updated your remote github repository (using the `git`commands `add`, `commit` and `push`) with all the Lab files generated along this Lab (e.g. `WordCountWithNLTK.ipynb`). Submit **before the deadline** to the RACO a "Lab2.txt" file including: 

1. Group number
2. name and email of the members of this group
3. github url that contains your lab answers (the same as Lab1)
4. Include the answer to the previous tasks.
5. add any comment that you consider necessary.


