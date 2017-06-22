---
layout: post
published: true
title: Installing Spark on Windows and Mac
---

>As a Windows user who does machine learning (do not judge me) there’s always a struggle to find some or the other things working on your “beloved” system.
I have a very minimal configuration packed in a light weight Asus case which has survived about a year of abuse, yet still strong to tell the tale. In this edition we would be looking at how to get up and running with Stand Alone Spark 2.0 (and later) on Windows and Mac OS.

## For Windows 10 (Works for Windows 7 too)

### Java

Download & Install Java (latest version) through https://www.java.com/en/download/

Add JAVA_HOME Path in System Variables.

```
C:\Users>java -version
java version “1.8.0_91”
Java(TM) SE Runtime Environment (build 1.8.0_91-b15)
Java HotSpot(TM) 64-Bit Server VM (build 25.91-b15, mixed mode)
```
***

### Scala
Download and Install Scala (latest version) through http://www.scala-lang.org/download/

Directly add <Scala location>\bin to the Path.

```
C:\Users>scala -version
Scala code runner version 2.10.5 — Copyright 2002–2013, LAMP/EPFL
```
***

### Spark
We’ll be using Spark prebuilt package for Hadoop 2.6 (later can be used too).

Download Spark through this link:: https://spark.apache.org/downloads.html

Extract the files to a folder (it can be your choice of folder)

Add the folder path to SPARK_HOME (eg: SPARK_HOME = D:\Hadoop\spark-2.0.1-bin-hadoop2.6) to System Variables as “%SPARK_HOME%\bin”.

We need to download the ‘winutils.exe’ file and place it in a location (Link to download: https://github.com/steveloughran/winutils, clone the entire folder in your local system and unzip).

Provide the path for HADOOP_HOME = “Location for winutils files” and add to system variables as “%HADOOP_HOME%\bin”.

```
C:\Users>spark-shell
Using Spark’s default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to “WARN”.
To adjust logging level use sc.setLogLevel(newLevel).
16/11/20 12:05:49 WARN SparkContext: Use an existing SparkContext, some configuration may not take effect.
Spark context Web UI available at http://192.168.59.1:4040
Spark context available as ‘sc’ (master = local[*], app id = local-1479623748733).
Spark session available as ‘spark’.
Welcome to
 ____ __
 / __/__ ___ _____/ /__
 _\ \/ _ \/ _ `/ __/ ‘_/
 /___/ .__/\_,_/_/ /_/\_\ version 2.0.1
 /_/
Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_91)
Type in expressions to have them evaluated.
Type :help for more information.
scala>
```

You can run various shells which includes the ones for Python (pyspark) and R Programming (SparkR) which will be helpful for Data Science related tasks.

***

### Spark API and connecting it with Jupyter Notebooks
You can run Python API on Spark by typing “pyspark” in the command prompt.

You’ll get a message → Using Python version 2.7.11 (default, Feb 16 2016 09:58:36) SparkSession available as ‘spark’. which confirms the pyspark shell is running properly.
To connect it to the Jupyter Notebooks we need to set a few Paths in our Windows environment:

```
PYSPARK_DRIVER_PYTHON = ipython
PYSPARK_DRIVER_PYTHON_OPTS = notebook
Run the below command to fire up the pyspark shell and make the spark context available directly in IPython Notebook.

C:\Users\prato_s>pyspark
 [TerminalIPythonApp] WARNING | Subcommand `ipython notebook` is deprecated and will be removed in future versions.
[TerminalIPythonApp] WARNING | You likely want to use `jupyter notebook`… continue in 5 sec. Press Ctrl-C to quit now.
[I 12:10:43.302 NotebookApp] The port 8888 is already in use, trying another random port.
[I 12:10:43.397 NotebookApp] Serving notebooks from local directory: C:\Users\prato_s
[I 12:10:43.397 NotebookApp] 0 active kernels
[I 12:10:43.398 NotebookApp] The Jupyter Notebook is running at: http://localhost:8889/
[I 12:10:43.398 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
```

We can always create and import the pyspark module & sqlContext for our needs.


## For MacOS

I had a chance to setup Spark on MacBook Pro along with my colleague, so this is the documentation for that. Better to start installing with Brew, this makes life easy.

Open the link (http://spark.apache.org/downloads.html) and download it in the folder you wish (in our case it was ~/Downloads)

Go to bash and execute the following the statements:

```
$ tar xvzf spark-2.0.1-bin-hadoop2.7.tgz 
```

Unzip the downloaded folder

```
mv spark-2.0.1-bin-hadoop2.7 spark2 
```

Change the filename to spark2

```
cd spark2/sbin 
```

Go to the sbin folder. Open the start-master.sh file. This will start Spark.

```
./start-master.sh
cd ../logs 
ls #(should show you a link) 
tail link #(as given above)
```

The last step will show you that spark as started and give you web link to check the Spark web interface. Spark is successfully installed here.

Unlike Windows, we’ll be using the module “findspark” to connect to our spark instance. Install the module through pip.

```
pip install findspark
```

Fire up the jupyter notebook from bash and write the below code to create spark context:

```
import findspark 
findspark.init(“/Users/user1/Downloads/spark2/”)
import pyspark 
from pyspark.sql import DataFrameNaFunctions 
from pyspark.sql.functions import lit 
from pyspark.ml.feature import StringIndexer #label encoding 
from pyspark.ml import Pipeline 
sc = pyspark.SparkContext(appName=”helloworld”)
```

You should be able to connect! Hope this is a nice brief documentation about how to setup and anyone following this should be able to setup Stand-alone Spark on their systems properly.
