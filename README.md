# Venmo Project Fall 2019

This repo serves as reference and knowledgebase for the work completed during the Fall of 2019 by Clive Unger, Ishank Arora, and Andy Chang in the UT Computational Media Lab

## Getting Started
[Setting up Python 3, Jupyter Notebook, and Spark 2.4 on TACC](./TACC-Wrangler-Python-3-and-Spark-2-4-Setup.md)

[Running a batch job on TACC](https://portal.tacc.utexas.edu/user-guides/stampede2#slurm-job-scheduler)
(This documentation is for Stampede2, but the same process applies for any TACC resource)

## Challenges
The dataset is large enough that it can't be fit into memory, so there are many challenges that come with that. We have written up a comprehensive log of the various challenges we faced and how they were resolved.
In addition, we also had various logistical challenges that we thought should be mentioned.

[Technical Challenges](./technical_challenges.md)

[Logistical Challenges](./logistical_challenges.md)

## Data
Reference to the data locations in TACC since data cannot be uploaded to GitHub

[The Data](./data.md)

## Notebooks
* Loading the Data in Spark
* Flattening the Data
* Creating a Graph Representation of the Data
* Basic Graph Manipulation with Spark and Graphframes
* Clustering Coefficient example (which turned out to be incorrect)
* Splitting the data per month
* SNAP
* NetworKit

## Useful Links
* [TACC Wrangler User Guide](https://portal.tacc.utexas.edu/user-guides/wrangler)
* [Apache Spark Documentation](https://spark.apache.org/docs/latest/)
* [GraphFrames Documentation](https://graphframes.github.io/graphframes/docs/_site/index.html)
* [NetworkX Documention](https://networkx.github.io/documentation/stable/index.html)
* [SNAP Documentation](http://snap.stanford.edu/)
* [NetworKit Documentation](https://networkit.github.io/)
