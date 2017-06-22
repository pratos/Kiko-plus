---
layout: post
published: true
title: Docker for Data Science - Part 1
---

<div align="center"><img src="https://www.docker.com/sites/default/files/docker_toolbox_0.png" width="500px;" height="350px;"/></div>

<p align="center"><b>Source: Docker Homepage</b></p>

### The situation
***
As Data Scientist/Data Analysts/Data Science enthusiasts, we are by far a messier version of our Software Engineering counter parts. We like quick, dirty solutions. Code base that sometimes puts a double burst cheese pizza to shame (yum though!).

We like to experiment, change things on the fly and get the results out as soon as possible...coz deadlines *shakes head*. All this adds to the mess that already is - dependencies, obscure packages and platform dependent issues. To replicate the same experiment takes more than just writing `random.seed(4)`, and there comes the classic problems: 

> _"Okay! But that ran on my system"_

> _"I have sent every code script, data and other things. Strange I haven't missed any."_

> _"Too lazy to install everything from scratch, hate Linux/Windows/Mac"_

> _"Can't install the package that you used, can you help me out?"_


What if I told you, as Data Scientist one can easily get all this sorted? Docker is the answer! 

There'll be a section of readers who would be thinking, _"Docker is nuts! Only DevOps folks know it in and out!"_. True in some respects, but if there's a way to standardize our Data Science Workflow and be productive then why not try it out. Knowing just small bit of Docker would be enough as a starting point.

### What is Docker?
***
According to Docker's official website:

> Docker is the world’s leading software container platform. Developers use Docker to eliminate “works on my machine” problems when collaborating on code with co-workers. Operators use Docker to run and manage apps side-by-side in isolated containers to get better compute density. Enterprises use Docker to build agile software delivery pipelines to ship new features faster, more securely and with confidence for both Linux and Windows Server apps.

Docker provides us an isolated container, where we can add all the things that we need to for our experiment to run. We could also create a full fledged web application that can be used as a production grade. So right from experiments to production applications, Docker provides a good means to get our Data Science applications out in the real world.

To learn Docker, we need to understand the terminologies and tools on offer and what we can do with it.

__Docker Terminology:__

1. __Docker Containers:__ Small user-level virtualization (isolation) that helps you install, build and run your code/workflow. All the code would be continuosly running in these containers.
2. __Docker Images:__ An image is an inert, immutable, file that's essentially a snapshot of a container. These are your actual committed containers (ones that have the process running, data stored, ports exposed to be used). Docker images are essentially the stored instances that you can (actually move around).
3. __Dockerfile:__ It is a YAML (almost) based file from which _Docker_ creates an image. It can be thought of as an automated script that has all the steps you want to execute.

You could be a Data Science enthusiasts who can share his code without documenting any single thing, `Dockerfile` (more on this later) is self-documenting. All you have to do is create a Docker image, upload it on DockerHub and share it with the world!

A good resource to know more about __Docker and Containerization technology__ as a whole read [this blog](https://medium.freecodecamp.com/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b)

In this blog post, we'll tackle very narrow problems of __Starting off with pre-built Docker images__ and __Environment packaging__.

### Docker installation
***
All the examples here are generic, except for the installation of _Docker_. The official _Docker installation_ documents for Windows and Mac are pretty straight forward. You can start off _Docker installation_ using this [DigitalOcean blog post](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04).

(_Tip: For any Linux distro related installations, DigitalOcean articles are the real deal! Always search for them._)

### Docker for instant gratification
***
<div align="center"><img src="https://i.imgflip.com/1nthdr.jpg" title="made at imgflip.com"/></div>

Getting the right tool to do things is hard and getting it installed on your system is a tough job sometimes. 

Let's take an example Python's `cryptography` installation on your Windows system, unless you have the right Microsoft authorized C++ compiler the package won't `pip install`. After ardorous, time sapping attempts you might get things running. In Ubuntu, installation is just a breeze.

[Read my experiences](https://medium.com/becoming-human/tensorflow-serving-by-creating-and-using-docker-images-336ca4de8671) in compiling TensorFlow Serving, that would give you a fair idea that setting up platform to do things, especially obscure things take time. 

Docker solves this, [DockerHub](https://hub.docker.com/) has a host of images uploaded by individuals and tech teams alike. Right from [Hello World images](https://hub.docker.com/_/hello-world/) to [LISP images](https://hub.docker.com/r/sergiobuj/cl-jupyter/). 

What do you need to do? Just `docker pull <username>/<image-name>`. It would download the image for you. Try out the following:

```
user@user:~$ docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
78445dd45222: Pull complete 
Digest: sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
Status: Downloaded newer image for hello-world:latest
```

To run the _docker image_ run command as below:

`docker run hello-world`

Should give you output:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```

Generally the _docker files_ that you would use for Data Science purposes would be much complex with a variety of components to take care of: _ports, volumes and many other things._

We'll look at how to run a full fledged _Anaconda_ jupyter notebook that has all the components pre-installed. (An easier version of _conda installation_). 

```
user@user:~$ docker pull continuumio/anaconda3
Using default tag: latest
latest: Pulling from continuumio/anaconda3
8ad8b3f87b37: Pull complete 
26e5bbd29116: Pull complete 
26a23cde1ff7: Pull complete 
0947a413f98b: Pull complete 
Digest: sha256:d04148529d340097c08677061e7d08f42e13a51dbdbf7488d92af0a65fd82973
Status: Downloaded newer image for continuumio/anaconda3:latest
```

To log in the docker container, we can run the command below:
    - `docker run -i -t continuumio/anaconda3 /bin/bash`
    
To run Jupyter notebook in a `no-browser` mode, run the command below:
    - `docker run -i -t -p 8888:8888 continuumio/anaconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && mkdir /opt/notebooks && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip='*' --port=8888 --no-browser"`
    
- Here, let's discuss what the 2nd command is doing:
    * `-i` is running the image interactively.
    * `-t` is allocate a pseudo-TTY.
    * `-p` is connect/publish the container ports to host. Here localhost:8888 to 8888 of container.
    * Downloading `jupyter` and starting the notebooks in the `--no-browser` mode.
        
 - A few more that you should know are:
     * `-d` is to run container in the background, without accidently closing it.
     * `-v` is to bind a volume to the container, useful when you want your notebooks and/or data to be reflected in the container from your local system.

Go to the browser and run http://localhost:8888 to start running your experiments. Pretty nifty, if you want to start off easily using a ready-made docker container.

But, all of this doesn't solve our main problem: __Managing Environments__.

### Manage Environments and let others use that easily!
***
For my daily work, I create virtual environments for each project using `Anaconda` distribution. It provides a very easy means to share code along with the `environment.yml` file that can be exported and used to install the entire environment. 

There's a good portability here, mind you, but still cross operating system (OS) installations may break. That's where _docker_ shines and with a great deal of eco-system components like _docker-compose_ and _docker-machine_, it is easier than ever to get up and running with a tangible Data Science application.

### Case: My colleague is fed up of sharing code with his friend, who isn't exactly someone who would install obscure dependencies and actually run the code. We want to provide him ways to change and run the code with minimal headache.
***
<div align="center"><img src="https://i.imgflip.com/1nth2e.jpg" title="made at imgflip.com" height="600px;" width="420px;"/></div>

To start off, we'll be creating a `Dockerfile`. As explained above, `Dockerfile` would take care of setting up the image for you that includes _downloading the base images, setting the maintainer name-id, installing the required ubuntu/debian programs, installing language dependencies_ and many more things.

We'll be creating a python3.5 based Docker image that would contain folders `data` and `notebooks` with ports 8888 exposed to connect to `jupyter notebooks`.

(_Note: If any of the python modules can't be installed via pip, it would be safe to write `RUN` commands to run them safely_).

Create a folder with the tree structure as below:
```
.
├── data
│   └── readme.md
├── Dockerfile
├── notebook
│   └── readme.md
├── README.md
└── requirements.txt
```

We have the requirements.txt file as below:
(_Note: Do `pip freeze > requirements.txt` to generate one for your conda environment_ )

<script src="https://gist.github.com/pratos/5460f87cc357cf2505914eed3118ee10.js"></script>

We'll start off with getting smaller components for our `Dockerfile`.

- base image: 
    
    To make things simple, the base image that we'll use is the `python:3.5.3-onbuild` image. All we need is a requirements.txt file that would have all the python modules to be installed. To recap, it does the following:
      
     * Install python 3.5.3
     * Pull in your source and put it in a conspicuous and runnable place.
     * Install your requirements.txt file.
     
(__Note: This would create a Debian container, not an Ubuntu one__)


```python
FROM python:3.5.3-onbuild
```

- Updating the repository sources


```python
RUN apt-get update
```

- Creating folders for `data` and `notebooks`


```python
RUN mkdir /data
RUN mkdir /notebooks
```

- Adding a temporary log folder in `/tmp`, using:


```python
RUN mkdir /tmp/tflearn_logs
```

- Make those three folders as `VOLUMES` so that they can be accessible through the host machine too. 

    The main reason we have `VOLUMES` is that the `data` and `notebooks` can be shared separately, not through the images that we create (_Docker best practices states that it isn't good to share data with Images, it could be done but avoided).


```python
VOLUME ["/data", "/notebooks", '/tmp/tflearn_logs']
```

Sometimes we might have installations like `curl` or `cron` that aren't exactly in our base image. We'll `RUN` the installations as below:


```python
RUN apt-get install cron -yqq \
    curl
```

- We'll expose the port:
    * Other for Jupyter: 8888


```python
# jupyter
EXPOSE 8888
```

- We'll have to make an arrangement somehow to start the jupyter notebook as we run our image. For that, we'll use `CMD` to run our commands:


```python
CMD jupyter notebook --no-browser --ip=0.0.0.0 --allow-root --NotebookApp.token='demo'
```

Finally, we have our _Dockerfile_ as below:

<script src="https://gist.github.com/pratos/eafb92540725d8fb81a58d3e5eee126c.js"></script>

We need to build the _docker image_ by running the following command:


```python
(sudo) docker -t build <image-name>
```

A simple inspection (below), would give you the statistics for the image generated:

```
user@user:~/docker-demo$ sudo docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
demoimage                latest              9d556dd63886        9 minutes ago       1.37 GB
```

Now the real test comes in, we need to test it by running the image and checking whether our volumes work or not!

Follow the commands below:


```python
sudo docker run -d -v ~/docker-demo/notebooks:/notebooks -v ~/docker-demo/data:/data \
        -v ~/docker-demo/logs:/tmp/tflearn_logs -p 8888:8888 -i demoimage
```

If you do a `sudo docker ps`, you would get the following output:
    
```
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS              PORTS                                            NAMES
fadf5e0646d5        demoimage             "/bin/sh -c 'tenso..."   9 seconds ago       Up 8 seconds        0.0.0.0:6006->6006/tcp, 0.0.0.0:8888->8888/tcp   clever_payne
```

So we are all set to test the docker image:

- Fire up the http://localhost:8888 to get the jupyter notebook.

The container and host folders would be synced and persisted, no need to worry about losing the notebook,data and logs. 

The last step would be saving this image to DockerHub as a public image or in a private repository to use it later.


```python
# Login to DockerHub (_Note: You need to create your DockerHub account first_)
sudo docker login
```

Once login is succeeded, we need to tag the image and then push the image:


```python
sudo docker tag <image-name> <login-id>/<image-name>
sudo docker push <login-id>/<image-name>
```

e.g. 
- sudo docker tag demoimage pratos/demoimage
- sudo docker push pratos/demoimage

So here it is, your very own _docker image_. My colleague would be happy to see this, like him even you could do more with _docker images_. Do try this out.

We would be looking at a few more things that _Docker_ offers in the next post. To all the _DevOps_ folks do provide feedback to this post and share it with all of us.

__Sources:__
1. [For more indepth and simple understanding on how to write a Dockerfile](https://www.digitalocean.com/community/tutorials/docker-explained-using-dockerfiles-to-automate-building-of-images)
2. [Docker Documentation](https://docs.docker.com/)
