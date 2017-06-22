---
layout: post
published: true
title: LISP? Hmm . . . 
---

> Finally, the author has moved to a Ubuntu system, after the horrific time with Windows. Adios Windows!

Randomly browsing through the vast treasure trove of blogs, there seems to come a time when you get fixated on something really special. I happened to come across this [very old article by Paul Graham](http://www.paulgraham.com/avg.html) that makes you go - _"Hmmm, okay"_. Mind you the article that titlliated me talks about the time when The Internet was becoming a sensational thing. 

Came across a meme about Lisp, would I definitely think about this? Maybe no.

![LISP](https://s-media-cache-ak0.pinimg.com/736x/b8/68/15/b8681575ee176ba12e4e514b5678f9f0.jpg)
[Source: Pinterest]


Digging up old archives and doing `Ctrl+Right Click` on the related links is my favourite pastime. I kept on munging through the very dusty archives of the web, articles that said _LISP was ahead of it's time_ and how _AI Winter set the rising sun of LISP prematurely to the horizon_. Reading things and actually experiencing them are two different beasts. After a short burst of reading about LISP, I kind of made up my mind to get some LISP action.


One of the oldest programming languages, LISP was the first one to be used for prototyping AI algorithms that were production ready ([Read this blog post](http://www.gigamonkeys.com/book/introduction-why-lisp.html)). *The name LISP derives from "LISt Processor". Linked lists are one of Lisp's major data structures, and Lisp source code is made of lists. Thus, Lisp programs can manipulate source code as a data structure, giving rise to the macro systems that allow programmers to create new syntax or new domain-specific languages embedded in Lisp.*


Your very own author is not a ninja hacker, but has done so much Ubuntu setups that he can claim to be a _hacker *ahem*_. Tried to do a quick installation of SBCL variant of Common LISP through [this link](https://common-lisp.net/project/common-lisp-beginner/sbclinstall_doc.html). Follow the step and you'll be able to login and write a few commands in your terminal to test the waters.


With the Jupyter notebooks spoiling us, the act of writing commands in a shell makes me queasy. I went out to search for jupyter kernels that would make my life and documenting LISP easy. I found three good github links:


- [robert-dodier/maxima-jupyter](https://github.com/robert-dodier/maxima-jupyter)
- [fredokun/cl-jupyter](https://github.com/fredokun/cl-jupyter)
- [Inaimathi/cl-notebook](https://github.com/Inaimathi/cl-notebook)


[robert-dodier/maxima-jupyter](https://github.com/robert-dodier/maxima-jupyter) and [fredokun/cl-jupyter](https://github.com/fredokun/cl-jupyter) provide a way to integrate LISP Kernel in a Jupyter notebook, which makes sense for me (and other users who love Jupyter). [Inaimathi/cl-notebook](https://github.com/Inaimathi/cl-notebook), on the other hand, is a different beast altogether and entirely made on LISP backend, with Javascript frontend. Like I said earlier, _"LISP? Hmmm...interesting!"_


A live demo [here](https://vimeo.com/97623064)


**Important note: I failed miserably at getting _cl-jupyter_ working on my laptop hence would update this once I can.**


Living in a world of instant gratification, _Docker images_ lets you download docker images and run them to run code without a single installation command. Like with Tensorflow and other obscurely difficult installations, there's always a kind soul who uploads Docker images for such hassles. The docker image for _cl-jupyter_ was just a Google search away.


You can find the docker image at [this link](https://hub.docker.com/r/sergiobuj/cl-jupyter/). To get going with Docker, here's a [link](https://docs.docker.com/engine/installation/) for basic Docker installation.


To get the docker image from DockerHub and run the jupyter notebook having LISP Kernel, follow the steps below:


```
$ docker pull sergiobuj/cl-jupyter
$ docker run -d -p 8888:8888 -v /notebooks:/notebooks sergiobuj/cl-jupyter
```

We need to create a repository to map it to /notebooks in the container so that the notebooks that we create during the docker sessions are persisted. To know more about data persistence, [read this](https://www.digitalocean.com/community/tutorials/how-to-work-with-docker-data-volumes-on-ubuntu-14-04)


Go to the location: http://localhost:8888 to find the familiar Jupyter interface. Go on and start programming. For reference, below is the screenshot for my browser:


![sbcl](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/sbcl.png)


Yet to start off with LISP, but curiosity about it has peaked certainly. Would be hacking away weekends over this, if I get time.

