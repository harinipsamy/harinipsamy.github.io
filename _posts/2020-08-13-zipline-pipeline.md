---
title: "Zipline - an Open source trading simulator by Quantopian"

---

Zipline is an open source trading simulator by Quantopian. It can be used to backtest financial models offline. 

 It is built on python so it is very easy to look at the source modules in case you run into some issues while implementing your model. 
 
I had a difficult time installing zipline. Zipline supports 

# Installation 

There's a simple pip install and also option to install using conda install, if you are using Anaconda. 
Installation instructions can be found here:
https://www.zipline.io/install.html

I did create an environment for installing zipline, to ensure smooth funcitioning of the library package and to avoid conflict with other site-packages, although that's my usual practice for working on any project. Although zipline says it is supported in python versions 2.7 and 3.5, I couldn't install it in an environment with python=3.5. Ultimately, I was able to install it in an environment with python=2.7. If you are using a linux system like me, there's additional installations involved which can all be done in one line of code.

# First program

Zipline's beginner tutorial is a great place to start, if you want to use zipline to backtest your data using python.
https://www.zipline.io/beginner-tutorial

Before building the pipeline, we need to first download data bundle in command line. There are default bundles available through ziplien like 'quandl' and 'quantopian-quandl', but it is also possible to ingest custom data bundle. 

Once the data is downloaded in command-line terminal, you can view if the bundles exist in in python (I am using Ipython kernel using Jupyter notebook) by executing:

```
import zipline
zipline.data.bundles.bundles
```

Once the data bundle is ingested, we need to register the bundle:
```
ingest_func = bundles.csvdir.csvdir_equities(['daily'], 'quantopian-quandl')
bundles.register('quantopian-quandl', ingest_func)
```
Don't forget to set your environment path to include the the path where the bundle is located, or the library won't be able to locate the data:

```os.environ['ZIPLINE_ROOT'] = os.path.join('path/to/data/bundle')```

There you go! You have just run your first algorithm using zipline. 












