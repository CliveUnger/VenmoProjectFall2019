# The Dataset


We have ~350 million Venmo transactions from 2012 to 2018. The entire dataset is about 400GB, but we have split and manipulated the data in various ways to make it easier to work with. This document is meant to point to all the different data subsets we have created and why.

### Original data:
```/corral-repl/utexas/Trump-Tweets/Venmo/ut_venmo_2018.json```

The original dataset, untouched. Each line is a JSON object that represents a single transaction.
There is a single line that is corrupt as it does not follow the proper schema.

### Flattened data:
```/corral-repl/utexas/Trump-Tweets/Venmo/fall19/ut_venmo_2018_flat.json```

The original dataset has many nested fields that can be hard to work with in a tradition data table/dataframe format.
Therefore we flattened the schema of the dataset to a single level. As a result some fields were dropped or edited.

### One million rows flattened
```/data/06271/cju256/one_mil_flat.json```

This is the first on million rows from the flatten data. Intended for faster processing on a small set before trying on the entire dataset.

### 10k rows Flattened
```/data/06271/cju256/ten_k_flat.json```

A random sample of 10,000 rows from the flattened data. Again used for testing, before running code on entire dataset.

### Nodes
```/data/06271/cju256/nodes.json```

All unique users in the dataset. Includes user attributes.

### Edges
```/data/06271/cju256/edges.json```
All edges in the dataset. Includes edge attributes such as message and time.


### Edge List only
```tbd```

### Edges Indexed by Month
```/data/06271/cju256/data_by_month/```
A directory of edge lists separated by month. Each JSON file is inside of a directory with the month name.
For example the full path of the edge list for 2017-09 is ```/data/06271/cju256/data_by_month/2017-09/part-00000-18fac98b-849b-469d-b951-8a32ace8c3e8-c000.json```

### Weighted edge list
```/data/06271/cju256/edges.json```

### Money flow edge List
```/data/06271/cju256/money_flow_edgelist.json```

### money flow weighted edge list
```/data/06271/cju256/money_flow_weighted_edgelist.json```

This is the best dataset to use to be most accurate to the Zhang paper.
