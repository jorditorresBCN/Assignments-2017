# Lab 8: Extracting and Analyzing data using Elastic Search and Kibana

**_Last update: 6-June-2017_**

In a previous hands-on we performed a data analysis using `matplotlib`. In this hands-on we are going to perform a data analysis using Kibana and Elastic Search.

* [Task 8.1: Elasticsearch](#Tasks31)
* [Task 8.2: Kibana](#Tasks32)  
* [Task 8.3: Longtash and Beats](#Tasks33)  
* [Task 8.4: Search bar](#Tasks34)  
* [Task 8.5: Using our Twitter dataset](#Tasks35)  
* [Task 8.56: Acknoledgements](#Tasks36)  

   
#  Tasks of Lab 8

<a name="Tasks31"/>

The goal of this lab is to fetch twitter data using logstash and then put it into elasticsearch. Finally i want to do visualisation using kibana. We will use *Beats* to collect information from Twitter, are open source data shippers that we will install as agents on our servers. Beats can send data directly to Elasticsearch or send it to Elasticsearch via Logstash, which we can use to parse and transform the data. We will build the following workflow:

![SlasticWorkflow](https://github.com/jorditorresBCN/Assignments-2017/blob/master/workflowElasticSearch.png "SlasticWorkflow")





## Task 8.1: Elasticsearch

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

Kibana instance should be running at [`http://localhost:5601`](http://localhost:5601) in your browser if you run with default configuration (see[here] (https://www.elastic.co/guide/en/kibana/current/settings.html) for configuring Kibana). We can access the Kibana web user interface via your browser [`http://localhost:5601`](http://localhost:5601).  When we first start Kibana, it will create a new Elasticsearch index called `.kibana` where it stores the visualizations and dashboards. Because this is the first time starting it, we should be prompted to configure an **index pattern**. We should see something similar to:


![KibanaWelcome](https://github.com/jorditorresBCN/Assignments-2017/blob/master/KibanaScreenWelcome.png "KibanaWelcome")



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

## Task 8.2222: Logstash configuration


In the `logstash-5.4.1` folder create the file `twitter.conf` (ncluded in this github) as:

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
  	user => elastic
	  password => changeme
  }
}
```

You can include more `keywords` as:

```
keywords => [ "barcelona", "messi" ]
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

### Realizando la primera prueba con Kibana

Después de todos estos pasos, es el momento de irnos a nuestro Kibana, y crearemos un nuevo index pattern, seleccionaremos que sea de tipo `tweeter` y en timestamp seleccionaremos `@timestamp`.


##### Figura configureIndexPattern

El index ha sido generado y ahora podríamos editar todos los campos que estamos ingiriendo, si queremos mostrarlos, analizarlos, etc:

Si nos fueramos ahora a Discover, podríamos ver que Kibana ya nos muestra datos de los logs que tenemos ya dentro de Logstash y Elasticsearch:

#### FALTA PANTALLA DE DISCOVER


### create index
Create the index pattern in Kibana via „Management–>Index Patterns–> Add New


IF THIS ERROR APPEARS: "Limit of total fields [1000] in index [twitter] has been exceeded"}

afegir a Dev Tools section
``` 
[2017-06-03T21:18:55,963][WARN ][logstash.outputs.elasticsearch] Could not index event to Elasticsearch. {:status=>400, :action=>["index", {:_id=>nil, :_index=>"twitter", :_type=>"tweet", :_routing=>nil}, 2017-06-03T19:18:55.000Z %{host} %{message}], :response=>{"index"=>{"_index"=>"twitter", "_type"=>"tweet", "_id"=>"AVxvZWi_fVHNJgzwxRRA", "status"=>400, "error"=>{"type"=>"illegal_argument_exception", "reason"=>"Limit of total fields [1000] in index [twitter] has been exceeded"}}}}
```
```
PUT twitter/_settings 
{ 
   "index.mapping.total_fields.limit": 2000 
}


```
En el  twitter.conf se puede ver el nombre del index que es twitter
##### FIGURA 2 Twittertotalfieldexceeded


<a name="Tasks35"/>

## Task 8.5:  Using our Twitter data

We are going to use our Twitter data which is stored in the twitter index. Uncheck the Index contains time-based events option. Our data is not yet properly setup to handle time-based events. We'll fix this later. Replace logstash-* with twitter.


*Warning: Keep the terminal open where Kibana was run to be able to keep the instance running. Create the Kibana folder in the same folder that ElasticSearch in order to use the default values  consideret in this hands-on.*

xxxx xxxxx xxxx 
We can access the Kibana web user interface via your browser [`http://localhost:5601`](http://localhost:5601). 

When we first start Kibana, it will create a new Elasticsearch index called `.kibana` where it stores the visualizations and dashboards. Because this is the first time starting it, we should be prompted to configure an **index pattern**. We should see something similar to:

##### FIGURA 1

Click `Create` button. Kibana will now show you the index definition for the twitter index. You can see all of the fields names, their data types and if the fields are analyzed and indexed. This provides a good high level overview of the data configuration in the index. You can also filter fields in the filter box. You should see something similar to this:

##### FIGURA 2


At this point, your index pattern is saved. You can now start discovering your data. Click on the Discover link in the navigation bar. This opens the Discover view which is a very helpful way to dive into your raw data. You should see something similar to this:

##### FIGURA 3

At the top of the screen is the query box which allows you to filter your data based on search terms. Enter coordinates.coordinates:* in the filter box to filter results that only contain that field. On the left of the screen is the list of fields in the index. Each field has an icon to the left of the field name that indicates the data type for the field. If you click on the field name, it will expand to show you sample data for that field in the index. Looked for the field named coordinates.coordinates. The icon to the left of that field indicates that it is a number field. If you click the name of the field, you can see that it expands. You should see something similar to this:

##### FIGURA 4 selleccionat Barcelona


<a name="Tasks36"/>

## Task 8.6:  Acknowledgements

I will thank my former students [Victoria Gabante](https://www.linkedin.com/in/victoria-gabante-guerra-03a679b2) and [Osmar Rodríguez](https://twitter.com/osmar106), who provided insight and expertise in this hands-on with their project at my master course [CC-MEI](http://jorditorres.org/CC-MEI-2017/) at UPC.

I would also like to show my gratitude to [Dani Cea](https://www.linkedin.com/in/danicea/) for his help on an [earlier version of this hands-on](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab.cc.mei.eskibana.2015.pdf) 2 years ago. 


# How to Submit this Assignment:  
Submit **before the deadline** to the *RACO Practicals section* a "LabX.txt" following the guidelines indicated in the last theory class. 
