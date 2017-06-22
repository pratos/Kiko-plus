---
layout: post
published: true
title: CircleCI for Continuous Integration
---

> After 32 agonizing, illiterate builds the author was able to setup CircleCI for testing, the next 4 he failed in the tests.

When I first entered IT, people who were alloted Testing domains were looked down upon. At the time, "Development" was something that was exciting. Being a noob (along with other noobs), my first impressions about testing were rather very negative. 

I was very wrong when my supposedly "great" code backfired in the deliveries. I didn't learn from that though since there wasn't a good test driven environment that my predecessors had set (plus my own disregard for my job). Mostly like below:

<p align="center">
  <img src="https://s-media-cache-ak0.pinimg.com/236x/b8/a0/11/b8a01136c3a25a32daa80fb48e7059bc.jpg"/>
</p>

***

## Something that is untested is broken.

[[Sourced from Flask Testing page](http://flask.pocoo.org/docs/0.12/testing/)]

Currently things aren't like that though, **a minor slight might cost a lot** (oh that rhymes!). With the hopes that our folks (again a rhyme!) move to test driven environment, had the opportunity to tinker with [CircleCI](https://circleci.com/) for our project. 

To start off, I wrote unit tests for our project code and ran that locally. Windows is the worst environment to test your code if that's being deployed in a Linux Server (common sense! (bad move, wasted a day to realize that Celery doesn't work)). 

Below is the project folder structure that I zeroed on for tests. I chose Python's `pytest` for the great amount of documentation and relative easy for writing tests (but mostly geek validated). 

```
user@userserver:~/server_backend$ tree
.
├── circle.yml
├── server_backend
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-35.pyc
│   │   ├── server.cpython-35.pyc
│   │   └── tasks.cpython-35.pyc
│   ├── rabbitmq_celery.md
│   ├── server.py
│   ├── tasks.py
│   └── test
│       ├── __pycache__
│       │   └── test_server.cpython-35-PYTEST.pyc
│       └── test_server.py
├── makefile
├── README.md
└── requirements.txt
```

The tests are quite easy to write down if you have a good understanding of what exactly you want to achieve. Since this blog post is not exactly on how to write tests for your code, would be skipping that. I referred the [official documentation](http://doc.pytest.org/en/latest/) and a few blog posts:

- [Getting Started with Pytest](https://jacobian.org/writing/getting-started-with-pytest/)
- [Pytest-Introduction](http://pythontesting.net/framework/pytest/pytest-introduction/)
- [Python unit testing with Pytest and Mock](https://medium.com/@bfortuner/python-unit-testing-with-pytest-and-mock-197499c4623c#.ca9j5mspl)
- [Some deep thing!](http://hackebrot.github.io/pytest-tricks/create_tests_via_parametrization/)

## Getting all this to Github

The main goal of this exercise was to setup a Continuous Integration (CI) workflow, wherein when I push a code to the github repo it automatically runs a build and all the tests are run on the code. This would be beneficial when I want to make sure that nothing breaks in the production environment before merging all the latest commits.

There are a lot of CI frameworks/products available -- Jenkins, TravisCI and CircleCI to name a few. CircleCI was an obvious choice since it provided **one free container** to setup our testing shop. Jenkins is open-source, but setup and maintainance could have been a big overhead. CircleCI with it's tight integration with Github was a promising prospect ([what I read](https://infinum.co/the-capsized-eight/continuous-integration-on-android-why-we-ditched-jenkins-for-circle-ci)).

## Initial setup

We need to add the CircleCI application to our Github and give access to it initially. For this go to the CircleCI homepage and signup

![signup](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/circlecisignup.png)

You would be redirected to the access page and then to the dashboard that would look like below:

![dashboard](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/circleciproj.png)

You can start building your project once you reach here. But there's a caveat, you need to have some tests in your repo inorder to build the project successfully. (Not very sure, since I had the folder structure messed up one time and the tests did not run the other time).

Since all our code is housed in a private repository and me not being the admin, I had to wait for the administrator to add the project repo to the build and allow collaborators to include it in their own project builds. 

## circle.yml

The major struggle is getting the correct circle.yml for a specified project. CircleCI runs the test in a container (much like a Docker container). For majority of the projects, CircleCI automatically creates containers and runs tests. We had a special case where majority of the setup was to be done carefully. 

[Manual configuration of circle.yml](https://circleci.com/docs/manually/) was the path we chose. For a python based project, CircleCI supports major releases in `2.7x` and `3.5x`. We went with `Python 3.5.2` (3.5.1 wasn't available, quite weird and the distributions are circle-python3.5.x format). The VM was `Ubuntu 14.04`.

The `circle.yml` file needs to be added to the root repository. We'll go stepwise to create a new  `circle.yml` file:

#### Step 1: <kbd>machine</kbd>

In order to set the specific python installation and assign global variables, `machine:` is the the parent which should have those keys. In our case, we had to specify `python version` to be used and create `$TF_BINARY_URL` (for TensorFlow installs). Our `machine:` would be as below:

```
machine:
  python:
      version: 3.5.2
  environment:
    TF_BINARY_URL: https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.10.0rc0-cp35-cp35m-linux_x86_64.whl
```

*How should it execute?*

![Machine](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/machine_key.png)

#### Step 2: <kbd>dependencies</kbd>

Now after python installation, we would need specific modules that we need. Since we already had generated `requirements.txt` file from the virtual environment, the `pip install -r requirements.txt` proved to be handy. 

__(NOTE: You might need to edit the `requirements.txt ` file if you are using Anaconda distribution and creating virtual environments using `conda create`. Remove `conda==<version>` and `tensorflow==<version>` specifically since it creates problems while setting up the CircleCI VM)__

There are a few dependencies that are `pre`, `post` and/or `override`, we just had to use `pre`.

```
dependencies:
  pre:
    - python --version
    - pip --version
    - pip install --upgrade pip
    - pip install -U $TF_BINARY_URL
    - pip install -r requirements.txt
```

*How should it execute?*

![Dependencies](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/dependencies_key.png)

#### Step 3: <kbd>database</kbd>

Databases are the most integral part of any end-to-end application and our application uses `mysql` as a database. In order to replicate the same set of tables and entries (to mock the test inputs), we ended up overriding the `circle_test` database i.e. default. 

We created a new user that matches the one in the tasks.py when we connect to our database and carefully generate it. Below is the way it should look:

```
database:
  override:
    - mysql -u ubuntu -e "CREATE USER 'user1'@'localhost' IDENTIFIED BY 'password';"
    - mysql -u ubuntu -e "GRANT ALL PRIVILEGES ON * . * TO 'user1'@'localhost';"
    - mysql -u ubuntu -e "FLUSH PRIVILEGES;"
    - mysql -h localhost -u "user1" --password="password" -e "create database athenadr;"
    - mysql -h localhost -u "user1" --password="password" -e "show databases;"
    - mysql -h localhost -u "user1" --password="password" -e "CREATE TABLE user1.users (id int(11) NOT NULL AUTO_INCREMENT,email varchar(100) NOT NULL,name varchar(100) DEFAULT NULL,tokens varchar(100) DEFAULT NULL,createdat timestamp NULL DEFAULT NULL,PRIMARY KEY (id),UNIQUE KEY email_UNIQUE (email),UNIQUE KEY uid_UNIQUE (id),UNIQUE KEY tokens_UNIQUE (tokens)) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8"
```

*How should it execute?*

![Databses](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/database_key.png)

#### Step 4: <kbd>test</kbd>

The final step is the one we are most interested in and should be looked at carefully. This is the place where we add the programming language specific test commands. These commands would trigger the testing modules specific to them. For our project we are using pytest, and a simple `pytest` would search for the tests in the directory structure to execute them. (CircleCI install `nose` module automatically for testing)

```
test:
  override:
    - pytest
```

*How should it execute?*

![Test](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/test_key.png)

***

## Success and How?

When you pass the tests, the checks get reflected on your PR (Pull Request) status too as below:

![Success](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/success.png)

My CircleCI dashboard for the curious.

![Build Status](https://raw.githubusercontent.com/pratos/pratos.github.io/master/images/circleciafterbuild.png)

To wrap this up, my test cases were pretty simple as well as the customizations required by my project were very limited. There's a lot that can be improved on this, lot of it might be sheer stupidity on my part (it works though). Comments and thoughts are welcomed.

#### Sources and Reading:
 - To know more about .yml format visit [this link](http://ess.khhq.net/wiki/YAML_Tutorial)
