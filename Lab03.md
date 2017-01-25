# Lab3: Analytics in the Cloud

In this Lab we’ll discuss 

# Tasks to do:

## Task 3.1:  Streaming API of Twitter
In case we want to “keep the connection open”, and gather all the upcoming tweets about a particular event, the [Streaming API](https://dev.twitter.com/streaming/overview) is what we need.  The Streaming APIs give developers low latency access to Twitter’s global stream of Tweet data. A proper implementation of a streaming client will be pushed messages indicating Tweets and other events have occurred. 
Connecting to the streaming API requires keeping a persistent HTTP connection open. In many cases this involves thinking about your application differently than if you were interacting with the REST API. Visit the [Streaming API](https://dev.twitter.com/streaming/overview) for more details about the differences between Streaming and REST. 


If your intention is to conduct singular searches, read user profile information, or post Tweets, consider using the REST APIs instead.


without any of the overhead associated with polling a REST endpoint.

We need to extend the StreamListener() to customise the way we process the incoming data. We will base our explanation with a working example (from [Marco Bonzanini](https://marcobonzanini.com/2015/03/02/mining-twitter-data-with-python-part-1/) ) that gathers all the new tweets with the "ArtificialIntelligence" content:
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

The above script will save each tweet on a new line, so you can use the command `wc -l python.json` from a Unix shell to know how many tweets you’ve gathered. Warning, depending on the search term, we can gather tons of tweets within a few minutes.

## Task 3.1:  Counting terms
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
Remmeber that `preprocess` have been defined in the previous Lab in order to capture Twitter-specific aspects of the text, such as #hashtags, @-mentions and URLs.:

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
    # Print the first 5 most frequent words
    print(count_all.most_common(5))
    
    ```
As you can see, the above code produces words (or tokens) that are stop words. Given the nature of our data and our tokenisation, we should also be careful with all the punctuation marks and with terms like `RT` (used for re-tweets) and `via` (used to mention the original author), which are not in the default stop-word list.

```

from nltk.corpus import stopwords
nltk.download("stopwords") # download the stopword corpus on our computer
import string
 
punctuation = list(string.punctuation)
stop = stopwords.words('english') + punctuation + ['rt', 'via', 'RT']

```

We can now substitute the variable terms_all in the first example with something like:

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
        terms_stop = [term for term in preprocess(tweet['text']) if term not in stop] # Update the counter
        count_all.update(terms_stop)
    for word, index in count_all.most_common(5):
        print '%s : %s' % (word, index)
```
The above code print the first 5 most frequent words.


 
### API  streaming --> generar fitxer aquell 

## I el tres es generar el fitxer de tweets JSON (donar un fet) com a programa .py i a partir d'aqui fer el sentiment analysi d'aquest senyor? haig de resoldre tema de codificacio
