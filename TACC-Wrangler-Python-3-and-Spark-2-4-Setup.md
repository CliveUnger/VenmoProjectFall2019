# TACC Wrangler Python 3 and Spark 2.4 Setup

## Python 3 on TACC

First let's get Python 3 running on Wrangler. TACC uses this "modules" system you can read about [here](https://portal.tacc.utexas.edu/software/modules)

Run:

    module load intel/18.0.2
    module load python3
    module save # this makes sure the modules will load every time you login
    
    # Then run this to ensure its installed correctly
    python3 --version

 Try logging out and back in to make sure the settings persist. If they do not then add the above load statements to the ".modules" file.

## Spark 2.4

On Wrangler, First install Spark 2.4.4 from [https://spark.apache.org/downloads.html](https://spark.apache.org/downloads.html). Be sure to select "Pre-built for Apache Hadoop 2.6". This is because TACC has Hadoop 2.6 rather than 2.7. Place this file somewhere in your home directory.

Extract the file with "tar xvf spark-XXX.tgz"

Next we need to set some environment variables in your .bashrc. This is the file that is run every time your log in to a Unix terminal.

    cd $HOME
    vim .bashrc
    
    # Add the following lines under SECTION 2 of the .bashrc
    unset XDG_RUNTIME_DIR # this is used later for juptyer notebook
    export SPARK_HOME=$HOME/pathToSparkDirectory/spark-2.4.4-bin-hadoop2.6
    export PATH=$SPARK_HOME/bin:$PATH # add Spark to PATH
    export PYSPARK_PYTHON=python3 # tell spark to use python 3

    # Now Run
    pyspark
    
    # It should output:
    Python 3.7.0 (default, Feb  6 2019, 21:24:19)
    [GCC Intel(R) C++ gcc 6.3 mode] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    19/09/29 14:07:05 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
    Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
    Setting default log level to "WARN".
    To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
    Welcome to
          ____              __
         / __/__  ___ _____/ /__
        _\ \/ _ \/ _ `/ __/  '_/
       /__ / .__/\_,_/_/ /_/\_\   version 2.4.4
          /_/
    
    Using Python version 3.7.0 (default, Feb  6 2019 21:24:19)
    SparkSession available as 'spark'.

Notice the output says Python 3.7 and Spark 2.4.4

## Jupyter Notebook with Python 3

So this is where things will get a bit hacky, but it works great. Basically we can't use the visualization portal UI to start a jupyter notebook with python 3.

First check if you have juptyer installed with: "jupyter notebook"

If not then install it with "pip3 install jupyter -user"

    cd $HOME
    # Now we need to submit a job for a jupyter notebook
    sbatch -A Venmo-Project /share/doc/slurm/job.jupyter
    # You might want to copy the above script to your home directory so you can edit settings
    
    # wait for a minute then
    cat jupyter.out
    
    # There should be a URL at the very bottom line to open your notebook in

    # Other useful commands
    
    # List Submitted Jobs
    squeue -u yourusername
    
    # Kill Jobs when you are finished with them, save resources
    scancel jobnumber

## Test Everything

See if you can run this. Change file paths to any text file. It should count line numbers.

[PySparkTesting.ipynb](PySparkTesting-896b0f0a-a73f-4003-8626-8bea283c38cf.ipynb)