# Lab 8: Extracting and Analyzing data using Elastic Search and Kibana

**_Last update: 6-June-2017_**

In a previous hands-on we performed a data analysis using `matplotlib`. In this hands-on we are going to perform a data analysis using Kibana and Elastic Search.

* [Task 8.1: Elastic Search](#Tasks31)
* [Task 8.2: Kibana](#Tasks32)  
* [Task 8.3: Longtash and Beats](#Tasks33)  
* [Task 8.4: Search bar](#Tasks34)  
* [Task 8.5: Acknoledgements](#Tasks35)  

   
#  Tasks of Lab 8

<a name="Tasks31"/>

The goal of this lab is collect information posted on Twitter, store it using Elasticsearh and display the information through graphs using Kibana. We will use *Beats* to collect information from Twitter, are open source data shippers that we will install as agents on our servers. Beats can send data directly to Elasticsearch or send it to Elasticsearch via Logstash, which we can use to parse and transform the data. We will build the following workflow:

![SlasticWorkflow](https://github.com/jorditorresBCN/Assignments-2017/blob/master/workflowElasticSearch.png "SlasticWorkflow")





## Task 8.1: Elastic Search

[Elastic Search](https://en.wikipedia.org/wiki/Elasticsearch) is an open source NoSQL distributed and scalable search engine. Elastic Search provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. 

The installation of Elastic Search is quite simple following the documentation [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/zip-targz.html). 

####  Download and install the .zip package
The `.zip` archive for Elasticsearch v5.4.1 can be downloaded and installed as follows:
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.1.zip
unzip elasticsearch-5.4.1.zip
```
#### Change the directory to Elasticsearch folder 
```
cd elasticsearch-5.4.1/ 
```

#### Running Elasticsearch from the command line
```
./bin/elasticsearch
```

#### Checking that Elasticsearch is running
After these simple steps an Elasticsearch instance should be running at [`http://localhost:9200`](http://localhost:9200) in your browser if you run with default configuration. You can send an http request to port 9200 on local host which should give you a response something like this:
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


*Warnings: By default, Elasticsearch runs in the foreground, prints its logs to the standard output (stdout), and can be stopped by pressing `Ctrl-C`. Keep the terminal open where elastic search is running to be able to keep the instance running.You could also use [`nohup`](https://en.wikipedia.org/wiki/Nohup) mode to run the instance in the background. Elasticsearch requires Java 8 or later. Use the official Oracle distribution. You can check the java version of your platform using the command `java -version`*


<a name="Tasks32"/>

## Task 8.2:  Kibana

[Kibana](https://en.wikipedia.org/wiki/Kibana) is an open source data exploration and visualization tool built on Elastic Search to help you understand data better. It provides visualization capabilities on top of the content indexed on an Elasticsearch cluster. Users can create bar, line and scatter plots, or pie charts and maps on top of large volumes of data.


The installation and initialization of Kibana is similar to that of Elasticsearch following the documentation [here](https://www.elastic.co/guide/en/kibana/current/install.html). 
Kibana is provided for Linux and Darwin as a .tar.gz package. I'm show the steps using the Darwin version (choose the correct version according your operation system). 

####  Download and install the Kibana package
The 64-bit Linux archive for Kibana v5.4.1 can be downloaded and installed as follows:  
```
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.4.1-darwin-x86_64.tar.gz
tar -xzf kibana-5.4.1-darwin-x86_64.tar.gz
```
#### Change the directory to Kibana folder 
```
cd kibana-5.4.1-darwin-x86_64
```

#### Running Elasticsearch from the command line
Kibana can be started from the command line as follows:

```
./bin/kibana
```

#### Checking that Kibana is running

Kibana instance should be running at [`http://localhost:5601`](http://localhost:5601) in your browser if you run with default configuration (see[here] (https://www.elastic.co/guide/en/kibana/current/settings.html) for configuring Kibana). You can send an http request to port 5601 on local host which should give you a response something like this:

![KibanaWelcome](https://github.com/jorditorresBCN/Assignments-2017/blob/master/KibanaScreenWelcome.png "KibanaWelcome")


*Warning: Keep the terminal open where Kibana was run to be able to keep the instance running. Create the Kibana folder in the same folder that ElasticSearch in order to use the default values  consideret in this hands-on.*



<a name="Tasks33"/>

## Task 8.3: Logstash and Beats

Logstash provides an input stream to Elastic for storage and search.



https://www.elastic.co/guide/en/beats/libbeat/current/logstash-installation.html


, and Kibana accesses the data for visualizations such as dashboards.[5]


<a name="Tasks34"/>

## Task 8.4: X-Pack

https://www.elastic.co/guide/en/x-pack/current/installing-xpack.html


## Task 8.XXXXX: COSES PENDENTS

* ElasticSearch Head Plugin (https://mobz.github.io/elasticsearch-head/

* Posible error: Total Fields Limit setting (https://discuss.elastic.co/t/total-fields-limit-setting/53004)  

user: elastic

password: changeme


<a name="Tasks35"/>

## Task 8.5:  Acknowledgements

I will thank my former students [Victoria Gabante](https://www.linkedin.com/in/victoria-gabante-guerra-03a679b2) and [Osmar Rodríguez](https://twitter.com/osmar106), who provided insight and expertise in this hands-on with their project at my master course [CC-MEI](http://jorditorres.org/CC-MEI-2017/) at UPC.

I would also like to show my gratitude to [Dani Cea](https://www.linkedin.com/in/danicea/) for his help on an [earlier version of this hands-on](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab.cc.mei.eskibana.2015.pdf) 2 years ago. 


# How to Submit this Assignment:  
Submit **before the deadline** to the *RACO Practicals section* a "LabX.txt" following the guidelines indicated in the last theory class. 
