# Lab 2: Doors in the Cloud
In this Lab we will discuss the overall structure of a tweet and discuss how to pre-process the text before we can get into some more interesting analysis in the next Lab. In particular, we will see how tokenisation, despite being a well-understood problem, can get tricky with Twitter data. Prior to this, we will  install A Python Development Environment which will be very helpful.

The last poart of this lab is build to discuss a basis for extracting interesting terms from a data set of tweets while we “keep the connection open” and gather all the upcoming tweets about a particular event.

* [Pre-lab howemork 2](#Prelab)
   * [HW 2.1: Installing Anaconda](#HW1)
   * [HW 2.2: Register Our App on Twitter](#HW2)  
* [Tasks Lab 2](#Tasks)
   * [Task 2.1: Get Started with NLTK](#NLTK)
   * [Task 2.2: Getting Started with `tweepy`](#tweepy)  
   * [Task 2.3:  Tweet pre-processing](#preproc)
   * [Task 2.4: Streaming API of Twitter](#Tasks24)
    * [Task 2.5: Analyzing tweets - counting terms](#Tasks25)  
    * [Task 2.6: Case study](#Tasks26)  
    * [Task 2.7: Student proposal](#Tasks27)    


<a name="Prelab"/>

#  Pre-lab homework 2

<a name="HW1"/>

## HW 2.1: Installing Anaconda
Download and installing Anaconda in your laptop following hands-on 4 ([Python Development Environment Quick Start](https://github.com/jorditorresBCN/Quick-Start/blob/master/Python-Development-Environment-Quick-Start.md)) guidelines. Start a Jupyter notebook on your terminal and create a note book that contains your previous [`Lab1.guessnumber.py`](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab01.md) code including some explanation of your steps using `markdown` cells. Save your notebook as `Lab2.guessnumber.ipynb` and add it to your remote GitHub repository.

<a name="HW2"/> 

## HW 2.2: Register Our App on Twitter  
Cloud applications are characterized by an increased focus on user participation and content creation, but also by a deep interaction and interconnection of applications sharing con-tent from different types of services in order to integrate multiple systems together. This scenario is, doubtlessly, possible thanks to the rise of  the “Application Programming Interfaces” (API). 

An API, or Application Programming Interface, provide a way for computer systems to interact with each other. There are many types of APIs. Every programming language has a built-in API that it used to write programs. For instance, you studied in previous courses that operating systems themselves have APIs used by programs to open files or draw text on the screen. 
Due this course is centered in the Cloud, we are going to focus on API that are built with web technologies as HTTP. We will refer to this type of API as web API, an interface to either a web server or a web browser. These APIs are used extensively for the development of web applications and work at either the server end or the client end. 
Web APIs are a key component into today Cloud era. Many cloud applications provide an API that allows developers to integrate their own code with these applications, taking ad-vantage of the services' functionality in their own apps.

One example amount the vast number of available one is Twitter API. Twitter API allows to access all tweets made by any user, the tweets containing a particular term or even a combination of terms, tweets done on the topic in a particular date range, etc.


In order to set up our Lab 2 to access Twitter data, there are some preliminary steps. Twitter implements OAuth (called Open Authorization) as its standard authentication mechanism, and in order to have access to Twitter data programmatically, we need to create an app that interacts with the Twitter API. There are four primary identifiers  we will need to note for an OAuth workflow: consumer key, consumer secret, access token, and access token secret. A good new from developer's perspective is that the Python ecosystem has already wellestablished libraries for most social media platforms, which come with an implementation of the authentication process.

The first step in this homework is the registration of your app. In particular, you need to point your browser to http://apps.twitter.com, log-in to Twitter and register a new application. You will receive a **Consumer Key** and a **Consumer secret**. From the configuration page "Keys and Access Token" of your app, you can also obtain the **Access Token** and a **Access Token Secret**. Save this information to perform the following Lab session.

> **Warning**: these are application settings that should always be kept private.

Note that you will need a Twitter account in order to login, create an app, and get these credentials.

<a name="Tasks"/>

#  Tasks of Lab 2

<a name="NLTK"/>

## Task 2.1: Get Started with NLTK
One of the most popular packages in Python for NLP is Natural Language Toolkit ([NLTK](http://www.nltk.org). The toolkit provides a friendly interface for many of the common NLP tasks, as well as lexical resources and linguistic data.

Tokenisation is one of the most basic, yet most important, steps in text analysis required in the following task. The purpose of tokenisation is to split a stream of text into smaller units called tokens, usually words or phrases. For this purpouse we will use the [NLTK](http://www.nltk.org) Python Natural Language Processing Toolkit:
```
import nltk
```
A difference between NLTK and many other packages is that this framework also comes with linguistic data for specific tasks. Given their size, such data is not included in the default installation, but has to be downloaded separately. For this reason, after importing NLTK, we need download NLTK Data which include a lot of corpora, grammars, models and etc. You can find the complete nltk data list [here](http://nltk.org/nltk_data/). You can downloaded all nltk resources by `nltk.download('all')` but it takes ~3.5G. For english text we could use `nltk.download('punkt')` to download the NLTK data package that includes a pre-trained tokenizer for English.

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
Create a notebook with the name `Lab2.WordCountWithNLTK.ipynb`, that computes and prints the 10 most common words in the book.

### Task 2.1.2: Word Count 2
Add a new code cell into the same notebook with the code that computes and prints the total number of word of this book.   


### Task 2.1.3: Remove puntuation
We can see that punctiation are many of the most common words. We can remove the punctuation using the character deletion step of translate method as:

```
    lowers = text.lower()
    no_punctuation = lowers.translate(None, string.punctuation)
    tokens = nltk.word_tokenize(no_punctuation)
```
Add a new code cell to the same notebook with the code (and the comments with markdown cells if you consider interesting) that computes and prints the 10 most common words without punctuation characters. 
    
### Task 2.1.4: Stop Words
Is not "Tensorflow" the most commond word? Why? What are Stop Words? Include your answer in a markdown cells in the same notebook.

When we are working with text mining applications, we often hear of the term “Stop Word Removal". We can do it using the same `nltk` package: 
```
from nltk.corpus import stopwords
tokens = get_tokens()
filtered = [w for w in tokens if not w in stopwords.words('english')]
count = Counter(filtered)
```
Add a new code cell to the same notebook with the code (and the comments with markdown cells if you consider interesting)  that computes and prints the 10 most common word after removing the stop words.  Now it make more sense, right? "TensorFlow" is the most common word!

<a name="tweepy"/>

## Task 2.2: Getting Started with `tweepy`  

In this task we will use `tweepy` package as a tool to access Twitter data in a fairly easy way with Python. There are different types of data we can collect, however we will focus on the “tweet” object.

### Task 2.2.1: The Twitter API
As a [homework](#HW1) we already register Our App on Twitter in order to set up our project to access Twitter data. However, the Twitter API limits access to applications. You can find more detail in [the official documentation](https://dev.twitter.com/rest/public/rate-limiting). It's also important to consider that [different APIs have different rate limits](https://dev.twitter.com/rest/public/rate-limiting). The implications of hitting the API limits is that Twitter will return an error message rather than the data we are asking for. Moreover, if we continue performing more requests to the API, the time required to obtain regular access again will increase as Twitter could flag us as potential abusers. If our application needs many API requests we can use the `time` module (`time.sleep()` function). 

Another important thing before to start is to know that we have two classes of API: **REST APIs** and **Streaming API**. All the REST APIs only allow you to go back in time (tweets that have already been published). Often these APIs limit the amount of tweets you can retrieve, not just in terms of rate limits as we mentioned, but also in terms of time span. In fact, it's usually possible to go back in time up to approximately one week. Also another aspect to consider about the REST API is that they are not guaranteed to provide all the tweets published on Twitter. 

On the other hand, the Streaming API looks into the future, we can retrieve all the tweets that match our filter criteria, as they are published. The Streaming API is useful when we want to filter a particular keyword and download a massive amount of tweets about it, While the REST APIs are useful when we want to search for tweets authored by a specific user or we want to access our own timeline.


### Task 2.2.2: Accessing your twitter account information  

In order to interact with the Twitter APIs, we need a Python client that implements the different calls to the API itself. There are several options as we can see from the [official documentation](https://dev.twitter.com/resources/twitter-libraries). We will chose for this lab [Tweepy](http://tweepy.readthedocs.io/en/v3.5.0/).
The easiest way to install the latest version is by using pip/easy_install to pull it from [PyPI](https://pypi.python.org/pypi) to your local directory:

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
Is the data printed correctly? Is it yours? 

### Task 2.2.3: Accessing Tweets  
Tweepy provides the convenient Cursor interface to iterate through different types of objects. For example, we can read our own Twitter home timeline with (we are using 1 to limit the number of tweets we are reading and only reading the `text` of the tweet):

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

<a name="preproc"/>

## Task 2.3:  Tweet pre-processing
In these tasks we’ll enter in more detail to the overall structure of a tweet and discuss how to pre-process its text before we can get into some more interesting analysis in the next Lab. In particular, we will seen how tokenisation, despite being a well-understood problem, can get tricky with Twitter data. After that we’ll discuss the analysis of term frequencies to extract meaningful terms from our tweets. 

The code used in this Lab is using part of the work done by [Marco Bonzanini](https://marcobonzanini.com/2015/03/02/mining-twitter-data-with-python-part-1/)). As Marco indicates, it is far from perfect but it’s a good starting point to become aware of the complexity of the problem, and fairly easy to extend.

Let’s have a look at the structure of the previous tweet that you printed.
The main attributes are the following:

* text: the text of the tweet itself
* created_at: the date of creation
* favorite_count, retweet_count: the number of favourites and retweets
* favorited, retweeted: boolean stating whether the authenticated user (you) have favourited or retweeted this tweet
* lang: acronym for the language (e.g. “en” for english)
* id: the tweet identifier
* place, coordinates, geo: geo-location information if available
* user: the author’s full profile
* entities: list of entities like URLs, @-mentions, hashtags and symbols
* in_reply_to_user_id: user identifier if the tweet is a reply to a specific user
* in_reply_to_status_id: status identifier id the tweet is a reply to a specific status
* \_json: This is a dictionary with the JSON response of the status
* author: The tweet author

As you can see there is a lot of information we can play with. All the \*_id fields also have a \*_id_str counterpart, where the same information is stored as a string rather than a big int (to avoid overflow problems). 

We will focus our task looking for the text of a tweet, breaking it down into words. While tokenisation is a well understood problem with several out-of-the-box solutions from popular libraries, Twitter data pose some challenges because of the nature of the language.
Let’s see the example using the NLTK package previously used to tokenise a fictitious tweet:


```
from nltk.tokenize import word_tokenize

tweet = 'RT @JordiTorresBCN: just an example! :D http://JordiTorres.Barcelona #masterMEI'

print(word_tokenize(tweet))
```
You will notice some peculiarities of twitter that are not captured by a general-purpose English tokeniser like the one from NLTK: @-mentions, emoticons, URLs and #hash-tags are not recognised as single tokens. Right?

Using some code borrowed from [Marco Bonzanini](https://marcobonzanini.com/2015/03/02/mining-twitter-data-with-python-part-1/) we could consider these aspects of the language (A former student, [Cédric Bhihe](https://www.linkedin.com/in/cedricbhihe/), suggested [this alternative code](https://github.com/jorditorresBCN/Assignments-2017/blob/master/CedricTokenizer.py)).  

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

As you can see, @-mentions, URLs and #hash-tags are now preserved as individual tokens. This tokeniser gives you the general idea of how you can do it for twitter text based on regular expressions (regexp), which is a common choice for this type of problem. 

With the previous basic tokenizer code, some particular types of tokens will not be captured, and will be probably broken into several tokens. To overcome this problem you can improve the regular expressions, or employ more sophisticated techniques like [*Named Entity Recognition*](https://en.wikipedia.org/wiki/Named-entity_recognition).

In this example, the regular expressions are compiled with the flags re.VERBOSE, to allow spaces in the regexp to be ignored (see the multi-line emoticons regexp), and re.IGNORECASE to catch both upper and lowercases. The tokenize() function simply catches all the tokens in a string and returns them as a list. This function is used within preprocess(), which is used as a pre-processing chain: in this case we simply add a lowercasing feature for all the tokens that are not emoticons (e.g. :D doesn’t become :d).

Keep track of your executions with different fictitious tweets and comments in the `Lab2.TokenizeTweetText.ipynb` notebook.

Now, we are ready for next Lab, where we will mining streaming twitter data.

<a name="Tasks24"/>

## Task 2.4: Streaming API of Twitter
In case we want to “keep the connection open”, and gather all the upcoming tweets about a particular event, the [Streaming API](https://dev.twitter.com/streaming/overview) is what we need.  The Streaming APIs give developers low latency access to Twitter’s global stream of Tweet data. A proper implementation of a streaming client will be pushed messages indicating Tweets and other events have occurred. 
Connecting to the streaming API requires keeping a persistent HTTP connection open. In many cases this involves thinking about your application differently than if you were interacting with the REST API. Visit the [Streaming API](https://dev.twitter.com/streaming/overview) for more details about the differences between Streaming and REST. The Streaming API is one of the favorite ways of getting a massive amount of data without exceeding the rate limits. If your intention is to conduct singular searches, read user profile information, or post Tweets, consider using the REST APIs instead.

We need to extend the `StreamListener()` class to customise the way we process the incoming data. We will base our explanation with a working example (from [Marco Bonzanini](https://marcobonzanini.com/2015/03/02/mining-twitter-data-with-python-part-1/)) that gathers all the new tweets with the "ArtificialIntelligence" content:
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
```
from tweepy import Stream
from tweepy.streaming import StreamListener
 
class MyListener(StreamListener):
 
    def on_data(self, data):
        try:
            with open('ArtificialIntelligenceTweets.json', 'a') as f:
                f.write(data)
                return True
        except BaseException as e:
            print("Error on_data: %s" % str(e))
        return True
 
    def on_error(self, status):
        print(status)
        return True
 
twitter_stream = Stream(auth, MyListener())
twitter_stream.filter(track=['ArtificialIntelligence'])
```

The core of the streaming logic is implemented in the `CustomListener` class, which extends `StreamListener` and overrides two methods: `on_data()` and `on_error()`. These are handlers that are triggered when data is coming through and an error is given by the API. if the error is that we have been rate limited by the Twitter API, we need to wait before we can use the service again. The `on_data()` method is called when data is coming through. This function simply stores the data as it is received in the  `ArtificialIntelligenceTweets.json` file. Each line of this file will then contain a single tweet, in the JSON format. You can use the command `wc -l ArtificialIntelligenceTweets.json` from a Unix shell to know how many tweets you’ve gathered.

Before continuing the hands-on, be sure that you generated correctly the `.json` file. now try with another term of your interest.

<a name="Tasks25"/>

## Task 2.5:  Analizing tweets - Counting terms
The first exploratory analysis that we can perform is a simple word count. In this way, we can observe what are the terms most commonly used in the data set.

Let's go to read the file with all tweets in order to be sure that everything is fine:

```
import json  
with open('ArtificialIntelligenceTweets.json','r') as json_file:
         for line in json_file:
             tweet = json.loads(line)
             print tweet["text"]
```
Now we are ready to start to tokenize all these tweets:
         

```
import json
 
with open('ArtificialIntelligenceTweets.json', 'r') as f:
    line = f.readline() 
    tweet = json.loads(line) 
    print(json.dumps(tweet, indent=4)) 
```
Now, if we want to process all our tweets, previously saved on file:

```
with open('ArtificialIntelligenceTweets.json', 'r') as f:
#import io
#f=io.open('data/stream_barcelona.json', 'r', encoding='utf8' )
     for line in f:
        tweet = json.loads(line)
        tokens = preprocess(tweet['text'])
        print(tokens)
```
Remember that `preprocess` have been defined in the previous Lab in order to capture Twitter-specific aspects of the text, such as #hashtags, @-mentions and URLs.:

```
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
 ```
In order to keep track of the frequencies while we are processing the tweets, we can use `collections.Counter()` which internally is a dictionary (term: count) with some useful methods like `most_common()`:
 
 ```
import operator 
import json
from collections import Counter
 
fname = 'ArtificialIntelligenceTweets.json'
with open(fname, 'r') as f:
    count_all = Counter()
    for line in f:
        tweet = json.loads(line)
        # Create a list with all the terms
        terms_all = [term for term in preprocess(tweet['text'])]
        # Update the counter
        count_all.update(terms_all)
    print(count_all.most_common(5))
    
```
As you can see, the above code produces words (or tokens) that are stop words. Given the nature of our data and our tokenisation, we should also be careful with all the punctuation marks and with terms like `RT` (used for re-tweets) and `via` (used to mention the original author), which are not in the default stop-word list.

```
import nltk
from nltk.corpus import stopwords
nltk.download("stopwords") # download the stopword corpus on our computer
import string
 
punctuation = list(string.punctuation)
stop = stopwords.words('english') + punctuation + ['rt', 'via', 'RT']

```

We can now substitute the variable `terms_all` in the first example with something like:

```
import operator 
import json
from collections import Counter
 
fname = 'ArtificialIntelligenceTweets.json'
with open(fname, 'r') as f:
    count_all = Counter()
    for line in f:
        tweet = json.loads(line)
        # Create a list with all the terms
        terms_stop = [term for term in preprocess(tweet['text']) if term not in stop]
        count_all.update(terms_stop)
    for word, index in count_all.most_common(5):
        print '%s : %s' % (word, index)
```

Besides stop-word removal, we can further customise the list of terms/tokens we are interested in. 
For instance, if we want to count *hastags* only:
```
terms_hash = [term for term in preprocess(tweet['text']) 
              if term.startswith('#')]
```
In the case we are interested to count terms only, no hashtags and no mentions:
```
terms_only = [term for term in preprocess(tweet['text']) 
              if term not in stop and
              not term.startswith(('#', '@'))] 
```
> Mind the double brackets (( )) `startswith()` takes a tuple (not a list) if  we pass a list of inputs. 

Although we do not consider it in this Lab, there are other functions from NLTK very useful. For instance, to put things in context, some analysis considers sequences of two terms. In this case we can use `bigrams()` function that will take a list of tokens and produce a list of tuples using adjacent tokens.

Do the same analysis with the `.json` file generated by you in the previous task.


<a name="Tasks26"/>

## Task 2.6:  Case study

At "[Racó (course intranet)](https://raco.fib.upc.edu/home/portada/jordi.torres)" you can find a small dataset as a example (please do not distribute due to Twitter licensing). This dataset contains 1060 tweets downloaded from around 18:05 to 18:15  on January 13. We used "Barcelona" as a `track` parameter at `twitter_stream.filter` function. 

We would like to get a rough idea of what was telling people about Barcelona. For example, we can count and sort the most commonly used hastags:
```
import operator 
import json
from collections import Counter
 
fname = 'Lab2.CaseStudy.json'
with open(fname, 'r') as f:
    count_all = Counter()
    for line in f:
        tweet = json.loads(line)
        # Create a list with all the terms
        terms_hash = [term for term in preprocess(tweet['text']) if term.startswith('#') and term not in stop]        
        count_all.update(terms_hash)
# Print the first 10 most frequent words
print(count_all.most_common(15))

```
The output is:
`[(u'#Barcelona', 68), (u'#Messi', 30), (u'#FCBLive', 17), (u'#UDLasPalmas', 13), (u'#VamosUD', 13), (u'#barcelona', 10), (u'#CopaDelRey', 8), (u'#empleo', 6), (u'#BCN', 6), (u'#riesgoimpago', 6), (u'#news', 5), (u'#LaLiga', 5), (u'#SportsCenter', 4), (u'#LionelMessi', 4), (u'#Informe', 4)]`

In order to see a more visual description we can plot it. There are different options to create plots in Python using libraries like matplotlib, ggplot, etc. We decided to use matplotlib With the following code 

```
%matplotlib inline
import matplotlib as mpl
mpl.rcParams['figure.figsize'] = (15,10)
import matplotlib.pyplot as plt

sorted_x, sorted_y = zip(*count_all.most_common(15))
#print(sorted_x, sorted_y)

plt.bar(range(len(sorted_x)), sorted_y, width=0.75, align='center');
plt.xticks(range(len(sorted_x)), sorted_x, rotation=80);
plt.axis('tight'); 

```
that uses the function `zip()`. We obtain the following plot:

![Lab2Plot](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab2Plot.png "lab2Plot")

We can see that people were talking about football, more than other things! And it seems that they were mostly talking about the football [league match that was played the next day](http://www.fcbarcelonanoticias.com/Calendario-y-resultados-liga.php?IDR=184).

Create a "matplot" with your dataset generated by you in the previous task.

<a name="Tasks27"/>

## Task 2.7:  Student proposal

We are asking to the student to create an example that will allow us to find some interesting insight from Twitter, using some realistic data taken by the student. Using what we have learnt in the previous Labs and sections, you can download some data using the streaming API, pre-process the data in JSON format and extract some interesting terms and hashtags from the tweets. 

Create a `.pynb` file with markdown cells describing the program steps and the characteristics of the dataset created (e.g. the time frame for the download, etc.).


# How to Submit this Assignment:  
Be sure that you have updated your remote github repository (using the `git`commands `add`, `commit` and `push`) with all the Lab `.ipynb` files generated along this Lab. Submit **before the deadline** to the *RACO Practicals section* a "Lab2.txt" file including: 

1. Group number
2. name and email of the members of this group
3. github url that contains your lab answers (the same as Lab1)
4. link to your dataset created in task 2.7.
5. add any comment that you consider necessary.


