---
layout: post
published: true
title: DataViz Dashboards using Python, the Resources 
---

All the Data Science enthusiasts would agree on the fact that: __Data Visualization using Python is hard!__. We'll look at some means via which we can do data visualization & dashboards.

### RShiny, oh my precious!
***

<div align = "center"><img src="http://www.theswarmlab.com/img/portfolio/shiny.png" height="400px;" width="470px;"/></div>

__R__ rules in the Data Visualization space, to make those plots more user friendly, we have __RShiny__. Hands-down the best tool to do exploratory data analysis.

But there's still a catch here, __RShiny__ is still unintuitive for someone not familiar with __HTML,CSS especially Bootstrap__. With the __reactive programming__ concept, mastering RShiny is a difficult task. Plus, the packaging and using it in for a production environment (and expectations of clients for __RShiny__ to be a good looking interface) makes it difficult. __RStudio's offering, RShiny Server__ is a good choice, but to purchase the enteprise version a small startup may have to rent their existing rented workspace ([RStudio Pricing](https://www.rstudio.com/pricing/)). There's a free version too, but again setup headaches.

### Popular Visualization Tools, mainstream...
***

<div align = "center"><img src="https://tableau.lcsexams.com/images/TableauLogo.jpg"/></div>

<div align = "center"><img src="http://www.calumo.com/wp-content/uploads/2017/03/Microsoft-Power-BI.png"/></div>

<div align = "center"><img src="https://lh5.googleusercontent.com/-DcTUn7CcILQ/AAAAAAAAAAI/AAAAAAAAAA8/FbY-q6axZlQ/photo.jpg" height="400px;" width="470px;"/></div>

Tableau, PowerBI and Qlikview are choices, but the licence costs would drain all the money. Not like a Data Science guy would purchase hefty licences to make dashboards...not everyone makes dashboards everyday! So there's an option of going the Javascript way! But...

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">- ok, who&#39;s ready to learn javascript?<br>[30 hands go up]<br>- who&#39;s going to give it more than 1 week?<br>[30 hands go down]<br><br>Next day: &quot;JS SUCKS&quot;</p>&mdash; I Am Devloper (@iamdevloper) <a href="https://twitter.com/iamdevloper/status/874989025748189184">June 14, 2017</a></blockquote>

### JavaScript, FTW!
*** 

<div align = "center"><img src="https://d21ii91i3y6o6h.cloudfront.net/gallery_images/from_proof/9731/medium/1450213382/d3.png" /></div>

Javascript for DataViz is a crowded space with lots of libraries (as expected), with varying amounts of abstractions. The most popular one is [D3js](https://d3js.org/). You can check out the rest at [this link](https://www.sitepoint.com/15-best-javascript-charting-libraries/). 

### Python? whoa stop right there!
***

<div align = "center"><img src="https://s-media-cache-ak0.pinimg.com/600x315/c3/aa/7d/c3aa7dc37e6beb7319e01d6b9d1b1f22.jpg" /></div>

For Python, there are libraries that help you in Data Visualization. Most popular ones are: __matplotlib__ and its abstraction: __seaborn__. 

__Matplotlib__ can be considered as a low abstraction level plotting tool that helps anyone to build charts just like how you build __Lego structures__. It is painstaking...your eyes will bleed by the time your finish the graph...but it is worth the pain...if you aren't on a deadline.

<div align = "center"><img src="https://matplotlib.org/1.3.1/_static/logo2.png" /></div>

Quick, dirty plotting that's a feature of __ggplot2__ or __R__, for that matter, isn't exactly for Python. It follows the same __Grammar of Graphics__ philosophy, but there's a steep learning curve. 

__Seaborn__ on the other hand is pretty much a decent library to start off. The official page describes it as:

>Seaborn is a library for making attractive and informative statistical graphics in Python. It is built on top of matplotlib and tightly integrated with the PyData stack, including support for numpy and pandas data structures and statistical routines from scipy and statsmodels.

There's another way through which the pain of quick, dirty data visualization is possible. It is possible via __pandas'__ inbuilt plotting functions for `dataframes`.

[Plotly](https://plot.ly/) has excellent APIs to make your static Python plots interactive. That's another option, also good guys at Plotly have released a framework to create dashboards: [Dash](https://plot.ly/products/dash/), which has an HTML-Python hybrid interface.

Now all this was all good, what if we want to integrate everything in a dashboard using just python. Making a dashboard in Python! That too pure Python code? You must be kidding, right?

That's what I thought when I started out, but who knew there's always hope! 

If you want to get an idea about Python Visualization landscape, watch [Jake VanderPlas': The Python Visualization Landscape at PyCon 2017](https://www.youtube.com/watch?v=FytuB8nFHPQ)

### Bokeh
***

<div align = "center"><img src="http://people.math.sc.edu/etpalmer/Images/bokeh.png" /></div>


>Bokeh is a Python interactive visualization library that targets modern web browsers for presentation. Its goal is to provide elegant, concise construction of novel graphics in the style of D3.js, and to extend this capability with high-performance interactivity over very large or streaming datasets. Bokeh can help anyone who would like to quickly and easily create interactive plots, dashboards, and data applications.

Sounds fun, but first time I tried it...not that great! 

Recently, revisited back again to work on interactive __Jupyter notebooks__. I went from being excited about __Dash__, to crying and bookmarking __D3js__ tutorials. Things weren't the same for me, I was sure __Bokeh__ wasn't okay. 

Then deadlines came up and I had to act fast. Fired up the browser, YouTubed the hell out and came across Bokeh videos (via the NYC Taxi Data visualizations by Continuum). This post isn't about coding out a Bokeh plot or Dashboard, but providing resources (and bitching about other languages/tools). 

- [Watch for Inspiration](https://www.youtube.com/watch?v=GkysOB8_xsE&t=1040s)
- [Understanding the Bokeh basics - Strata conference 2016](https://www.youtube.com/watch?v=Cwnb_o0UORM&t=3110s)
- [Sarah Bird's Getting started with Bokeh](https://www.youtube.com/watch?v=9FlUFLmaWvY&t=170s)
- [Sarah Bird's Bokeh for Web Developers](https://www.youtube.com/watch?v=O5OvOLK-xqQ)
- [Casey Clements' Bokeh basics](https://www.youtube.com/watch?v=Kojrxqgecx4)
- [Bryan Van de Ven - Bokeh for Data Applications and Visualization](https://www.youtube.com/watch?v=h0y90MyGo-c)

Some github resources:

- [Bokeh notebooks](https://github.com/bokeh/bokeh-notebooks)
- [Bokeh slides](http://chdoig.github.io/scipy2015-blaze-bokeh/#/)
- [Bokeh nbviewer](http://nbviewer.jupyter.org/github/bokeh/bokeh-notebooks/blob/master/index.ipynb)

Even if blogs/videos are a great resource to start off, project documentation is the best resource if you get stuck. __Bokeh__ documentation is fairly good, sometimes the whole searches are a bit of a pain. Said that, it is still great!

- [Bokeh User Guide](http://bokeh.pydata.org/en/latest/docs/user_guide.html)
- [Bokeh Gallery - Inspired from RShiny Gallery](http://bokeh.pydata.org/en/latest/docs/gallery.html)

One great advantage of Bokeh is it plays nicely along with other libraries: [Leveraging Libraries](http://bokeh.pydata.org/en/latest/docs/user_guide/compat.html)

You can convert your exisiting `matplotlib` code to `bokeh`. 

For large datasets, there's always a problem of well...large number of data points that can't be fitted on a 2D space. We have a lot of ways through which we can do it. __Continuum__ has open sourced libraries that make this task easy:

- [Datashader](https://datashader.readthedocs.io/en/latest/)
- [Dask](https://github.com/dask/dask)
- [Holoviews](http://holoviews.org/)

A few advanced notebooks that could help:

- [Common pitfalls](https://anaconda.org/jbednar/plotting_pitfalls/notebook)
- [Datashader: Webinar](https://continuum-analytics.wistia.com/medias/8zu9idwoym?mkt_tok=eyJpIjoiTmpKbU9EZ3hOV0l4TnprNCIsInQiOiJiZlp1Yks2ekpXeG1kbTdIVEVuZ0g1WXVNR2h1RzJiSHhocXB0YVdaN0dWejZESGhQZTNjOGhOakQ5ZW9RR0tNUmo3amJvT0JIRmthblpSS1FWTjlUQT09In0%3D)
- [How to meaningfully use datashader](https://anaconda.org/jbednar/nyc_taxi/notebook)

By all means, these tools are something that I know of and does not include a lot of tools. Do comment in to add a few!
