# Lab 2: Doors in the Cloud


# Tasks to do:


### Task 2.1:  Installing Anaconda
 Download and installing Anaconda in your labtop following hands-on 4 ([Python Development Environment Quick Start](Phyton-Development-Environment-Quick-Start.md)) guidelines. Start a Jupyter notebook on your terminal and create a note book that contains your previous [`Lab1.guessnumber.py`](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab01.md) code including some explanation of your steps using `markdown` cells. Save your notebook as `Lab2.guessnumber.pynb` and add it to your remote github repository.

### Task 2.2: Get Started with NLTK
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
### Task 2.2.1: Word Count
Create a notebook with the name `Lab2.WordCountWithNLTK.ipynb`, that computes and prints the 10 most common word in the book.

### Task 2.2.2: Word Count
Add a new code cell into the same notebook with the code that computes and prints the total number of word of this book.   

ULL! que em dona error encara!


### Task 2.2.3: Remove puntuation
We can see that punctiation are many of the most common words. We can remove the punctuation using the character deletion step of translate method as:

```
    lowers = text.lower()
    no_punctuation = lowers.translate(None, string.punctuation)
    tokens = nltk.word_tokenize(no_punctuation)
```
Add a new code cell to the same notebook with the code (and the comments with markdown cells if you consider interesting)  that computes and prints the 10 most common word without punctuation characters. 
    
### Task 2.2.4: Stop Words
Is not "Tensorflow" the most commond word? Why? What are Stop Words? Include your answer in a markdown cells in the same notebook.

When we are working with text mining applications, we often hear of the term “Stop Word Removal". We can do it using the same `nltk` package: 
```
from nltk.corpus import stopwords
tokens = get_tokens()
filtered = [w for w in tokens if not w in stopwords.words('english')]
count = Counter(filtered)
```
Add a new code cell to the same notebook with the code (and the comments with markdown cells if you consider interesting)  that computes and prints the 10 most common word after removing the stop words.  Now it make more sense, right? "TensorFlow" is the most common word!!!

### Task 2.3: Getting Started with `tweepy`  
[These libraries](https://dev.twitter.com/resources/twitter-libraries), while not necessarily built or tested by Twitter, should support the current Twitter API.
In this task we will use `tweepy` package as a tool to access Twitter data in a fairly easy way with Python. There are different types of data we can collect, however we will focus on the “tweet” object.

### Task 2.3.1: Register Our App on Twitter  
Twitter implements OAuth as its standard authentication mechanism, and in order to have access to Twitter data programmatically, we need to create an app that interacts with the Twitter API. There are four primary identifiers  we will need to note for an OAuth workflow: consumer key, consumer secret, access token, and access token secret.  

The first step is the registration of your app. In particular, you need to point your browser to http://apps.twitter.com, log-in to Twitter and register a new application.

You will receive a **Consumer Key** and a **Consumer secret**.   From the configuration page "Keys and Access Token" of your app, you can also obtain the  **Access Token** and a **Access Token Secret**. 

> **Warning**: these are application settings that should always be kept private.

Note that you will need a Twitter account in order to login, create an app, and get these credentials.

### Task 2.3.2: Accessing your twitter account information  
Twitter provides [REST APIs](https://dev.twitter.com/rest/public) we can use to interact with their service. There is also a bunch of Python-based clients out there as [Tweepy](http://tweepy.readthedocs.io/en/v3.5.0/).
The easiest way to install the latest version is by using pip/easy_install to pull it from [PyPI](https://pypi.python.org/pypi) from your local directory:

```
torres@vm:~$  pip install tweepy
```
You may also use Git to clone the repository from Github and install it manually:


```
torres@vm:~$  git clone https://github.com/tweepy/tweepy.git
torres@vm:~$  cd tweepy
torres@vm:~$  python setup.py install
```
Create a notebook with the name `Lab2.TweepyAPI.ipynb` to keep track of all your work.

In order to authorise our app to access Twitter on our behalf, we need to use the OAuth interface:

```
import tweepy
from tweepy import OAuthHandler
 
consumer_key = 'YOUR-CONSUMER-KEY'
consumer_secret = 'YOUR-CONSUMER-SECRET'
access_token = 'YOUR-ACCESS-TOKEN'
access_secret = 'YOUR-ACCESS-SECRET'

auth = OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_secret)
 
api = tweepy.API(auth)
```
The `api` variable is now our entry point for most of the operations with Twitter. 

Tweepy provides access to the well documented Twitter API. With tweepy, it's possible to get any object and use any method that the official Twitter API offers. For example, a User object has its documentation at https://dev.twitter.com/docs/platform-objects/users and following those guidelines, tweepy can get the appropriate information.

Main Model classes in the Twitter API are: `Tweets`, `Users`, `Entities` and `Places`. 

in order to be sure that everything is correctly installed print the main information of your Twitter account. After create the user object, the `me()` method returns the user whose authentication keys were used:
```
user = api.me()
 
print('Name: ' + user.name)
print('Location: ' + user.location)
print('Friends: ' + str(user.followers_count))
print('Created: ' + str(user.created_at))
print('Description: ' + str(user.description))
```
Are the data printed correct? Are it yours? 

### Task 2.3.2: Accessing Tweets  
Tweepy provides the convenient Cursor interface to iterate through different types of objects. For example, we can read our own Twitter homepage with (we are using 1 to limit the number of tweets we are reading and only reading the `text` of the tweet):

```
for status in tweepy.Cursor(api.home_timeline).items(1):
    print(status.text) 
```
The `status` variable is an instance of the `Status()` class, a nice wrapper to access the data. The JSON response from the Twitter API is available in the attribute _json (with a leading underscore), which is not the raw JSON string, but a dictionary.
```
import json

for status in tweepy.Cursor(api.home_timeline).items(1):
    print(json.dumps(status._json, indent=2))
    
```
What if we want to have a list of 10 of our friends? 
```
for friend in tweepy.Cursor(api.friends).items(1):
    print(json.dumps(friend._json, indent=2))

```
And how about a list of some of our tweets?

```
for tweet in tweepy.Cursor(api.user_timeline).items(1):
    print(json.dumps(tweet._json, indent=2))
```    
As a conclusion, you can notice that with `tweepy` we can easily collect all the information and store them in the original JSON format, fairly easy to convert into different data models (many storage systems provide import feature).

Use the previous API presented for obtaining information about your tweets.  Keep track of your executions and comments in the   `Lab2.TweepyAPI.ipynb` notebook.

### Task 2.X:  
Be sure that you have updated your remote github repository (using the `git`commands `add`, `commit` and `push`) with all the Lab `.ipynb` files generated along this Lab. Submit **before the deadline** to the RACO a "Lab2.txt" file including: 

1. Group number
2. name and email of the members of this group
3. github url that contains your lab answers (the same as Lab1)
4. Include the answer to the previous tasks.
5. add any comment that you consider necessary.


