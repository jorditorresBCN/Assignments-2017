# Lab 4: Interacting with users and services in the Cloud


## Task 4.1:  How to provide your services through the web?
As we show, we can plots in Python using libraries like matplotlib.  However, how to present our results through the web?  
Because D3 plays well with web standards like CSS and SVG, and allows to create some wonderful interactive visualisations in the web, many of my data science colleagues think that it is one of the coolest libraries for data visualisation.

This task will guide the student to get started in to understand [D3.js](https://d3js.org) which is, as the name suggests, based on Javascript. We will present a simple option to support data D3 visualisation with Python using the Python library [Vincent](https://github.com/wrobstory/vincent), that bridges the gap between a Python back-end and a front-end that supports D3.js visualisation. Vincent takes our data in Python format and translates them into [Vega](https://github.com/trifacta/vega), a JSON-based visualisation grammar that will be used on top of D3.

Firstly, we need to install Vincent:

```
torres@vm:~$  sudo pip install vincent
```

Using the list of most frequent terms(without hashtags) from our case study data set, we want to plot their frequencies:


```
torres@vm:~$  sudo pip install vincent
```

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
