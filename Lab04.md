#Lab4: Advanced Anaytics as a Cloud services

### usar l'API de Google per sentiment analysis d'imatges (o Watson)


## Task 3.4:  Data Visualization basics
There are different options to create plots in Python using libraries like matplotlib, ggplot, D3.js, etc.  Because D3 plays well with web standards like CSS and SVG, and allows to create some wonderful interactive visualisations in the web, many of my data science colleagues think that it is one of the coolest libraries for data visualisation. 

This task will guide the student to get started in  to understand [D3.js](https://d3js.org) which is, as the name suggests, based on Javascript. We will present a simple option to support data D3 visualisation with Python using the Python library [Vincent](https://github.com/wrobstory/vincent), that bridges the gap between a Python back-end and a front-end that supports D3.js visualisation. Vincent takes our data in Python format and translates them into [Vega](https://github.com/trifacta/vega), a JSON-based visualisation grammar that will be used on top of D3.
Firstly, we need to install Vincent:

```
torres@vm:~$  sudo pip install vincent
```

Using the list of most frequent terms(without hashtags) from our case study data set, we want to plot their frequencies:


```
torres@vm:~$  sudo pip install vincent
```
