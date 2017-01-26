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
Posar aqui l exemple de API de google maps i pintar-ho

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
