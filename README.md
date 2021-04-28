# KommatiPara_APP

A very small company called KommatiPara that deals with bitcoin trading has two separate datasets dealing with clients that they want to collate to starting interfacing more with their clients. For this purpose, KommatiPara application has been created in order to enable the company to reach useful data for a new marketing push. The application conducts ETL process as follows:

* **Extract**: Reads two csv files from given paths and transform them into data frames.

* **Transform**: Filters one of the data frames according to selected countries, drops some columns which are unncessary for the company aim and renames columns for the easier readability to the business users. At the end, two data frames are joined according to the 'client_identifier' column. For the transformation process two generic functions which are 'filtering' and 'renaming' have been created.

* **Load**: Loads the output of the transformation to the given location.

In order to test data frame and column equalities in between the expected dataframe and transformed dataframes, **Chispa** method has been used. Application conduct out these tests successfully. Filtering and renaming functions work properly and generate expected results.

Application receives three arguments from the users which are:

1. The paths of the each datasets
2. Countries that users want to filter
3. Names that users want to give the columns

Additionally, the application keeps log records in order to search when unexpected problem occurs. Log records are kept and according to the rotating policy, the application rotates the log every day with a back up count of 5 

# Installation

## For pyspark

Spark is an open source project under Apache Software Foundation. Spark can be downloaded here: https://spark.apache.org/downloads.html

PySpark installation using PyPI is as follows: ```bash 
$ pip install pyspark ```


Import findspark from your notebook and initilaze it as follows:
```python
import findspark

findspark.init() 
```



## For chispa test

Install the chispa with: 
```python 
pip install chispa 
```

# Usage

##  Spark session

```python import pyspark
from pyspark.sql import SparkSession


spark = SparkSession \
    .builder \
    .master("local") \
    .appName("chispa") \
    .config("spark.some.config.option", "some-value") \
    .getOrCreate() 
```
    
## Chispa test 

```python 
from chispa.dataframe_comparer import *

import pytest

from chispa.column_comparer import assert_column_equality
```

## Configuration of logging

```python 
import logging
import time

from logging.handlers import TimedRotatingFileHandler

def logger():
    logger = logging.getLogger('<Give the name for the log file>')

    logHandler = TimedRotatingFileHandler(filename="<Give the name for the log file>", when="D", interval=1, backupCount=5)
    logFormatter = logging.Formatter('%(asctime)s %(name)-12s %(message)s')
    logHandler.setFormatter(logFormatter)
    logger.setLevel(logging.INFO)


    if not logger.handlers:
        streamhandler = logging.StreamHandler()
        streamhandler.setLevel(logging.INFO)
        formatter = logging.Formatter('%(asctime)s - %(name)s - %(message)s')
        streamhandler.setFormatter(formatter)

        logger.addHandler(streamhandler)
        logger.addHandler(logHandler)

    return logger 

logger = logger()
```

## CSV file

```python
import csv
```

# Challange

I have difficulty in importing functions from application file to test file. Jupyter notebook run the codes cell by cell. Therefore, whenever I call the function, it generates ERROR related to 'logging' because logging codes are in different cells. In order to solve this problem, I created logger function and send to the filtering function. Thus, whenever filtering function is called, logging function runs too. If someone has a different solution, please feel free to contribute to this project. 


# Contributing

It is open source project and anyone can contribute to this project. Any contributions you make are greatly appreciated.
