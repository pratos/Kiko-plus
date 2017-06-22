---
layout: post
published: true
title: Celery Setup for Windows
---

Over the past few months I've been falling for Ubuntu's terminal. Windows installations look very daunting, but since I'm married to Windows for the forseeable future I'll be continuing with Celery installation for Windows (duh!).

## Celery installation

![Celery](http://www.markon.com/sites/default/files/styles/product_banner/public/prdimg//RSS_Celery_Sticks.jpg?itok=irLWirI8)

### Step 1: Get a Broker!

What Celery needs is a broker, to deal with all the incoming messages. One that would be the bouncer on a cold, night in Koregaon Park pub. We'll go with RabbitMQ (coz that's what the official [Celery page](http://docs.celeryproject.org/en/latest/getting-started/first-steps-with-celery.html) endorses).

Download the RabbitMQ [Windows binary](http://www.rabbitmq.com/install-windows.html). Once, done run the .exe file. 

There's a surprise for you if you don't have Erlang (oops!). So now you are routed to the Erlang website to download Erlang.

### Step 0: Er...lang?

Go to the website (or you would be routed to [this page](http://www.erlang.org/downloads)). 

Download the ```OTP 19.2 Windows 64-bit Binary File (101891457)``` (or whichever is your system architecture).

Run the .exe file and start over with Step 1.

Note: Microsoft Visual C++ would pop-up (annoying MS Builds), if you don't have the required things for Erlang installation. __Be Patient!__ And (annoyingly) it would tell you to restart. Restart your system for the setup to take effect. How I miss ```source .bashrc```

There's lot of configurations we could do to customize our RabbitMQ Server, but that's for another day. Check [this page](https://www.rabbitmq.com/configure.html) for the same.

### Step 2: Where the Server @?

Unlike Ubuntu, where the server starts off after your do ```$ sudo apt-get install rabbitmq-server```, we have to do a bit of *click-click* to get things up and running. Windows does provide a background process (when initialized, see *below*).

![Task Manager](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/rabbitmq_bkprocess.png)

But to get a shiny web app interface, we do have to turn on the management plugin ([Source](http://arcware.net/installing-rabbitmq-on-windows/)). Follow the steps to install it:

* Open the command prompt and navigate to the location:__C:\Program Files\RabbitMQ Server\rabbitmq_server-3.6.6\sbin__
* First do a Server restart: 

```
C:\Program Files\RabbitMQ Server\rabbitmq_server-3.6.6\sbin>rabbitmq-server restart

          RabbitMQ 3.6.6. Copyright (C) 2007-2016 Pivotal Software, Inc.
##  ##      Licensed under the MPL.  See http://www.rabbitmq.com/
##  ##
##########  Logs: C:/Users/prato_s/AppData/Roaming/RabbitMQ/log/RABBIT~1.LOG
######  ##        C:/Users/prato_s/AppData/Roaming/RabbitMQ/log/RABBIT~2.LOG
##########
          Starting broker...
completed with 6 plugins.

```
    
    __Note:__ Keep the above service running, I had to keep it running at the backend on command prompt. Do not close it.
    
* Then enable the service and check for the service status:

``` 
C:\Program Files\RabbitMQ Server\rabbitmq_server-3.6.6\sbin>rabbitmq-plugins enable rabbitmq_management
Plugin configuration unchanged.

Applying plugin configuration to rabbit@DESKTOP-R0SMU94... nothing to do.

C:\Program Files\RabbitMQ Server\rabbitmq_server-3.6.6\sbin>rabbitmqctl status
```
    
* Go to [this link](http://localhost:15672/). Login as *guest*
    Credentials:
    Username: guest
    Password: guest

* Below is the webpage visible to you after successful login:
![WebPage](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/rabbit_login.png)

* You can add users from the admin page. Default admin i.e *guest* can be used in the broker url in the following format:

```
broker_url = 'amqp://guest:guest@localhost:15672//'
```

### Step 3: Celery install (Actual)

After all the above things we need to install *Celery for Python*. Now go to the virtual environment/create one using ```conda create --name <environment-name> python=<version:2.7/3.5>``` where you want to install *Celery*.

* Do ```pip install celery```
* After installation is done, check whether we are able to import from *Celery*
* [EDIT @14/01/2017]: Celery 4.0 does not have support for Windows, they have dropped it. So downgrade to ```pip install celery==3.1.25``` and it works. But since I had to port my work to Ubuntu 16.04 abandoned the Windows development.

```
(venv) C:\Users\user1>python
Python 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> from celery import Celery
>>>
```
    
That was all for the *Celery* installation. Would keep posted on __*How to create a Celery Application?*__

#### Sources & Reading:
1. [RabbitMQ for beginners - What is RabbitMQ?](https://www.cloudamqp.com/blog/2015-05-18-part1-rabbitmq-for-beginners-what-is-rabbitmq.html)
2. [Official RabbitMQ](http://www.rabbitmq.com/configure.html)
3. [RabbitMQ Ubuntu 16.04 Installation](https://hostpresto.com/community/tutorials/how-to-setup-rabbitmq-server-on-ubuntu-16-04/) [EDIT @14/01/2017]
4. [How to install RabbitMQ for MacOS](http://docs.celeryproject.org/en/latest/getting-started/brokers/rabbitmq.html#broker-rabbitmq)
