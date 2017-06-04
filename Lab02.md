# Lab 2: Doors in the Cloud
In this Lab session we will discuss the overall structure of a tweet and we'll discuss how to pre-process the text before we can get into some more interesting analysis in the next Lab session. In particular, we will see how tokenisation, despite being a well-understood problem, can get tricky dealing with Twitter data. Prior to this, we will install A Python Development Environment which will be very helpful for this and future sessions.

* [Pre-lab howemork 2](#Prelab)
   * [HW 2.1: Installing Anaconda](#HW1)
   * [HW 2.2: Registering Our App on Twitter](#HW2)
* [Tasks for Lab session #2](#Tasks)
   * [Task 2.1: Geting Started with NLTK](#NLTK)
   * [Task 2.2: Getting Started with `tweepy`](#tweepy)
   * [Task 2.3: Tweet pre-processing](#preproc)

<a name="Prelab"/>

#  Pre-lab homework 2

<a name="HW1"/>

## HW 2.1: Installing Anaconda
Download and install Anaconda in your laptop following the hands-on guidelines at ([Python Development Environment Quick Start](https://github.com/jorditorresBCN/Quick-Start/blob/master/Python-Development-Environment-Quick-Start.md)). Start a Jupyter notebook on your terminal and create a note book, using `markdown` cells, that contains your previous [`Lab1.guessnumber.py`](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab01.md) code including some explanation of the steps you've followed. Save your notebook as `Lab2.guessnumber.ipynb` and add it to your remote GitHub repository.

<a name="HW2"/> 

## HW 2.2: Registering Our App on Twitter
Cloud applications are characterized by an increased focus on user participation and content creation, but also by a deep interaction and interconnection of applications sharing content from different types of services in order to integrate multiple systems together. This scenario is, undoubtlessly, possible thanks to the rise of “Application Programming Interfaces” (API). 

An API, or Application Programming Interface, provides a way for computer systems to interact with each other. There are many types of APIs. Every programming language has a built-in API that it is used to write programs. For instance, you have studied in previous courses that operating systems themselves have APIs used by programs to interact with files or present text on the screen. 
Since this course is centered in studying Cloud technologies, we are going to focus on APIs that are built on top of web technologies such as HTTP. We will refer to this type of API as `web API`: an interface to be used by either web servers or web browsers. Such APIs are extensively used for the development of web applications and work at either the server end as well as at the client end. 
Web APIs are a key component in nowadays Cloud era. Many cloud applications provide an API that allows developers to integrate their own code to intereact with them, taking advantage of the services' functionality for their own apps.

One example among the vast number of available APIs is Twitter API. Twitter API offers the possibility of accessing all tweets made by any user, all tweets containing a particular term or combination of terms, all tweets about a given topic during a date range, etc.


In order to set up our Lab session #2 to be able to access Twitter data, there are some preliminary steps. Twitter implements OAuth (called Open Authorization) as its standard authentication mechanism, and in order to have access to Twitter data programmatically, we'll need to create an app that interacts with the Twitter API. There are four primary identifiers  we will need to note for an OAuth workflow: consumer key, consumer secret, access token, and access token secret. Good news from developer's perspective are that the Python ecosystem has already well-established libraries for most social media platforms, which come with an implementation of the authentication process.

The first step in this homework is registering your app on Twitter. In particular, you need to point your browser to http://apps.twitter.com, log-in to Twitter and register a new application. You will receive a **Consumer Key** and a **Consumer secret**. From the configuration page "Keys and Access Token" of your app, you can also obtain the **Access Token** and a **Access Token Secret**. Save this information to perform the following Lab session.

> **Warning**: these are application settings that should always be kept private.

Note that you will need a Twitter account in order to login, create an app, and get these credentials.

<a name="Tasks"/>

#  Tasks for Lab session #2

<a name="NLTK"/>

## Task 2.1: Geting Started with NLTK
One of the most popular packages in Python for NLP (Natural Language Processing) is Natural Language Toolkit ([NLTK](http://www.nltk.org). This toolkit provides a friendly interface for many of the common NLP tasks, as well as lexical resources and linguistic data.

Tokenisation is one of the most basic, yet most important, steps in text analysis required for the following task. The purpose of tokenisation is to split a stream of text into smaller units called tokens, usually words or phrases. For this purpouse we will use the [NLTK](http://www.nltk.org) Python Natural Language Processing Toolkit:
```
import nltk
```
A difference between NLTK and many other packages is that this framework also comes with linguistic data for specific tasks. Given their big size, such data is not included in the default installation, but can be downloaded separately. For this reason, after importing NLTK, we'll need to download NLTK Data which includes a lot of corpora, grammars, models and etc. You can find the complete nltk data list [here](http://nltk.org/nltk_data/). You can download all nltk resources using `nltk.download('all')` but it takes ~3.5G. For English text we could use `nltk.download('punkt')` to download the NLTK data package that includes a pre-trained tokenizer for English.

Let’s see the example using the NLTK to tokenise the book [First Contact with TensorFlow](http://www.jorditorres.org/Tensorflow)  (`FirstContactWithTensorFlow.txt`could be downloaded from this github) and outputs the 10 most common words in the book.
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
### Task 2.1.1: Word Count 1
Create a notebook named `Lab2.WordCountWithNLTK.ipynb`, that computes and prints the 10 most common words in the book.

### Task 2.1.2: Word Count 2
Add a new code cell into the same notebook with the code that computes and prints the total number of words of that book.   


### Task 2.1.3: Remove puntuation
We can remove the punctuation by using the character deletion step of the translate method:

```
    lowers = text.lower()
    no_punctuation = lowers.translate(None, string.punctuation)
    tokens = nltk.word_tokenize(no_punctuation)
```
Add a new code cell to the same notebook with the code (and the comments with markdown cells if you consider it interesting) that computes and prints the 10 most common words without punctuation characters. 
    
### Task 2.1.4: Stop Words
Is it not "Tensorflow" the most commond word? Why? Which are the Stop Words? Include your answer in a markdown cells in the same notebook.

When we work with text mining applications, we often hear of the term “Stop Word Removal". We can do it using the same `nltk` package: 
```
from nltk.corpus import stopwords
tokens = get_tokens()
filtered = [w for w in tokens if not w in stopwords.words('english')]
count = Counter(filtered)
```
Add a new code cell to the same notebook holding the code (and the comments with markdown cells if you consider interesting)  that computes and prints the 10 most common word after removing the stop words.  Now it makes more sense, right? "TensorFlow" is the most common word!

<a name="tweepy"/>

## Task 2.2: Getting Started with `tweepy`

In this task we will use the `tweepy` package as a tool to access Twitter data in a fairly easy way using Python. There are different types of data that we can collect, however we will focus on the “tweet” object.

### Task 2.2.1: The Twitter API
As [homework](#HW1) we already registered Our App on Twitter in order to set up our project and access Twitter data. However, the Twitter API limits access to applications. You can find more details at [the official documentation](https://dev.twitter.com/rest/public/rate-limiting). It's also important to consider that [different APIs have different rate limits](https://dev.twitter.com/rest/public/rate-limiting). The implications of hitting the API limits is that Twitter will return an error message rather than the data we are asking for. Moreover, if we continue performing more requests to the API, the time required to obtain regular access again will increase as Twitter could flag us as potential abusers. If our application needs many API requests we can use the `time` module (`time.sleep()` function). 

Another important thing to know, before we begin, is that we have two classes of APIs: **REST APIs** and **Streaming API**. All REST APIs only allow you to go back in time (tweets that have already been published). Often these APIs limit the amount of tweets you can retrieve, not just in terms of rate limits as we mentioned, but also in terms of time span. In fact, it's usually possible to go back in time up to approximately one week. Also another aspect to consider regarding the REST API is that it does not guarantee to provide all tweets published on Twitter. 

On the other hand, the Streaming API looks into the future: we can retrieve all tweets matching our filtering criteria as they are published. The Streaming API is useful when we want to filter a particular keyword and download a massive amount of tweets about it. On the other hand REST API is useful when we want to search for tweets authored by a specific user or we want to access our own timeline.


### Task 2.2.2: Accessing your twitter account information

In order to interact with the Twitter APIs, we need a Python client that implements the different calls to the API itself. There are several options as we can see at the [official documentation](https://dev.twitter.com/resources/twitter-libraries). We will choose for this lab [Tweepy](http://tweepy.readthedocs.io/en/v3.5.0/).
One easy way to install the latest version is by using pip/easy_install to pull it from [PyPI](https://pypi.python.org/pypi) to your local directory:

```
torres@vm:~$  pip install tweepy
```
You may also want to use Git to clone the repository from Github and install it manually:


```
user@vm:~$  git clone https://github.com/tweepy/tweepy.git
user@vm:~$  cd tweepy
user@vm:~$  python setup.py install
```
Create a notebook named `Lab2.TweepyAPI.ipynb` to keep track of all your work.

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

Tweepy provides access to the well documented Twitter API. With tweepy, it's possible to get any object and use any method that the official Twitter API offers. For example, a `User` object has its documentation at https://dev.twitter.com/docs/platform-objects/users and following those guidelines, tweepy can get the appropriate information.

Main Model classes in the Twitter API are: `Tweets`, `Users`, `Entities` and `Places`. 

In order to be sure that everything is correctly installed print the main information of your Twitter account. After creating the `User` object, the `me()` method returns the user whose authentication keys were used:
```
user = api.me()
 
print('Name: ' + user.name)
print('Location: ' + user.location)
print('Friends: ' + str(user.followers_count))
print('Created: ' + str(user.created_at))
print('Description: ' + str(user.description))
```
Is the data printed correctly? Is it yours? 

### Task 2.2.3: Accessing Tweets
Tweepy provides the convenient `Cursor` interface to iterate through different types of objects. For example, we can read our own Twitter home timeline using the code below.

```
# we use 1 to limit the number of tweets we are reading 
# and we only access the `text` of the tweet
for status in tweepy.Cursor(api.home_timeline).items(1):
    print(status.text) 
```
The `status` variable is an instance of the `Status()` class, a nice wrapper to access the tweet data. The JSON response from the Twitter API is available at the attribute _json (with a leading underscore), which is not the raw JSON string, but a dictionary.
```
import json

for status in tweepy.Cursor(api.home_timeline).items(1):
    print(json.dumps(status._json, indent=2))
    
```
What if we wanted to have a list of 10 of our friends? 
```
for friend in tweepy.Cursor(api.friends).items(1):
    print(json.dumps(friend._json, indent=2))

```
And how about a list of some of our tweets?

```
for tweet in tweepy.Cursor(api.user_timeline).items(1):
    print(json.dumps(tweet._json, indent=2))
```    
As a conclusion, notice that using `tweepy` we can easily collect all the tweet information and store it in JSON format, fairly easy to convert into different data models (many storage systems provide import feature).

Use the previous API presented to obtain information about your tweets.  Keep track of your executions and comments using the   `Lab2.TweepyAPI.ipynb` notebook.

<a name="preproc"/>

## Task 2.3:  Tweet pre-processing
We will now enter into more detail regarding the overall structure of a tweet and discuss how to pre-process its text before we can go into more interesting analysis in the next Lab session. In particular, we will see how tokenisation, despite being a well-understood problem, can get tricky with Twitter data. After that we’ll discuss the analysis of term frequencies to extract meaningful terms from our tweets. 

The code used in this Lab session is using part of the work done by [Marco Bonzanini](https://marcobonzanini.com/2015/03/02/mining-twitter-data-with-python-part-1/)). As Marco indicates, it is far from perfect but it’s a good starting point to become aware of the complexity of the problem, and fairly easy to extend.

Let’s have a look at the structure of the previous tweet that you printed.
The main attributes are:

* text: text of the tweet itself
* created_at:  creation date
* favorite_count, retweet_count: the number of favourites and retweets
* favorited, retweeted: boolean stating whether the authenticated user (you) have favourited or retweeted this tweet
* lang: the language of the tweet (e.g. “en” for english)
* id: the tweet identifier
* place, coordinates, geo: geo-location information if available
* user: the author’s full profile
* entities: list of entities such as URLs, @-mentions, hashtags and symbols
* in_reply_to_user_id: user identifier if the tweet is a reply to a specific user
* in_reply_to_status_id: status identifier id the tweet is a reply to a specific status
* \_json: This is a dictionary with the JSON response of the status
* author: The tweet author

As you can see there is a lot of information we can play with. All the \*_id fields also have a \*_id_str counterpart, where the same information is stored as a string rather than a big int (to avoid overflow problems). 

We will focus on looking for the text of a tweet and breaking it down into words. While tokenisation is a well understood problem with several out-of-the-box solutions from popular libraries, Twitter data pose some challenges because of the nature of the language used.
Let’s see an example using the NLTK package previously used to tokenise a fictitious tweet:


```
from nltk.tokenize import word_tokenize

tweet = 'RT @JordiTorresBCN: just an example! :D http://JordiTorres.Barcelona #masterMEI'

print(word_tokenize(tweet))
```
You will notice some peculiarities of twitter that are not captured by a general-purpose English tokeniser such as the one from NLTK: @-mentions, emoticons, URLs and #hash-tags are not recognised as single tokens. Right?

Using some code borrowed from [Marco Bonzanini](https://marcobonzanini.com/2015/03/02/mining-twitter-data-with-python-part-1/) we could consider these aspects of the language (A former student of this course, [Cédric Bhihe](https://www.linkedin.com/in/cedricbhihe/), suggested [this alternative code](https://github.com/jorditorresBCN/Assignments-2017/blob/master/CedricTokenizer.py)).  

```python
import re
 
emoticons_str = r"""
    (?:
        [:=;] # Eyes
        [oO\-]? # Nose (optional)
        [D\)\]\(\]/\\OpP] # Mouth
    )"""
 
regex_str = [
    emoticons_str,
    r'<[^>]+>', # HTML tags
    r'(?:@[\w_]+)', # @-mentions
    r"(?:\#+[\w_]+[\w\'_\-]*[\w_]+)", # hash-tags
    r'http[s]?://(?:[a-z]|[0-9]|[$-_@.&+]|[!*\(\),]|(?:%[0-9a-f][0-9a-f]))+', # URLs
 
    r'(?:(?:\d+,?)+(?:\.?\d+)?)', # numbers
    r"(?:[a-z][a-z'\-_]+[a-z])", # words with - and '
    r'(?:[\w_]+)', # other words
    r'(?:\S)' # anything else
]
    
tokens_re = re.compile(r'('+'|'.join(regex_str)+')', re.VERBOSE | re.IGNORECASE)
emoticon_re = re.compile(r'^'+emoticons_str+'$', re.VERBOSE | re.IGNORECASE)
 
def tokenize(s):
    return tokens_re.findall(s)
 
def preprocess(s, lowercase=False):
    tokens = tokenize(s)
    if lowercase:
        tokens = [token if emoticon_re.search(token) else token.lower() for token in tokens]
    return tokens
 
tweet = 'RT @JordiTorresBCN: just an example! :D http://JordiTorres.Barcelona #masterMEI'
print(preprocess(tweet))
```

As you can see, @-mentions, URLs and #hash-tags are now preserved as individual tokens. This tokenizer gives you a general idea of how you can tonkenize twitter text using regular expressions (regexp), which is a common choice for this type of problem. 

With the previous basic tokenizer code, some particular types of tokens will not be captured, and will be probably broken into several other tokens. To overcome this problem you can improve the regular expressions, or employ more sophisticated techniques such as [*Named Entity Recognition*](https://en.wikipedia.org/wiki/Named-entity_recognition).

In this example, the regular expressions are compiled with the flags re.VERBOSE, to allow spaces in the regexp to be ignored (see the multi-line emoticons regexp), and re.IGNORECASE to match both upper and lowercase text. The tokenize() function simply catches all the tokens in a string and returns them as a list. This function is used within preprocess(), which is used as a pre-processing chain: in this case we simply add a lowercasing feature for all the tokens that are not emoticons (e.g. :D doesn’t become :d).

Keep track of your executions with different fictitious tweets and comments in the `Lab2.TokenizeTweetText.ipynb` notebook.

Now, we are ready for next Lab session where we will be mining streaming twitter data.


# How to Submit this Assignment:
Be sure that you have updated your remote github repository (using the `git`commands `add`, `commit` and `push`) with all the Lab `.ipynb` files generated along this Lab. Submit **before the deadline** to the *RACO Practicals section* a "Lab2.txt" file including: 

1. group number
2. name and email of the members of the group
3. github url that contains your lab answers (the same as Lab1)
5. add any comment that you consider necessary


