.. title: My Jupyter Notebook Slides Setup
.. slug: my-jupyter-notebook-slides-setup
.. date: 2018-02-26 16:46:37 UTC+08:00
.. tags: python, jupyter, public speaking, slides
.. category: 
.. link: 
.. description: 
.. type: text

Here's how I set up Jupyter notebook for presentations::

  pip install jupyter_contrib_nbextensions
  jupyter contrib nbextension install --sys-prefix
  jupyter nbextension enable splitcell/splitcell
  pip install rise
  jupyter-nbextension install rise -py --sys-prefix

I use Damian Avila's `RISE extension`_, which saves me from having to write any HTML or JS, allowing me to focus on the
code and figures. 

.. _Rise extension: https://damianavila.github.io/RISE/

