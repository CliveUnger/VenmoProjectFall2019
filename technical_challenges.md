# Technical challenges

Throughout our time working on this project, we have faced various technical challenges that slows down the process of doing actual data science and analytics that the lab cares about.
As a result, we have compiled a list of all the issues we faced on this project and how they were overcome.

###### Large data, but not Big Data:
The pain point comes from the fact that this dataset is 400GB. This is not large enough to be considered "Big Data" but still large enough to not fit in memory and not work with traditional "pandas" type operations (at least not without some hacking). The dataset is also slight incomplete. We are missing 2 months from the year 2018 due to issues with data collection.

###### Wrangler Decommissioned: No Hadoop:
The first roadblock of the semester was the fact that Wrangler, the TACC resource we were accustomed to. The caveat is that it not complete being decommissioned for a while, just some of the batch job resources and Hadoop. The inability to start jobs with more than one node, or a Hadoop cluster was an issue, because the plan was to use Apache Spark along with the Hadoop file system (HDFS) for efficient distributed computing. As a result, we had to settle for running Spark in local mode. This was still an advantage to running code on a single thread, because in local mode, PySpark can take advantage of all the machine cores, and if something runs out of memory it will be off loaded to the disk.

###### setting up SQL from scratch is hard:
Last semester we tried setting the data in a MySQL database, but I had limited knowledge on how to properly index each column and set up datatypes so that queries would be efficient. I ended up wasting a few days uploading the data, in hopes of easy data queries, only for that not to be the fact.
The failure to use MySQL and the slowness of going through the dataset line-by-line is why we turned to use Apache Spark. It's distributed computation basically allows use to load the data across multiple nodes/cores and treat it like a normal "pandas" data frame.

###### Python 3 and Spark 2.4 on TACC
Be default TACC uses python 2.7 and only had Spark <2.0 installed. We were used to programming with Python3 and use the newest version of Spark (2.4) for its easy data frame abstractions.
Getting this to work was not a trivial task. While python3 install was simple, we could no longer use the default visualization portal to run Jupyter notebooks. Therefore, we had to go through a hacky way to run Jupyter notebooks via a batch job.
Next, we had to install Spark 2.4 and make sure that python could access it in Jupyter. While this was resolved and documented [here](./TACC-Wrangler-Python-3-and-Spark-2-4-Setup.md) it took a while to google around and find the root of various problems.

###### Flattening the data
The next challenge we faced was the fact that each data entry was a json object with a nested structure, so accesses certain fields was a bit difficult. In addition, the schema was not consistent across entries.
We decided to alleviate this problem by flattening the schema of the dataset into a tabular schema. The initial idea was to convert it to a flat CSV, however we realized that the line numbers were not adding up and there were certain corrupt rows.
This is because the message field in transaction would sometimes have a newline character and Spark would misread it as a new entry. We tried various methods to try and get spark to read the CSV properly, as pandas can handle a CSV with such an issue.
In the end, we settled for storing it again as json, but with a flat, single level schema. This resulted in a larger file, but easier parsing.
We also had to decide if any of the json fields should be dropped. For example, we saw that the “via” field was also blank, so we just left it out. Also, since the fields comments, likes, and mentions were arrays of every user that commented, liked, or mentioned a post, we had to condense those fields down to simple represent the number of comments, likes, or mentions respectively.

###### Graph Representation
After wrangling the data and loading it with spark we needed to find a way to represent the data as a graph.
This was something that no one on the team had done before, so some basic research was done to understand the common standards. We decided on creating a method that could easily be read by Spark, as it has a graph processing API, GraphX. We created a node list and an edge list. The node list was every unique user we found in the dataset along with certain attributes such as username and account creation date. Next, since the original dataset is a list of transactions it is inherently an edge list. However, it would have duplicate edges for repeat payments between users. For example, if Alice pays Bob 10 times, that will be an edge in the dataset 10 times. As a result, we condense the data into a weighted edge list where the weight represents the number of times an interaction happens. The edge list included other relevant fields to each transaction such as the message and time. Next, since Venmo has both charges and payments we had to decide whether our graph should show interactions between “actors” and “targets” or flow of money. Since the Zhang paper uses the flow of money model, we swap the source and destination for any transactions that were marked as “charges”. This way the source is who is sending money and the destination is who is receiving money. Lastly, we saw that some users were making payments/charges to nonexistent users, resulting in nulls. We just removed these transactions from the dataset.
In many cases we did not need every field from the edge list, so we created an edge list only dataset that only had the source, destination, and weight.
One thing that was done to make some processing easier was to index the dataset by month. An edge list csv was created was created for each unique month in the dataset.

##### PySpark GraphFrames
While Spark has the GraphX API, it is only available in Scala, and we are using Python 3. As a result we have to use another package called [GraphFrames](https://graphframes.github.io/graphframes/docs/_site/index.html). This package exposes the graphX api to python as well as adds additional functionality, primarily the ability to view graphs in a dataframe.
This package is great for basic graph queries however falls shorts for more advanced graph statistics.
###### Clustering Coefficient
One of the first statistics I tried calculating was the clustering coefficient of the graph. Since GraphFrames doesn’t have a built in function for this, I had to implement it myself. I follow [this formula]( https://networkx.github.io/documentation/stable/reference/algorithms/generated/networkx.algorithms.cluster.clustering.html#networkx.algorithms.cluster.clustering) for a directed graph. I consistently got numbers lower than the zhang paper, so suspected that something must be wrong. The formula seemed correct but it’s possible zhang used some other formula.
Through this process I realized that I might be able to utilize other graph processing libraries since the edge list by itself (just src,dst,wght) was only 15GB which could easily fit in TACC memory.

#### Graph Libraries
###### [NetworkX](https://networkx.github.io/documentation/stable/index.html)
Single threaded application built on Python, so not memory efficient. Unable to load the entire dataset into memory. Potential use cases for smaller graphs.

###### [SNAP](http://snap.stanford.edu/)
Super fast graph processing. Built on C++ with Python API. Only downside is no easy way to represent weights.

###### [NetworKit](https://networkit.github.io/)
Another fast C++ library with a Python API. Integrates well with NetworkX. Weird issue on TACC where is causes a SegFault when run as a script. Only use it in Jupyter notebook.
Has Louvain community detection but only for undirected graphs. In fact, many of its functions only work with undirected graphs.

###### Louvain on directed
The currently challenge we are working on is trying to get Louvain community detection to work on a directed graph.
