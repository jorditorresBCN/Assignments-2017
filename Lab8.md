# Lab 8: Extracting and Analyzing data using Elastic Search and Kibana

**_Last update: 6-June-2017_**

In a previous hands-on we performed a data analysis using `matplotlib`. In this hands-on we are going to perform a data analysis using Kibana and Elastic Search.

* [Task 8.1: Elastic Search](#Tasks31)
* [Task 8.2: Kibana](#Tasks32)  
* [Task 8.3: Creating dashboards](#Tasks33)  
* [Task 8.4: Search bar](#Tasks34)  
* [Task 8.5: Acknoledgements](#Tasks35)  

   
#  Tasks of Lab 8

<a name="Tasks31"/>

## Task 8.1: Elastic Search

[Elastic Search](https://www.elastic.co) is an open source NoSQL distributed and scalable search engine. Elastic Search provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. 

The installation of Elastic Search is quite simple and it is as follows:
1. Download and unzip Elasticsearch from https://www.elastic.co/start 
2. Change the directory to Elasticsearch folder `cd elasticsearch-5.4.0` (commands according last visit on 01/June/2017)
3. Run `bin/elasticsearch` (or bin\elasticsearch.bat on Windows)

After these simple steps an Elasticsearch instance should be running at [`http://localhost:9200`](http://localhost:9200) in your browser if you run with default configuration. You will receive: 
```
{
  "name" : "zTy5Ohn",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "MT8F5SiVTJmmkXh8lMjzag",
  "version" : {
    "number" : "5.4.0",
    "build_hash" : "780f8c4",
    "build_date" : "2017-04-28T17:43:27.229Z",
    "build_snapshot" : false,
    "lucene_version" : "6.5.0"
  },
  "tagline" : "You Know, for Search"
}
```


*Important: Keep the terminal open where elastic search is running to be able to keep the instance running.You could also use [`nohup`](https://en.wikipedia.org/wiki/Nohup) mode to run the instance in the background.*


<a name="Tasks32"/>

## Task 8.2:  Kibana

[Kibana](https://en.wikipedia.org/wiki/Kibana) is an open source data exploration and visualization tool built on Elastic Search to help you understand data better. It provides visualization capabilities on top of the content indexed on an Elasticsearch cluster. Users can create bar, line and scatter plots, or pie charts and maps on top of large volumes of data.


The installation and initialization of Kibana is similar to that of Elasticsearch:

1. Download and unzip Kibana (in the same folder you donwloaded ElasticSearch) from https://www.elastic.co/start
3. Change the directory to Kibana folder `cd kibana-5.4.0-darwin-x86_64`
4. Run bin/Kibana (or bin\Kibana.bat on Windows)

Kibana instance should be running at [`http://localhost:5601`](http://localhost:5601) in your browser if you run with default configuration.

![KibanaWelcome](https://github.com/jorditorresBCN/Assignments-2017/blob/master/KibanaScreenWelcome.png "KibanaWelcome")


*Important: Keep the terminal open where Kibana was run to be able to keep the instance running. *



XXXXX


XXXXX


XXXXX


XXXXX

XXXXX



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


<a name="Tasks33"/>

## Task 3.3:  Case study

At "[Racó (course intranet)](https://raco.fib.upc.edu/home/portada/jordi.torres)" you can find a small dataset as a example (please do not distribute due to Twitter licensing). This dataset contains 1060 tweets downloaded from around 18:05 to 18:15  on January 13. We used "Barcelona" as a `track` parameter at `twitter_stream.filter` function. 

We would like to get a rough idea of what was telling people about Barcelona. For example, we can count and sort the most commonly used hastags:
```
import operator 
import json
from collections import Counter
 
fname = 'Lab3.CaseStudy.json'
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

![Lab3Plot](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab3Plot.png "lab3Plot")

We can see that people were talking about football, more than other things! And it seems that they were mostly talking about the football [league match that was played the next day](http://www.fcbarcelonanoticias.com/Calendario-y-resultados-liga.php?IDR=184).

Create a "matplot" with your dataset generated by you in the previous task.

<a name="Tasks34"/>

## Task 3.4:  Student proposal

We are asking to the student to create an example that will allow us to find some interesting insight from Twitter, using some realistic data taken by the student. Using what we have learnt in the previous Labs and sections, you can download some data using the streaming API, pre-process the data in JSON format and extract some interesting terms and hashtags from the tweets. 

Create a `.pynb` file with markdown cells describing the program steps and the characteristics of the dataset created (e.g. the time frame for the download, etc.).

<a name="Tasks35"/>

## Task X.5:  Acknoledgements
This hands-on was based on a [post at Analytics Vidhya](https://www.analyticsvidhya.com/blog/2017/05/beginners-guide-to-data-exploration-using-elastic-search-and-kibana) by Supreeth Manyam ([@ziron](https://datahack.analyticsvidhya.com/user/profile/ziron). Also the project of [Victoria Gabante](https://www.linkedin.com/in/victoria-gabante-guerra-03a679b2) and [Osmar Rodríguez](https://twitter.com/osmar106) in our master course [CC-MEI](http://jorditorres.org/CC-MEI-2017/) at UPC contributed in this hands-on.


# How to Submit this Assignment:  
Submit **before the deadline** to the *RACO Practicals section* a "LabX.txt" following the guidelines indicated in the last theory class. 
