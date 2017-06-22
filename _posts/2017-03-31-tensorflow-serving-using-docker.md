---
layout: post
published: true
title: Tensorflow Serving by creating and using Docker images
---


Deep Learning (DL) and for a good amount, Machine Learning (ML) suffers from the lack of a proper workflow that makes things simple for the research to directly translate into production. There's a dearth of easy ways to do it, many cloud providers (Microsoft Azure ML Studio, Google Cloud Platform) and independent servcice providers (BigML) have made doing ML production a bit easy.

Still there's a lot of stress to get an ML solution to production. Like how Software Engineering has matured with an eco-system of easing the developer's load to get things into production, ML has still a way to go. About DL, it still more nascent that traditional ML. 

TensorFlow serving debuted in 2015 and has been garnering more attention within the Deep Learning community. Google has been subtly aggressive about TensorFlow, and that the adoption rate is pretty strong. Statistics says it all:

<span>&nbsp;</span>

<div align="center"><img src="https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/comparison.png"/></div>

<span>&nbsp;</span>

<div align="center"><img src="https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/googlesearch.png"/></div>

<span>&nbsp;</span>

The sheer monstrosity in the volume is evident in the above images. TensorFlow isn't just the media hype, it gives in so many tools that makes life easy for a Deep Learning researcher. When doing experiments -- __[TensorBoard](https://www.tensorflow.org/get_started/summaries_and_tensorboard)__, it's integration with other open source projects (namely __Docker, Hadoop, Kubernetes__) and other new things (like __XLA Compiler__) that makes working on TensorFlow exciting. Another important framework that TensorFlow provides us is __[serving](https://tensorflow.github.io/serving/)__. 

In this part we'll try to get TensorFlow Serving model server up and running.

***

#### What is TensorFlow Serving?

>TensorFlow Serving is a flexible, high-performance serving system for machine learning models, designed for production environments. TensorFlow Serving makes it easy to deploy new algorithms and experiments, while keeping the same server architecture and APIs. TensorFlow Serving provides out-of-the-box integration with TensorFlow models, but can be easily extended to serve other types of models and data.

At the start of this post, we lamented the dearth of Machine Learning workflows that would make developer and researcher's lives easier. TensorFlow Serving (tfserving) is a good step forward to do that. It sets up a continuous delivery pipeline, with lesser headaches that include serving a new model instantly, trying to get the requests to old model completed and then discard it. Below is how the tfserving home page describes it:

<span>&nbsp;</span>
<div align="center"><img src="https://tensorflow.github.io/serving/images/tf_diagram.svg" height="500px;" width="500px;"/></div>

tfserving makes it easy for retrained model to deployed to a production environment. It also goes a bit further to make it available to the outside world via a [gRPC server](http://www.grpc.io/) making it discoverable. We'll be making use of tfserving to run Inceptionv3 model. 

This post would deal with creating a custom Docker image (if you aren't familiar with Docker do read up on that, plus I'll be writing a few beginner posts on Docker) and pushing it to DockerHub or other managed container services. _Why making it as a docker image and pushing it to some repo? You'll get to know._

***

#### Experience with getting tfserving working

In the wintery October 2016, I was tasked with getting tfserving running for our internal research project. Being the naive person I was, tried a lot of things but couldn't get it working. With a sleuth of projects coming in, tfserving went to the backburner until a few weeks back when shit got real.

To give you a rough estimate of how much time I took to get tfserving working : 4 months (neglect) + 4 weeks (iterative failures). I'm an Arsenal fan so, `4` is in my horoscope. 

After a lot of reading (not much out there on the web), trail&error success did come in the form of a Docker image.

***

#### Pre-requisites

You could do it on your laptop, provided you have an excellent threshold for patience and absolutely smashing internet connection. I would advise you to use cloud VMs to execute the instructions in this blogpost. 

I used Google Cloud's Compute Engine to create a 30Gb (Standard Disk), Debian GNU/Linux8 OS VM with 2CPUs. This costs me around `$`49.75. We need a 30Gb disk to commit the images to the disk, had to struggle with this since I was using 10Gb disk. Also, another reason was free $300 Credits to use and no extra to pay.

You can use DigitalOcean/AWS/MS Azure, your choice or even a local system. We'll start with the actual execution, ssh in your vm and follow along.

***

#### 1. Downloading the `serving` repository from `tensorflow`:

- `git clone --recursive https://github.com/tensorflow/serving`
- `cd serving`

#### 2. Installing `Docker` in Debian:

- `sudo apt-get install \
     apt-transport-https \
     ca-certificates \
     curl \
     software-properties-common`
     
- `curl -fsSL "https://download.docker.com/linux/debian/gpg"`
- `sudo apt-key add -`
- `sudo apt-key fingerprint 0EBFCD88`

- `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"`
- `sudo apt-get update`
- `sudo apt-get install docker-ce`
- `apt-cache madison docker-ce`
- `sudo apt-get install docker-ce=17.03.1~ce-0~debian-jessie`
- `cd serving`

Try out a dry run by checking:

- `docker info`

(__NOTE:__ Add docker user to sudo group follow this [link](http://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo))

#### 3. Tweaking the Docker file in `serving`

TensorFlow using Docker file is an easy way to get started without much hassle, but there's still some things that you need to change. Firstly, there's no way the port 9000 (_used for tensorflow model server where it could be accessed_) and with the very recent build the Bazel version needs to be upgraded from 0.4.2 to 0.4.5 so that things don't break.

Here are two things that you need to do after doing `$ cd tensorflow_serving/tools/docker/`:
- Change the `Dockerfile.devel` by adding `EXPORT 9000`
- Change the `ENV BAZEL_VERSION 0.4.2` to `ENV BAZEL_VERSION 0.4.5` ([TensorFlow: Error #8738](https://github.com/tensorflow/tensorflow/issues/8738))

#### 4. Building the Docker file

- `docker build --pull -t $USER/tfserving -f ./tensorflow_serving/tools/docker/Dockerfile.devel .`

#### 5. Running the docker image, do the following:
- `docker run --name=tfserving -p 9000:9000 -d -it $USER/tfserving` (-d for detached mode)

#### 6. Saving the Docker image

This new image would be ~1.09Gb and serves as a base Docker image for `tfserving`. We'll be using [quay.io](https://quay.io) to save our images. It has a pretty decent plan, with 30 days free trial. Register and go to the tutorials page to know how to login through the CLI.

Follow along after logging in:

- `docker ps`
    ```
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
     d3f896a1dde6        pratos/tfserving    "/bin/bash"         4 seconds ago       Up 3 seconds        0.0.0.0:9000->9000/tcp   tfserving

    ```
- `docker commit d3f896a1dde6 quay.io/pratos/tfserving` (Commit the images)
- `docker push quay.io/pratos/tfserving` (Push the repository to quay.io)

You'll find your image in the repository.

<span>&nbsp;</span>
<div align="center"><img src="https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/quay.io.png"/></div>

#### 7. Compiling `serving` repo in the Docker Image

This is the most important sections of the process and the most time consuming. The original image would swell from ~1.09Gb to ~5.8Gb, patience and a stable internet connection is needed (advisable to keep this process running in `screen` or `tmux` so that it is persisted even when your local internet connection drops off).


Delete the container you ran previously and create a new one without the `-d` option or simple attach it (you'll be able to ssh through `docker attach <container>`)

- `docker run --name=tfserving -p 9000:9000 -it quay.io/pratos/tfserving`

You'll be in the docker image now, run the commands below carefully to make sure there's no problem later:

- `mkdir -p /work/`
- `cd /work/ `
- `sudo apt-get install git`
- `git clone --recurse-submodules https://github.com/tensorflow/serving`
- `cd /work/serving/tensorflow && ./configure`

The last command would ask you a few popup questions (most of which would be no). Configure that and run the next set of commands below:

- `cd /work/serving`
- `bazel build -c opt tensorflow_serving/... --jobs 1 --curses no --discard_analysis_cache`

One thing to note here is number of jobs that _Bazel_ spawns is 200, which would lead to virtual memory error. Since, we are on a budget VM (not exactly high end) the number of jobs set is `--jobs 1`. You could set it higher/tune it accordingly if you want to process to finish faster. 

I experienced a lot of errors when I started with `tfserving`, after reading up documentation and github issues, was able to crack this thing. Go out for a coffee or do something else once you start the compilation, otherwise waiting for _Bazel_ to compile would feel like the following:

<span>&nbsp;</span>
<div align="center"><a href="https://imgflip.com/i/1mhtjz"><img src="https://i.imgflip.com/1mhtjz.jpg" title="made at imgflip.com"/></a></div>
<span>&nbsp;</span>

If you don't get error, compile is successful and you can run the below command to check:

- `/work/serving/bazel-bin/tensorflow_serving/model_servers/tensorflow_model_server`

```
usage: /work/serving/bazel-bin/tensorflow_serving/model_servers/tensorflow_model_server

Flags:

--port=8500                      	int32	port to listen on

--enable_batching=false          	bool	enable batching

--model_name="default"           	string	name of model

--model_version_policy="LATEST_VERSION"	string	The version policy which determines the number of model versions to be served at the same time. The default value is LATEST_VERSION, which will serve only the latest version. See file_system_storage_path_source.proto for the list of possible VersionPolicy.

--file_system_poll_wait_seconds=1	int32	interval in seconds between each poll of the file system for new model version

--model_base_path=""             	string	path to export (required)

--use_saved_model=false          	bool	If true, use SavedModel in the server; otherwise, use SessionBundle. It is used by tensorflow serving team to control the rollout of SavedModel and is not expected to be set by users directly.
```

#### 8. Testing for InceptionV3 model

Our next aim is to make sure that Inceptionv3 works and we are able to get inferences once we provide test images through the server.

Downloading the InceptionNet models, to test inferences:
- `curl -O http://download.tensorflow.org/models/image/imagenet/inception-v3-2016-03-01.tar.gz`
- `tar xzf inception-v3-2016-03-01.tar.gz`
- `bazel-bin/tensorflow_serving/example/inception_saved_model --checkpoint_dir=inception-v3 --output_dir=inception-export`

_(A hefty output, with warnings peppered handful.)_

```
      WARNING:tensorflow:From /work/serving/bazel-bin/tensorflow_serving/example/inception_saved_model.runfiles/inception_model/inception/inception_model.py:150: histogram_summary (from tensorflow.python.ops.logging_ops) is deprecated and will be removed after 2016-11-30.
      
Instructions for updating:

Please switch to tf.summary.histogram. Note that tf.summary.histogram uses the node name instead of the tag. This means that 
TensorFlow will automatically de-duplicate summary names based on the scope they are created in.

. . . <WARNINGS CONTINUED>

2017-03-29 19:55:53.938435: W external/org_tensorflow/tensorflow/core/platform/cpu_feature_guard.cc:45] The TensorFlow library wasn't compiled to use FMA instructions, but these are available on your machine and could speed up CPU computations.

Successfully loaded model from inception-v3/model.ckpt-157585 at step=157585.

Exporting trained model to inception-export/1

WARNING:tensorflow:From /work/serving/bazel-
bin/tensorflow_serving/example/inception_saved_model.runfiles/tf_serving/tensorflow_serving/example/inception_saved_model.py:155: initialize_all_tables (from tensorflow.python.ops.data_flow_ops) is deprecated and will be removed after 2017-03-02.

Instructions for updating:

Use `tf.tables_initializer` instead.

Successfully exported model to inception-export
```


Starting the `gRPC server`: 

- `bazel-bin/tensorflow_serving/model_servers/tensorflow_model_server --port=9000 --model_name=inception --model_base_path=inception-export &> inception_log &`
    
```
[1] 45
```

Save a few images (any random would do, except for humans I guess) using wget/curl

Let's get the inferences:
- `bazel-bin/tensorflow_serving/example/inception_client --server=localhost:9000 --image=./images/dog1.jpg` 

``` 
D0329 20:43:26.487714047   26684 ev_posix.c:101]             Using polling engine: poll
outputs {
  key: "classes"
  value {
    dtype: DT_STRING
    tensor_shape {
      dim {
        size: 1
      }
      dim {
        size: 5
      }
    }
    string_val: "beagle"
    string_val: "English foxhound"
    string_val: "Walker hound, Walker foxhound"
    string_val: "bluetick"
    string_val: "tennis ball"
  }
}
outputs {
  key: "scores"
  value {
    dtype: DT_FLOAT
    tensor_shape {
      dim {
        size: 1
      }
      dim {
        size: 5
      }
    }
    float_val: 8.8880033493
    float_val: 5.99537038803
    float_val: 5.76968288422
    float_val: 3.91565990448
    float_val: 3.65689516068
  }
}

E0329 20:43:29.299981796   26684 chttp2_transport.c:1810]    close_transport: {"created":"@1490820209.299938346","description":"FD shutdown","file":"src/core/lib/iomgr/ev_poll_posix.c","file_line":427}
```

#### 9. Saving the final Docker container

Now that we have the server running on __port 9000__, <kbd>Ctrl</kbd>+<kbd>p</kbd> and <kbd>Ctrl</kbd>+<kbd>q</kbd> to quit the Docker container.

Run the commands below to make sure you commit the containers to images
- `docker commit d3f896a1dde6 quay.io/pratos/baseinception` (Commit the container to images)
- `docker push quay.io/pratos/baseinception` (Push the repository to quay.io)

So in this blog we looked at how to create a new docker image that can be now used just by downloading it from anywhere. DockerHub can also be used for the same, but quay.io felt better (mostly coz I was lazy to search and read up on DockerHub). 

That's all for now. Feel free to use the docker image: __docker pull quay.io/pratos/baseinception__

__Sources:__
1. [TensorFlow Serving using Docker](https://tensorflow.github.io/serving/docker)
2. [TensorFlow Serving Basics](https://tensorflow.github.io/serving/serving_basic)
3. [OSL Engineering - Medium Blog](https://medium.com/osldev-blog/tensorflow-serving-practical-introduction-9ce29ccd63f)
4. [ZenDesk Engineering - Medium Blog](https://medium.com/zendesk-engineering/how-zendesk-serves-tensorflow-models-in-production-751ee22f0f4b)
5. [Tensorflow Serving in 10 minutes](https://medium.com/@avloss/tensorflow-serving-in-10-minutes-a0d03f00ebeb)
