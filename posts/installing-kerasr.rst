.. title: Installing kerasR
.. slug: installing-kerasr
.. date: 2017-06-10 08:43:10 UTC+08:00
.. tags: machine learning
.. category: 
.. link: 
.. description: 
.. type: text

This is a quick reference to installing kerasR, a slim wrapper around Keras_ starting with the required Python packages. 

.. _Keras: https://keras.io/

Python packages
---------------
Create a virtualenv::

  $ virtualenv pydata --python=/usr/bin/python3
  Running virtualenv with interpreter /usr/bin/python3
  Using base prefix '/usr'
  New python executable in /home/brian/pydata/bin/python3
  Also creating executable in /home/brian/pydata/bin/python
  Installing setuptools, pip, wheel...done.
  $ source pydata/bin/activate
  (pydata) $ 

Install ``keras``. This will also install the other prerequisites for doing any sort of datasciency stuff in Python 
(``numpy``, ``pandas``) as well as Theano. Tensorflow will be installed in the next step.

::

  (pydata) $ pip install keras
  Collecting keras
  Collecting six (from keras)
  Using cached six-1.10.0-py2.py3-none-any.whl
  Collecting theano (from keras)
  Collecting pyyaml (from keras)
  Collecting scipy>=0.14 (from theano->keras)
    Downloading scipy-0.19.0-cp35-cp35m-manylinux1_x86_64.whl (47.9MB)
      100% |████████████████████████████████| 47.9MB 27kB/s 
  Collecting numpy>=1.9.1 (from theano->keras)
    Downloading numpy-1.13.0-cp35-cp35m-manylinux1_x86_64.whl (16.9MB)
      100% |████████████████████████████████| 16.9MB 66kB/s 
  Installing collected packages: six, numpy, scipy, theano, pyyaml, keras
  Successfully installed keras-2.0.4 numpy-1.13.0 pyyaml-3.12 scipy-0.19.0 six-1.10.0 theano-0.9.0

Install Tensorflow::

  (pydata) $ pip install tensorflow


kerasR
------
In R, install the ``kerasR`` package::

  > install.packages("kerasR")
  Installing package into ‘/home/brian/R/x86_64-pc-linux-gnu-library/3.4’
  ...
  ** testing if installed package can be loaded
  successfully loaded keras
  * DONE (kerasR)

This may also install the ``reticulate`` package, which is an interface to Python objects and methods.

A guide_ to using ``kerasR`` is provided as a vignette.

Troubleshooting
---------------
If you get an error message when executing ``library(kerasR)`` saying::

  > library(kerasR)

  keras not available
  See reticulate::use_python() to set python path, 
  then use kerasR::keras_init() to retry

this means ``kerasR`` (or more specifically, ``reticulate``) can't find the ``keras`` python package, you need to start R *after* loading your virtualenv::

  $ source pydata/bin/activate
  (pydata) $ R
  > library(kerasR)
  Using TensorFlow backend.
  successfully loaded keras
  >

.. _guide: https://cran.r-project.org/web/packages/kerasR/vignettes/introduction.html

