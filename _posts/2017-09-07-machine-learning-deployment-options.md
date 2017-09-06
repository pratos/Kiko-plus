---
layout: post
published: false
title: 
---

- Letting a good ML model sit on your laptop is equivalent of killing a puppy. Those cute little babies are good, good boys. 
- We'll go from the obvious to the outright -- "No I can't do it" in this article:
* __Yes I can do this:__
* __Interactive Notebooks__: 
* __Dashboards__: 

* __No I can't do it:__

A few ways in which we can get ML Models working for us as well as end users (could be Data Engineering folks, could be Frontend Developers or could be other Data Scientists!).

__Wrapping ML Models creating Web APIs:__

Wikipedia describes __API__ as:
> In computer programming, an Application Programming Interface (API) is a set of subroutine definitions, protocols, and tools for building application software. In general terms, it is a set of clearly defined methods of communication between various software components. 
            
What we need is __Web API__. Web API allows us to expose our services (ML Models) to the outside world. Since the most popular architectural style that's used by majority of developers out there: __Representational State Transfer REST__ would be our choice. There's a few more out there: __SOAP__, __RPC__ & __Graph__, to name a few.

__For the R Magicians:__

Having being bombarded with questions, "I use R, don't know C/C++/Node/Python. What should I do?" Well, there's [__Plumber__](https://github.com/trestletech/plumber). Works well if there's a small prototype to be tested. __R__, with all its positives, isn't a language that is robust for web applications. 
__R__ is still not a primary citizen for Cloud vendors, hard to get your __R__ code scale up to hundreds of users via APIs. There are options, some of which we'll be looking below.

[OpenCPU](https://www.opencpu.org/) is another option that could be enticing for __R__ folks.

__Python Hackers:__
Data Science folks who are moving to __Python__ or already use __Python__ are spoilt for choices in terms of creating an API. With so many frameworks at disposal and the standardization of modules makes wrapping __Python__ ML models in an API a fairly smooth journey.

Options that you could look at are: __Flask, Django, Falcon__ (NOTE: There's who bunch out there, but you can find excellent documentation & blogs on the above. My preferred choice is Flask).

There's an interesting way in which __R__ users can solve their __wrapping API__ problem using __Python__. [__rpy2__](https://rpy2.bitbucket.io/) module provides a way to load __R Objects__ in a python environment and execute __R syntax__. For inferences, __rpy2__ code works fairly well. 

__Other Langugages:__
__Go, Julia, Javascript, Java, etc__ have more or less have a decent share of scientific computing, machine learning libraries & web services frameworks. Would love to hear in the comments the experiences that people had working with languages apart from __R & Python__.

__Wrapping ML Models & code using Containers:__

Containerization is everywhere, __Docker__ is still a buzzword. Embracing this buzzword will do a lot good to the Data Science community. Long plagued by the __"new libraries breaking existing code"__, __"works on my laptop"__ & __"I give up installing this thing!"__, __Docker__ is a decent solution on problem of packaging & running code elsewhere.

It will certainly help in the pursuit of making the code available. Give the __Docker__ container with your ML Model & code to DevOps/Software Engineering folks, you'll spread joy.

__Managed Services via Cloud Vendors:__

With the Cloud Computing & Containerization wave sweeping the software development industry, Data Science will benefit from the solutions that are provided.

__Amazon Web Services (AWS):__

AWS offers __Amazon EC2 Container Service__ if you plan to use Docker containerization. Their __serverless offering: AWS Lambda__ is pretty much cutting-edge and has a __Python__ SDK. __ML via sklearn is possible__ or __R Models via rpy2__.

__Microsoft Azure:__

__Microsoft Azure__ provides __Azure Machine Learning Studio__ that is a GUI to do Machine Learning on cloud. But it provides __experiments__ & in those execution cells to write up your own __R/Python__ scripts and run locally created __.rda/.pk__ files for inferences. Can create __R/Python__ notebooks and create __Web API__ using __Azure's SDK__. Very easy to create __Web APIs__ out of the __experiments__ that you run with a click of a button. Both __real-time__ and __batch__ predictions available. Like __AWS__, provides containerization services.

__Google Cloud Platform (GCP):__

They have more focus on __AI/ML__ as a cloud provider and offer range of services from __processing/transforming large volumes of data__ to __deploying ML Models__.

__Tensorflow__ is a first class citizen. You can easily deploy just the __Tensorflow__ models, will autoscale like (performance-wise we don't know) __AWS Lambda__ and keep the pricing low.

__GCP__ offers containerization services and also __Google App Engine__ that could provide option to deploy small machine learning APIs using just the code.

__ML Deployments as a service:__

[Domino Datalabs](https://www.dominodatalab.com/)
[Algorithmia](https://algorithmia.com/enterprise)
[yHat](https://www.yhat.com/)
[seldon.io](https://www.seldon.io/)
