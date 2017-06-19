# Lab 8: Extracting and Analyzing data using Elastic Search and Kibana

**_Last update: 19-June-2017_**

In a previous hands-on we performed a data analysis using `matplotlib`. In this hands-on we are going to perform a data analysis using Kibana and Elastic Search.

* [Task 8.1: Elasticsearch](#Tasks31)
* [Task 8.2: Kibana](#Tasks32)  
* [Task 8.3: Longtash and Beats](#Tasks33)  
* [Task 8.4: Using our Twitter dataset](#Tasks34) 
* [Task 8.5: Elastic Cloud](#Tasks34) 
* [Task 8.6: Student Project](#Tasks36)  
* [Task 8.7: Acknoledgements](#Tasks37)  

   
#  Tasks of Lab 8

<a name="Tasks31"/>

The goal of this lab is to fetch twitter data using logstash and then put it into elasticsearch. Finally We want to do visualisation using kibana. We will use *Beats* to collect information from Twitter, are open source data shippers that we will install as agents on our servers. Beats can send data directly to Elasticsearch or send it to Elasticsearch via Logstash, which we can use to parse and transform the data. We will build the following workflow:

![SlasticWorkflow](https://github.com/jorditorresBCN/Assignments-2017/blob/master/workflowElasticSearch.png "SlasticWorkflow")





## Task 8.1: Intall elasticsearch

[Elasticsearch](https://en.wikipedia.org/wiki/Elasticsearch) is an open source NoSQL distributed and scalable search engine. Elastic Search provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. 

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

## Task 8.2:  Install kibana

[Kibana](https://en.wikipedia.org/wiki/Kibana) is an open source data exploration and visualization tool built on Elastic Search to help you understand data better. It provides visualization capabilities on top of the content indexed on an Elasticsearch cluster. Users can create bar, line and scatter plots, or pie charts and maps on top of large volumes of data.


The installation and initialization of Kibana is similar to that of Elasticsearch following the documentation [here](https://www.elastic.co/guide/en/kibana/current/install.html). 
Kibana is provided for Linux and Darwin (macOS) as a .tar.gz package. I'm show the steps using the Darwin version (choose the correct version according your operation system). 

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

#### Checking that Kibana is running

Kibana instance should be running at [`http://localhost:5601`](http://localhost:5601) in your browser if you run with default configuration (see [here](https://www.elastic.co/guide/en/kibana/current/settings.html) for configuring Kibana). We can access the Kibana web user interface via your browser [`http://localhost:5601`](http://localhost:5601).  When we first start Kibana, it will create a new Elasticsearch index called `.kibana` where it stores the visualizations and dashboards. Because this is the first time starting it, we should be prompted to configure an **index pattern**. We should see something similar to:


![KibanaWelcome](https://github.com/jorditorresBCN/Assignments-2017/blob/master/KibanaScreenWelcome.png "KibanaWelcome")



<a name="Tasks33"/>

## Task 8.3: Install logstash and beats

The simplest architecture for the Beats platform setup consists of one, Elasticsearch, and Kibana. The Beats insert the transactions directly into the Elasticsearch instance. If you want to perform additional processing or buffering on the data, however, you’ll want to install Logstash. An important advantage to this approach is that you can use Logstash to modify the data captured by Beats in any way you like. You can also use Logstash’s many output plugins to integrate with other systems.

To download and install Logstash, use the commands that work with your system according [this webpage](https://www.elastic.co/guide/en/beats/libbeat/current/logstash-installation.html). In our case we used Darwin (macOS): 

```
curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.4.1.zip
unzip logstash-5.4.1.zip
```


In the `logstash-5.4.1` directory, it can be `/usr/share/logstash` if you installed Logstash from a .deb package in linux, we need to create the configuration file `twitter.conf` (included in this github) that onfigure Logstash to listen on port 5044 for incoming Beats connections and to index into Elasticsearch as:

```
input {
  	beats {
    	port => 5044
  	}
  	twitter {
      	consumer_key => "XXXXXXXXXXXXXXXXXXXXX"
      	consumer_secret => "XXXXXXXXXXXXXXXXXXXXX"
      	oauth_token => "XXXXXXXXXXXXXXXXXXXXX"
      	oauth_token_secret => "XXXXXXXXXXXXXXXXXXXXX"
      	keywords => [ "barcelona" ]
    	full_tweet => true
  	}
}

filter {
}

output {
  stdout { 
    codec => dots
  }
  elasticsearch {
    hosts => "localhost:9200"
    index => "twitter" 
    document_type => "tweet" 
    template => "twitter_template.json"
    template_name => "twitter"
  }
}
```
You can include more `keywords` as:

```
keywords => [ "barcelona", "london", "paris" ]
```

Also in the `logstash-5.4.1` folder create the file `twitter_template.json` that indicates the template used (included in this github) :

```
{
  "template": "twitter",
  "order":    1,
  "settings": {
    "number_of_shards": 1
  },
  "mappings": {
    "tweet": {
      "_all": {
        "enabled": false
      },
      "dynamic_templates" : [ {
         "message_field" : {
           "match" : "message",
           "match_mapping_type" : "string",
           "mapping" : {
             "type" : "string", "index" : "analyzed", "omit_norms" : true
           }
         }
       }, {
         "string_fields" : {
           "match" : "*",
           "match_mapping_type" : "string",
           "mapping" : {
             "type" : "string", "index" : "analyzed", "omit_norms" : true,
               "fields" : {
                 "raw" : {"type": "string", "index" : "not_analyzed", "ignore_above" : 256}
               }
           }
         }
       } ],
      "properties": {
        "text": {
          "type": "string"
        },
          "coordinates": {
          "properties": {
             "coordinates": {
                "type": "geo_point"
             },
             "type": {
                "type": "string"
             }
          }
       }
      }
    }
  }
}

```


To install the required plugin, run the following command inside the logstash directory according [this webpage](https://www.elastic.co/guide/en/beats/libbeat/current/logstash-installation.html). In our case we used (mac): 

```
./bin/logstash-plugin install logstash-input-beats
./bin/logstash-plugin update logstash-input-beats
./bin/logstash -f twitter.conf 
```

Warning: It a error `"Limit of total fields [1000] in index [twitter] has been exceeded"` appears like: 

``` 
[2017-06-03T21:18:55,963][WARN ][logstash.outputs.elasticsearch] Could not index event to Elasticsearch. {:status=>400, :action=>["index", {:_id=>nil, :_index=>"twitter", :_type=>"tweet", :_routing=>nil}, 2017-06-03T19:18:55.000Z %{host} %{message}], :response=>{"index"=>{"_index"=>"twitter", "_type"=>"tweet", "_id"=>"AVxvZWi_fVHNJgzwxRRA", "status"=>400, "error"=>{"type"=>"illegal_argument_exception", "reason"=>"Limit of total fields [1000] in index [twitter] has been exceeded"}}}}
```
You can add the following command using the Dev Tools section:
```
PUT twitter/_settings 
{ 
   "index.mapping.total_fields.limit": 2000 
}
```
![Twittertotalfieldexceeded](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Twittertotalfieldexceeded.png "Twittertotalfieldexceeded")



<a name="Tasks34"/>

## Task 8.4:  Analizing our Twitter data

Now we are ready to start to analize our Twitter Data:
1. Select the `keywords` in the `logstash-5.4.1/twitter.conf` file.
2. start elasticsearch, kibana and logstash using different xterm in order to see if during the initialization process any error occurs
are corrently 
```
elasticsearch-5.4.1/bin/elasticsearch 
curl -XDELETE 'http://localhost:9200/twitter'
kibana-5.4.0-darwin-x86_64/bin/Kibana 
logstash-5.4.1/bin/logstash -f logstash-5.4.1/twitter.conf
```

We are ready to access the Kibana web user interface via your browser [`http://localhost:5601`](http://localhost:5601).  Uncheck the Index contains time-based events option. Replace `logstash-*` with twitter and use as timestamp field `@timestamp`:

![configureIndexPattern](https://github.com/jorditorresBCN/Assignments-2017/blob/master/configureIndexPattern.png "configureIndexPattern")

Click `Create` button. Kibana will now show you the index definition for the twitter index. You can see all of the fields names, their data types and if the fields are analyzed and indexed. This provides a good high level overview of the data configuration in the index. Y

In order to treate new index pattern you can use Management–>Index Patterns–> Add New.


At this point, your index pattern is saved. You can now start discovering your data. Click on the Discover link in the navigation bar. This opens the Discover view which is a very helpful way to dive into your raw data. You should see something similar to this:

![discover](https://github.com/jorditorresBCN/Assignments-2017/blob/master/discover.png "discover")

At the top of the screen is the query box which allows you to filter your data based on search terms. Enter `messi`as a example in the filter box to filter results that only contain that field. On the left of the screen is the list of fields in the index. Each field has an icon to the left of the field name that indicates the data type for the field. If you click on the field name, it will expand to show you sample data for that field in the index. 

![discovermessi](https://github.com/jorditorresBCN/Assignments-2017/blob/master/discovermessi.png "discovermessi")


<a name="Tasks35"/>

## Task 8.5:  Elastic Cloud (optional)

The easiest and fastest way to get started with Elasticsearch is to spin up a free trial on Elastic Cloud [(here)](https://www.elastic.co/cloud/as-a-service/signup?ultron=kibana-get-started&blade=touch&hulk=email&mkt_tok=eyJpIjoiTVRGa05tTTJZVGN4WXpabSIsInQiOiJLT3p4RkJoSmZYcnF1SnZtdU1IXC9VNTArWHVkYTdvaXllMXdYXC8wWjROSTJESmpoN0x2T0hjbmNoV1V5b2VmdlB6VkFrdXBMaUNOZjlkRUkzQ1lwekNPbWVNUWVCSDZaaTA0ajlFXC9NTldpNlZCWThpdW1JSmdCT2pqeGpaNVVvQSJ9) , which comes with a free Kibana instance.

<a name="Tasks36"/>


## Task 8.6:  Student project: Visualize and Dashboard (80% grade of the lab)
Use the Visualize and Dashboard links in the navigation bar to obtain a awesome Dashboard that describes your data. 


![KibanaDashboard](https://raw.githubusercontent.com/jorditorresBCN/Assignments-2017/master/KibanaScreenDashboard.png "KibanaDashboard")

<a name="Tasks37"/>

## Task 8.7:  Acknowledgements

I will thank my former students [Victoria Gabante](https://www.linkedin.com/in/victoria-gabante-guerra-03a679b2) and [Osmar Rodríguez](https://twitter.com/osmar106), who provided insight and expertise in this hands-on with their project at my master course [CC-MEI](http://jorditorres.org/CC-MEI-2017/) at UPC.

I would also like to show my gratitude to [Dani Cea](https://www.linkedin.com/in/danicea/) for his help on an [earlier version of this hands-on](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab.cc.mei.eskibana.2015.pdf) 2 years ago. 


# How to Submit this Assignment:  
Submit **before the deadline** to the *RACO Practicals section* a "LabX.txt" following the guidelines indicated in the last theory class. 
