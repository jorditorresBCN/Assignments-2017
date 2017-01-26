# Lab 4: Interacting with users and services in the Cloud


## Task 4.1:  How to provide your services through an API?
As we show, we can plots in Python using libraries like matplotlib.  However, how to provide our results through an API to consumers?
If you want to use python to build a small server or just want to provide web-base API with minimal efforts, you can just use python `SimpleHTTPServer` and `SocketServer` packages. 

Create the python file `OurService.py`:

```python
#!/usr/bin/env python
import SimpleHTTPServer
import SocketServer

class MyRequestHandler(SimpleHTTPServer.SimpleHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/':
            self.path = '/OurService.html'
        return SimpleHTTPServer.SimpleHTTPRequestHandler.do_GET(self)

Handler = MyRequestHandler
server = SocketServer.TCPServer(('0.0.0.0', 8954), Handler)

print 'Started my REST API on port 8954' 
server.serve_forever()

```
and lauch it as:

```
torres@vm:~$  python OurService.py
```
Now you can open your browser at http://localhost:8954 and obtain acces to `OurService.html`.

The question now is how we can generate our service `OurService.html` in order to be provided by the server. Imagine that we want to provide the plot obtained in the previous Lab. We have different options, however I suggest to use [D3.js](https://d3js.org). 

Because D3 plays well with web standards like CSS and SVG, and allows to create some wonderful interactive visualisations in the web, many of my data science colleagues think that it is one of the coolest libraries for data visualisation.

[D3.js](https://d3js.org) is, as the name suggests, based on Javascript. We will present a simple option to support data D3 visualisation with Python using the Python library [Vincent](https://github.com/wrobstory/vincent), that bridges the gap between a Python back-end and a front-end that supports D3.js visualisation. Vincent takes our data in Python format and translates them into [Vega](https://github.com/trifacta/vega), a JSON-based visualisation grammar that will be used on top of D3.

Firstly, we need to install Vincent:

```
torres@vm:~$  sudo pip install vincent
```

Using the list of most frequent hashtags from our case study data set, we want to plot their frequencies:


```
import vincent
 
word_freq = count_all.most_common(15)
labels, freq = zip(*word_freq)
data = {'data': freq, 'x': labels}
bar = vincent.Bar(data, iter_idx='x')
bar.to_json('term_freq.json')

```
At this point, the file term_freq.json will contain a description of the plot that can be handed over to D3.js and Vega. A simple template (taken from Vincent resources) to visualise the plot:

```html
<html>  
<head>    
    <title>Vega Scaffold</title>
    <script src="http://d3js.org/d3.v3.min.js" charset="utf-8"></script>
    <script src="http://d3js.org/topojson.v1.min.js"></script>
    <script src="http://d3js.org/d3.geo.projection.v0.min.js" charset="utf-8"></script>
    <script src="http://trifacta.github.com/vega/vega.js"></script>
</head>
<body>
    <div id="vis"></div>
</body>
<script type="text/javascript">
// parse a spec and create a visualization view
function parse(spec) {
  vg.parse.spec(spec, function(chart) { chart({el:"#vis"}).update(); });
}
parse("term_freq.json");
</script>
</html>
```
Save the above HTML page as `OurService.html`.
Now you can open your browser at http://localhost:8954 and obtain acces to `OurService.html`.

With this procedure, we can plot [many different types of charts](http://vincent.readthedocs.io/en/latest/) with `Vincent`.

If you are interested in building a real server, there are many good Python frameworks for building a RESTful API as [Flask](http://flask.pocoo.org/), [Falcon](http://falconframework.org/) and [Bottle](http://bottlepy.org/docs/dev/index.html).



## Task 4.2: How to provide our service combined with third-party services?

Twitter allows its users to provide their location when they publish a tweet, in the form of latitude and longitude coordinates. With this information, we are ready to create some nice visualisation for our data, in the form of interactive maps.

For this purpouse we will use [GeoJSON](http://geojson.org), a format for encoding a variety of geographic data structures and [Leaflet.js](http://leafletjs.com), a Javascript library for interactive maps.

GeoJSON supports a variety of geometric types of format that can be used to visualise the desired shapes onto a map. For our examples, we just need the simplest structure, a Point. A point is identified by its coordinates (latitude and longitude). In GeoJSON, we can also represent objects such as a Feature or aFeatureCollection. The first one is basically a geometry with additional properties, while the second one is a list of features. Our Twitter data set can be represented in GeoJSON as `aFeatureCollection`. In order to generate this GeoJSON data structure we simply need to iterate all the tweets looking for the coordinates field. A very important warning is that this field may not be present. Today many tweets are not includes their geographic location (many users prefer not indicate this information on their tweets).

This code will create the GeoJSON data structure (for tweets where the coordinates are explicitely given) and then the data are dumped into a file called geo_data.json:
```
fname = 'Lab3.CaseStudy.json'
with open(fname, 'r') as f:
    geo_data = {
        "type": "FeatureCollection",
        "features": []
    }
    for line in f:
        tweet = json.loads(line)
        if tweet['coordinates']:
            geo_json_feature = {
                "type": "Feature",
                "geometry": tweet['coordinates'],
                "properties": {
                    "text": tweet['text'],
                    "created_at": tweet['created_at']
                }
            }
            geo_data['features'].append(geo_json_feature)
 
with open('geo_data.json', 'w') as fout:
    fout.write(json.dumps(geo_data, indent=4))
```

Now, with the Leaflet.js Javascript library for interactive maps, we can create our `.html` file that will host the map.
Visit the [Leaflet Quick Start Guide](http://leafletjs.com/examples/quick-start/) for more details of how Leaflet.js can be used. The following `.html`page provides the maps with our geolocated tweets:

```html
<!DOCTYPE html>
<html>
<head>
	
	<title>Quick Start - Leaflet</title>

	<meta charset="utf-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	
	<link rel="shortcut icon" type="image/x-icon" href="docs/images/favicon.ico" />

	<link rel="stylesheet" href="https://unpkg.com/leaflet@1.0.3/dist/leaflet.css" />
	<script src="https://unpkg.com/leaflet@1.0.3/dist/leaflet.js"></script>

	<script src="http://code.jquery.com/jquery-2.1.0.min.js"></script>

	<style>
		#map {
    	height: 600px;
		}
</style> 

</head>
<body>


<!-- this goes in the <body> -->
<div id="map"></div>
<script>
// Load the tile images from OpenStreetMap
var mytiles = L.tileLayer('http://{s}.tile.osm.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
});

// Initialise an empty map
	var map = L.map('map');
// Read the GeoJSON data with jQuery, and create a circleMarker element for each tweet
$.getJSON("./geo_data.json", function(data) {
    var myStyle = {
        radius: 2,
        fillColor: "red",
        color: "red",
        weight: 1,
        opacity: 1,
        fillOpacity: 1
    };
 
    var geojson = L.geoJson(data, {
        pointToLayer: function (feature, latlng) {
            return L.circleMarker(latlng, myStyle);
        }
    });
    geojson.addTo(map)
});
map.addLayer(mytiles).setView([40.5, 5.0], 5);
</script>
</body>
</html>


```
A screenshot of the results:

![Lab4map2](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab4map1.png "Lab4map2")
![Lab4map1](https://github.com/jorditorresBCN/Assignments-2017/blob/master/Lab4map1.png "Lab4map1")



## Task 4.3:  How to provide our services through the an API?
Com hem dit, en general pero, els serveis usen APIs, no sempre son persones les uqe consumeixen els nostres serveis.
AIXO M HO HAIG DE MIRAR BE.
d aqui agafar les diferents opcions que hi ha
https://www.quora.com/What-is-a-good-Python-framework-for-building-a-RESTful-API

----
If you want to use python to build a small server or just want to provide
web-base api with minimal efforts, you can just use python SimpleHTTPServer package as below:

import SimpleHTTPServer
SimpleHTTPServer.test()

With only two lines, that's really awesome. Right? 

Or you can do as follows: 
python -m SimpleHTTPServer
the default port is 8000. 

TREURE-HO D AQUI
A simple website with python using SimpleHTTPServer and SocketServer, how to only display the html file and not the whole directory?


## Task 4.4:  Advanced Analytics as a Service in the Cloud
usar l'API de Google per treure etiquetes addicionals de les fotos de tweeter per saber més coses
http://stackoverflow.com/questions/10607621/a-simple-website-with-python-using-simplehttpserver-and-socketserver-how-to-onl

ho tinc a python serverChart.py


# How to Submit this Assignment:  
Be sure that you have updated your remote github repository with  the Lab `.ipynb` file generated along this Lab. Submit **before the deadline** to the *RACO Practicals section* a "Lab4.txt" file including: 

1. Group number
2. name and email of the members of this group
3. github url that contains your lab answers (the same as Lab1 and Lab2)
4. link to your dataset created in task 3.4.
5. add any comment that you consider necessary.
