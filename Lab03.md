# Lab3: Analytics in the Cloud

In this Lab we’ll discuss 

# Tasks to do:

## Task 3.1:  Streaming
In case we want to “keep the connection open”, and gather all the upcoming tweets about a particular event, the [Streaming API](https://dev.twitter.com/streaming/overview) is what we need.  The Streaming APIs give developers low latency access to Twitter’s global stream of Tweet data. A proper implementation of a streaming client will be pushed messages indicating Tweets and other events have occurred. 
Connecting to the streaming API requires keeping a persistent HTTP connection open. In many cases this involves thinking about your application differently than if you were interacting with the REST API. Visit the [Streaming API](https://dev.twitter.com/streaming/overview) for more details about the differences between Streaming and REST. 


If your intention is to conduct singular searches, read user profile information, or post Tweets, consider using the REST APIs instead.


without any of the overhead associated with polling a REST endpoint.

We need to extend the StreamListener() to customise the way we process the incoming data. We will base our explanation with a working example that gathers all the new tweets with the "ArtificialIntelligence" content:
```
import tweepy
from tweepy import OAuthHandler

```

### API  streaming --> generar fitxer aquell 

## I el tres es generar el fitxer de tweets JSON (donar un fet) com a programa .py i a partir d'aqui fer el sentiment analysi d'aquest senyor? haig de resoldre tema de codificacio
