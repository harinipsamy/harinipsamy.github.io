---
title: "Implementing simple dual moving average algorithm using Zipline"
tags:
   - Data Science
   - Quantitative Finance
   - AI in Investing
   - Zipline
   - Quantopian
   - Stock market
---


Zipline is an open source trading simulator by Quantopian. It can be used to backtest financial models offline. 

It is built on python dataframes so it is easy to integrate with the existing python libraries.
 
I had a difficult time installing zipline, but it was fulfilling when I finally succeeded and ran my first algorithm. 

# Installation 

There's a simple pip install and also option to install using conda install, if you are using Anaconda. 
Installation instructions can be found here:
[https://www.zipline.io/install.html](https://www.zipline.io/install.html)

It is generally considerd good practice to create an environment for each project, to ensure smooth funcitioning and to avoid conflict with other site-packages. Even though zipline says it is supported in python versions 2.7 and 3.5, I couldn't install it in an environment with python=3.5. Ultimately, I was able to install it in an environment with python=2.7. If you are using a linux system like me, there's additional installations involved which can all be done in one line of code.

# First algorithm

Zipline's beginner tutorial is a great place to start, if you want to use zipline to backtest your data using python.
[https://www.zipline.io/beginner-tutorial](https://www.zipline.io/beginner-tutorial)

Before building the pipeline, you need to first download data bundle in command line. There are default bundles available through zipline like 'quandl' and 'quantopian-quandl', but it is also possible to ingest custom data bundle. 

Once the data is downloaded in command-line terminal, you can view if the bundles exist in python (I am using Ipython kernel using Jupyter notebook) by executing:

```python
import zipline
zipline.data.bundles.bundles
```

To run the algorithm in the beginner tutorial, there's no need to register the data bundle. All you need to do is run the code. It might be worth to note that if you downloaded the bundle with 'no benchmark', then you also need to change the magic line to include no benchmark, else, it will throw an error.

Change: 
```python 
%%zipline --start 2016-1-1 --end 2018-1-1
```

to:

```python 
%%zipline --start 2016-1-1 --end 2018-1-1 --no-benchmark
```

Also, the data in the examples are not updated after about March 2018. So, to explore the functionality using the library, its better to stick to dates on or before March 2018. 

# Custom data bundle

If you intend to use custom data bundle, there are a few extra steps you need to do.

Once the data bundle is ingested, it needs to be registered:
```python
ingest_func = bundles.csvdir.csvdir_equities(['daily'], 'custom-data-bundle')
bundles.register('custom-data-bundle', ingest_func)
```
Don't forget to set your environment path to include the path where the bundle is located, or the library won't be able to locate the data:

```python 
os.environ['ZIPLINE_ROOT'] = os.path.join('path/to/data/bundle')
```


There you go! You have just run your first algorithm using zipline. 

The full code can be found here: [https://github.com/harinipsamy/Factor-Model/blob/master/first_algorithm_zipline.ipynb](https://github.com/harinipsamy/Factor-Model/blob/master/first_algorithm_zipline.ipynb)
