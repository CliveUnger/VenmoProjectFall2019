# The Dataset


We have ~350 million Venmo transactions from 2012 to 2018. The entire dataset is about 400GB, but we have split and manipulated the data in various ways to make it easier to work with. This document is meant to point to all the different data subsets we have created and why.

### Original data: ```/corral-repl/utexas/Trump-Tweets/Venmo/ut_venmo_2018.json```

The original dataset, untouched. Each line is a JSON object that represents a single transaction.
There is a single line that is corrupt as it does not follow the proper schema.

### Flattened data: ```/corral-repl/utexas/Trump-Tweets/Venmo/fall19/ut_venmo_2018_flat.json```
The original dataset has many nested fields that can be hard to work with in a tradition data table/dataframe format.
Therefore we flattened the schema of the dataset to a single level. As a result some fields were dropped or edited.

### One million rows flattened
This is the first on million rows from the flatten data. Intended for faster processing on a small set before trying on the entire dataset.

### 10k rows Flattened
A random sample of 10,000 rows from the flattened data. Again used for testing, before running code on entire dataset.

### Nodes

### Edges

### Edge List only

### Edges Indexed by Month

### Weighted edge list

### Money flow edge List

### money flow weighted edge list
