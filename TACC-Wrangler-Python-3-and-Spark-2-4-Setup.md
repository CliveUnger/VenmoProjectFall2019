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

## Jupyter Notebook with Python 3

Normally we use juptyer notebook on TACC by logging in via the visualization portal (https://vis.tacc.utexas.edu/). However, this does not have support for Python 3, so we have to access jupyter via a batch job.


First check if you have juptyer installed with: "jupyter notebook"

If not then install it with "pip3 install jupyter -user"

Next follow these commands:

    cd $HOME
    # Now we need to submit a job for a jupyter notebook
    # TACC has a premade script for this in the /share/doc/slurm folder
    # You might want to copy the script to your home directory so you can edit settings
    sbatch -A Venmo-Project /share/doc/slurm/job.jupyter

    # The notebook takes a minute to get started, so wait a bit then run
    cat jupyter.out

    # There should be a URL at the very bottom line to open your notebook in

Expected output of jupyer.out:

    job 252658 execution at: Tue Nov 12 09:44:03 CST 2019
    TACC: running on node c251-102
    TACC: memory limit set to 125036013 kilobytes
    TACC: using jupyter notebook binary /opt/apps/intel18/python3/3.7.0/bin/jupyter-notebook
    TACC: using jupyter command: /opt/apps/intel18/python3/3.7.0/bin/jupyter-notebook --port 5902 --ip=0.0.0.0 --no-browser --config=/home/00832/envision/tacc-tvp/server/scripts/stampede2/jupyter/jupyter.tvp.config.py --certfile=/home/00832/envision/.viscert/vis.2015.04.pem
    perl: warning: Setting locale failed.
    perl: warning: Please check that your locale settings:
        LANGUAGE = (unset),
        LC_ALL = (unset),
        LANG = "C.UTF-8"
    are supported and installed on your system.
    perl: warning: Falling back to the standard locale ("C").
    TACC: got login node jupyter port 53102
    TACC: created reverse ports on Wrangler login
    Your jupyter notebook server is now running!
    Please point your favorite web browser to https://vis.tacc.utexas.edu:53102/?token=7c04bc88e11153fc60213faa628f3306866114d3b80329a4

  ### Other useful commands

    # List Submitted Jobs
    squeue -u yourusername

    # Kill Jobs when you are finished with them, this saves resources
    scancel jobnumber


  ## Spark 2.4

  On Wrangler, First install Spark 2.4.4 from [https://spark.apache.org/downloads.html](https://spark.apache.org/downloads.html). Be sure to select "Pre-built for Apache Hadoop 2.6". This is because TACC has Hadoop 2.6 rather than 2.7. Place this file somewhere in your home directory.

  Extract the file with "tar xvf spark-XXX.tgz"

  Next we need to set some environment variables in your .bashrc. This is the file that is run every time your log in to a Unix terminal.

      cd $HOME
      vim .bashrc

      # Add the following lines under SECTION 2 of the .bashrc
      unset XDG_RUNTIME_DIR # this is used later for juptyer notebook
      export SPARK_HOME=$HOME/YOURpathToSparkDirectory/spark-2.4.4-bin-hadoop2.6

      # add Spark to PATH
      export PATH=$SPARK_HOME/bin:$PATH

      # tell spark to use python 3
      export PYSPARK_PYTHON=python3

-
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


Lastly in order for Jupyter Notebook to access pyspark we need to install a python module called "findspark".
Run: ```pip3 intall findspark```. More information [here.](https://www.sicara.ai/blog/2017-05-02-get-started-pyspark-jupyter-notebook-3-minutes)

At the start of any notebook you want pyspark on run:

    import findspark
    findspark.init()

## Test Everything

See if you can run this. Change file paths to any text file. It should count line numbers.

[PySparkTesting.ipynb](notebooks/PySparkTesting.ipynb)
